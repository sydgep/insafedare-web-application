# Intelligent Recommender Evaluation Guide

This document explains how to test, use, and evaluate the intelligent recommender system. The recommender is an LLM-based assistant, embedded in the INSAFEDARE web application, that
reviews synthetic-data pipelines and assurance cases and suggests improvements.

---

## Starting the application

Start the application like this:

```bash
java -jar insafedare-web-application-<version>.jar --insafedare.evaluation.enabled=true
```

The `--insafedare.evaluation.enabled=true` flag turns on evaluation mode. It adds two things to
the chat view that you will use during the review: a scoring panel under each answer, and an
"export evaluation bundle" button. The application works fine without
the flag, but collecting your results is much easier with it, so please keep it on.

The first start takes a few extra minutes because the application builds a semantic search
index over its safety ontology, using the embedding model. Ollama must already be running at
that point. Later starts reuse the saved index (it lives under `~/.insafedare/`).

Once the log settles down, open http://localhost:8080 in a browser.
### Quick health check

- `curl -s http://localhost:8080/api/insafedare/evaluation/status` returns `{"enabled":true}`
  when evaluation mode is on.
- `curl -s http://localhost:11434/api/ps` shows which model is loaded (after your first
  question). The `size_vram` field tells you how much of the model sits on the GPU.

## Reaching the recommender

1. In the workbench, look at the right sidebar. The last icon is the **Recommender** chat view. Click it to open the panel.
2. To give the recommender something to look at, select the pipeline (or assurance) in the explorer on the left and open its diagram. The recommender reasons about the models
   in the currently selected document.

### What is in the chat view

- **Conversation list**: create, rename, and delete conversations. Conversations are saved and
  can be reopened later.
- **Message box with send and stop buttons**: ask questions in natural language, answers
  stream in live. The stop button aborts a generation.
- **Context toggle chips**: these control what the recommender sees (the metamodel vocabulary,
  the safety ontology, the graph and vector search backends). Leave everything on unless a
  scenario says otherwise.
- **Document upload (paperclip)**: attach a text or markdown file to the conversation. Its
  content is given to the model as extra context.
- **Citations panel**, shown under an answer: the ontology concepts the answer references,
  with the retrieval tier they came from. This is your "where did that come from" signal.
- **Hallucination warning banner**: appears when the answer mentions a node type that does not
  exist in the modelling language. If you ever see one, write it down. It means the system
  caught the model inventing vocabulary, and that is a real finding.
- **Truncation banner**: appears if the model ran out of context window in the middle of an
  answer.
- **Rubric feedback panel** (evaluation mode only, under each answer): the scoring panel,
  described in section 7.
- **Export evaluation bundle button** (evaluation mode only, at the top of the panel):
  packages your whole session for sending back, described in section 8.

### How the recommender works, in 30 seconds

The recommender is not a bare LLM. Before the model is
called, deterministic code inspects your actual diagram. Around 27 hand-written rules
(SHACL/SPARQL) detect structural problems in the pipeline (missing evaluations, disconnected
nodes, missing merge steps, and so on) and in the assurance case (unsupported goals, orphan
elements). These findings, together with retrieved safety-ontology context and the actual
content of your diagram, are injected into the prompt. The LLM's job is to explain and
prioritise these grounded facts, and to add its own judgement on top. It is not supposed to
guess the structure. Your job as a reviewer is to judge how well it does that.

##  Warm-up (10 minutes, not scored)

Before scoring anything, get a feel for the tool:

1. Open the pipeline diagram and add a single **FileExtraction** node.
2. In the Recommender panel, create a new conversation and ask: *"What do you see in my
   pipeline?"*
3. Watch the answer stream in. Open the citations panel. Ask one follow-up question.

If answers take longer than about 5 minutes, jump to the troubleshooting section (section 10).
Your model is probably running on CPU.

## Evaluation scenarios

Each scenario tells you (a) what to build in the diagram, (b) what to ask (please use the
exact question), and (c) what to look for. After each answer, score it with the rubric panel
(section 7) before moving on. Use one conversation per scenario and name it after the
scenario, so the exported bundle stays organised.

To build the models: add nodes of the named types from the diagram palette and connect them in
the order given. Attribute values are set in the Details view while the node is selected.

### Scenario 1: minimal pipeline, what next?

- **Build**: a single **FileExtraction** node. Set its `inputFile` attribute, for example to
  `patients.csv`. Nothing else.
