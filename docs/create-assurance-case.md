# Create an Assurance Case

[← Back to the main README](../README.md)

An assurance case organises a claim about a system, dataset, process, or intended use together with the reasoning and evidence that support that claim. In the INSAFEDARE context, an assurance case can help make the properties and limitations of privacy-preserving data and data-processing workflows explicit and reviewable.

The exact fields and terminology available depend on the version of the application. Use the labels shown in the interface and follow your organisation's assurance process.

## 1. Prepare the information

Before creating an assurance case, gather the information that will support the case:

- **Subject:** the dataset, pipeline, generated data product, or system being assessed.
- **Intended use:** the purpose for which the subject is intended to be used.
- **Claim:** the property or outcome you want to justify.
- **Context and assumptions:** the boundaries, conditions, and assumptions under which the claim is made.
- **Argument or reasoning:** how the available evidence supports the claim.
- **Evidence:** documents, metadata, evaluation results, reports, or other artefacts relevant to the claim.
- **Limitations and residual risks:** known gaps, caveats, or conditions that remain unresolved.

Do not include patient-identifiable or other restricted information unless the platform and your organisation's approved procedures explicitly permit it.

## 2. Create a new assurance case

1. Launch the INSAFEDARE Web Application and open the assurance-case functionality.
2. Choose the action to create a new assurance case.
3. Provide a clear, descriptive title.
4. Describe the subject and intended use.
5. Add the main claim that the case is intended to support.
6. Record the relevant context, assumptions, scope, and limitations using the fields provided by the interface.
7. Save the case.

Use concise, verifiable statements. A claim should be specific enough that a reviewer can determine what evidence would support or challenge it.

## 3. Build the argument

Break the main claim into smaller subclaims where necessary. For each subclaim:

1. State what is being asserted.
2. Explain the reasoning that connects the subclaim to the parent claim.
3. Identify the evidence needed to support the reasoning.
4. Record assumptions, dependencies, or unresolved questions.

Keep claims, reasoning, and evidence distinct. A document or evaluation result is evidence; it is not, by itself, the claim being made about the subject.

## 4. Attach supporting evidence and metadata

Add the available evidence through the controls provided by the application. Depending on the case and the application's capabilities, useful evidence may include:

- dataset descriptions and provenance;
- data-quality or statistical evaluation results;
- privacy evaluation results and descriptions of the privacy-preserving transformations applied;
- pipeline configuration and execution records;
- synthetic-data generation details and validation results;
- access, governance, or regulatory-context documentation; and
- references to relevant reports or controlled documents.

For each item, make clear which claim or subclaim it supports. Record the evidence source, version, date, and relevant limitations where those details are available. Avoid asserting that a dataset is anonymous, compliant, or safe solely because a transformation or evaluation was performed.

## 5. Review and save the case

Before considering the case ready for review:

- Check that the main claim is clear and within the stated scope.
- Confirm that each subclaim has a clear line of reasoning.
- Check that evidence is relevant, accessible to the intended reviewers, and linked to the claims it supports.
- Identify missing evidence, contradictory findings, assumptions, and residual risks.
- Ensure that the case does not make stronger claims than the evidence supports.
- Save the latest version and follow your organisation's review or approval process.

An assurance case structures and communicates an argument; creating one does not automatically establish regulatory compliance, prove that privacy risk is eliminated, or constitute independent approval.


Next: **[Configure the Intelligent Recommendation System →](configure-intelligent-recommender.md)**