- **Ask**: *"What should I add next?"*
- **Look for**: the deterministic rules will report that the pipeline has no synthesizer and is
  very small. A good answer explains those findings and points you at a concrete next step, a
  preprocessing step or a synthesizer node, by name. A weak answer is generic advice that
  would fit any pipeline.

### Scenario 2: generator without evaluation, is it safe?

- **Build**: **FileExtraction** (`inputFile=patients.csv`), then **DropNan**, then
  **CtganSynthesizer**, connected in that order.
- **Ask**: *"Is this pipeline ready to generate safe synthetic data?"*
- **Look for**: the rules flag the missing evaluation stages. The answer should clearly say
  no, and it should name the missing node types explicitly, in particular
  **DistanceToClosestRealRecord** (or another privacy evaluation) and
  **StatisticalEvaluation**, because the deterministic findings recommend exactly those. Also check the citations panel: privacy and fidelity related
  ontology concepts should show up. This scenario is the best single probe of whether the
  model relays its grounded findings faithfully.

### Scenario 3: complete pipeline, does it recognise good work?

- **Build**: extend scenario 2 into a full pipeline. **FileExtraction**, then **DropNan**,
  then **CtganSynthesizer**, and add three evaluation nodes in parallel:
  **DistanceToClosestRealRecord**, **StatisticalEvaluation**, and **UtilityEvaluation**. Each
  evaluation node compares real and synthetic data, so connect both the DropNan output and the
  synthesizer output to each of them. On the UtilityEvaluation node, set `targetCol` to
  something like `outcome`.
- **Ask**: *"Does this pipeline cover the main evidence needs?"*
- **Look for**: there is little to criticise here. The rules even produce positive
  confirmations for a complete evaluation suite. A good answer acknowledges that the pipeline
  is well formed, without inventing problems. If the answer manufactures severe-sounding
  issues for a healthy model, score "correct" and "no unsupported claims" accordingly.

### Scenario 4: disconnected evaluation, does it spot meaningless metrics?

- **Build**: take scenario 3 and delete the connection between the synthesizer and the
  **DistanceToClosestRealRecord** node. Leave the node in place, just disconnected (or connected only on
  the raw input side).
- **Ask**: *"Why are the privacy metrics produced by this pipeline not meaningful?"*
- **Look for**: the rules detect that a privacy evaluation exists but is not downstream of the
  synthesizer. The answer should explain why that matters (privacy metrics have to be computed
  on the generated output, so the node must be reachable from the synthesizer) and say how to
  fix the wiring. Check that the wording actually matches your diagram and uses the right node
  names.

### Scenario 5: two sources, no merge

- **Build**: a fresh pipeline. **FileExtraction** (`inputFile=a.csv`) and
  **DatabaseExtraction**, both connected directly into one **CtganSynthesizer**, with no merge
  step in between.
- **Ask**: *"Is my data preparation sound?"*
- **Look for**: the rules flag multiple sources feeding the synthesizer without a **MergeData**
  step. After scoring the answer, add a **MergeData** node (set `mergeOn`, for example to
  `subject_id`) between the two sources and the synthesizer, ask the same question again in
  the same conversation, and check that the finding is gone and the answer changes to match.
  This before-and-after is a good test of whether answers track the actual model state.

### Scenario 6: assurance case, unsupported goal

- **Build**: switch to the assurance case document and its diagram. Add a single **Goal**
  named `G_Top`, with nothing supporting it.
- **Ask**: *"Is my assurance argument complete?"*
- **Look for**: the GSN rules flag a goal that has no supporting Strategy or Solution (and an
  orphan element). The answer should explain what a complete argument needs, using valid GSN
  next steps (a **Strategy** decomposing the goal, or a **Solution** providing evidence) and
  correct GSN terminology, not pipeline vocabulary.

### Scenario 7: assurance case, solution without real evidence

- **Build**: extend the case into a chain: **Goal** `G_Privacy`, supported by **Strategy**
  `S_Argue`, supported by **Solution** `Sol_Stat`. Do not link anything to pipeline elements.
- **Ask**: *"Does the solution Sol_Stat provide enough evidence for the privacy goal?"*
- **Look for**: this one is interesting because the deterministic rules find nothing wrong.
  Structurally the chain is fine. Judging whether `Sol_Stat` is actually concrete evidence is
  left entirely to the model. A good answer notices that the solution is not backed by
  anything concrete (no pipeline link, no artefact) and says so. A weak answer waves it
  through because the structure looks complete. Score "correct" and "unsupported claims"
  strictly here.

### Scenario 8: attached requirements document

- **Build**: keep scenario 7's assurance case, plus the scenario 2 pipeline. On your machine,
  create a small markdown file called `requirements.md` with five requirements, for example:

  ```
  REQ-1: Synthetic data must preserve patient privacy.
  REQ-2: The generation process must be documented.
  REQ-3: Input data must be quality-checked before generation.
  REQ-4: Re-identification risk must be measured.
  REQ-5: Utility must be demonstrated by training models on synthetic data (TSTR).
  ```

- **Do**: attach the file with the paperclip button, then **ask**: *"Which of the attached
  requirements are not yet covered by this assurance case?"*
- **Look for**: the answer should engage with the document explicitly, referencing the `REQ-`
  identifiers, and correctly identify the gaps. With this setup, essentially all requirements
  lack assurance-case coverage; REQ-4 and REQ-5 have partial pipeline support at best.

### Scenario 9: ablation, what does the ontology add?

- **Do**: go back to the scenario 2 pipeline and conversation. Ask the scenario 2 question
  once more. Then turn the ontology toggle off in the context chips and ask the exact same
  question again.
- **Look for**: compare the two answers. With the ontology on, the answer should cite domain
  concepts (check the citations panel) and be more specific about why privacy and fidelity
  evaluation matter in this domain. With it off, expect a more generic answer with no
  citations. Note in your comments whether the difference is real and useful in your
  judgement. This comparison is direct evidence for (or against) the value of the retrieval
  machinery.

### Throughout all scenarios

- If a hallucination banner ever appears, write down in which scenario and what term was
  flagged. That is a finding, not noise.
- If a truncation banner appears, note it and re-ask a shorter question.
- Feel free to ask follow-up questions anywhere. The rubric scores the first answer, but the
  quality of follow-up handling is well worth a comment.

### Free exploration (15 to 30 minutes, strongly encouraged)

After the scripted scenarios, build or modify a model that looks like your real use case and
ask the questions you would actually ask. Score those answers too. Unscripted expert usage is
the most valuable feedback this evaluation can produce.

## Scoring: the rubric

In evaluation mode there is a "Rubric feedback" panel under every answer, with seven questions
scored Yes / Partial / No, plus a free-text comment. Score every answer you evaluate right
after reading it, while it is fresh. The dimensions:

1. **Correct given the model under review?** No factual errors about your diagram; the
   deterministic findings are explained, not contradicted.
2. **Actionable?** After reading, you would know what to add, connect, or configure next.
3. **Specific to this model?** It names your actual nodes and elements, rather than generic
   boilerplate that fits any pipeline.
4. **Appropriate priority and severity framing?** Important things first, no alarmism about
   minor issues, no downplaying of real ones.
5. **Clear and well organised?** Understandable without re-reading.
6. **Free of unsupported claims?** Nothing asserted that your model, the findings, or the
   attached documents don't support.
7. **Proposes ontology concepts as model elements?** Careful, this one is inverted: answer
   "Yes" only if the model suggested adding something that is an ontology concept but not a
   real node type. This is a known blind spot, so "Yes" here is a defect finding.

Use the comment field for anything the scores don't capture: formatting problems, tone,
latency that bothered you, a particularly good or bad phrase (quote it).

If you are running without evaluation mode, keep the same scores per scenario in a text file
and send that instead. Evaluation mode is much preferred though.

## Sending your results back

When you are done, meaning all scenarios are scored and your comments are written:

1. Click the **Export evaluation bundle** button at the top of the Recommender panel.
2. Your browser downloads two files: `evaluation-bundle-<timestamp>.zip` (your session logs:
   every question, every answer, your scores and comments, plus technical metrics) and the
   project ZIP (the models you built).
3. Send both files back to the evaluation coordinator.

For transparency, here is what the bundle contains: your questions, the model's answers, the
exact context the model was given (which includes your model contents and any attached
documents), your scores and comments, and environment information (application version, model
name, Ollama version). Nothing is uploaded automatically anywhere. Nothing leaves your machine
until you send the files yourself.

---

*This guide matches application version 2026.1.0, chat model `gemma4:12b`, and embedding model
`nomic-embed-text` on Ollama. The scenario set mirrors the scenario catalogue and evaluation
strategy of deliverable D5.3. The deliverable presents the same material in formal form, but
this guide is meant to be sufficient on its own.*

[← Back to the main README](../README.md)

