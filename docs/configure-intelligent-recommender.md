# Configure the Intelligent Recommendation System

[← Back to the main README](../README.md)

# Install and Configure Ollama

The INSAFEDARE Web Application uses [Ollama](https://ollama.com/) to provide AI-assisted recommendations during graphical pipeline design. Before using this feature, install Ollama, ensure its service is running, and download the required language and embedding models.

This guide describes how to install and configure Ollama on **Linux, Windows, and macOS**.

## 1. Prerequisites

- An internet connection to download Ollama and the required models.
- Sufficient disk space to store the models. The `gemma4:12b` model requires several gigabytes of storage.
- Access to a terminal or command-line interface.

## 2. Linux

### 2.1. Install Ollama

Open a terminal and run:

```bash
curl -fsSL https://ollama.com/install.sh | sh
```

For installation details and alternative methods, consult the [official Ollama website](https://ollama.com/).

### 2.2. Start the Ollama service

If Ollama is configured as a systemd service, start and enable it with:

```bash
sudo systemctl enable --now ollama
```

Check its status:

```bash
systemctl status ollama
```

If Ollama was installed without a systemd service, start the Ollama application using the method appropriate for your installation.

### 2.3. Download the required models

Run the following commands:

```bash
ollama pull gemma4:12b
ollama pull nomic-embed-text
```

Wait for both downloads to complete. Depending on your internet connection and hardware, downloading the models may take some time.

### 2.4. Verify the installation

List the locally available models:

```bash
ollama list
```

Confirm that both `gemma4:12b` and `nomic-embed-text` appear in the list.

## 3. Windows

### 3.1. Install Ollama

1. Visit the [official Ollama download page](https://ollama.com/download).
2. Download and install the Windows version.
3. Launch Ollama and ensure it is running.

### 3.2. Download the required models

Open PowerShell or Command Prompt and execute:

```powershell
ollama pull gemma4:12b
ollama pull nomic-embed-text
```

Wait for both models to finish downloading.

### 3.3. Verify the installation

Run:

```powershell
ollama list
```

Confirm that both required models are listed.

## 4. macOS

### 4.1. Install Ollama

1. Visit the [official Ollama download page](https://ollama.com/download).
2. Download the version compatible with your Mac.
3. Install and open Ollama.

### 4.2. Download the required models

Open Terminal and execute:

```bash
ollama pull gemma4:12b
ollama pull nomic-embed-text
```

Wait for both downloads to complete.

### 4.3. Verify the installation

Run:

```bash
ollama list
```

Confirm that both required models are listed.

## 5. Verify Ollama Connectivity

The INSAFEDARE application expects Ollama to be available at the following local endpoint:

```text
http://localhost:11434
```

To check whether the service is responding, run the following command.

**Linux and macOS:**

```bash
curl http://localhost:11434/api/tags
```

**Windows PowerShell:**

```powershell
Invoke-RestMethod http://localhost:11434/api/tags
```

The response should include information about the models available locally. Confirm that `gemma4:12b` and `nomic-embed-text` are present.

## 6. Storage Considerations

The required models can occupy several gigabytes of disk space. Ensure that the drive used by Ollama has enough free space before downloading them.

If the default system drive has limited capacity, configure Ollama to store its models on a drive with more available space **before** pulling the models.

Refer to the [official Ollama documentation](https://docs.ollama.com/) for instructions on configuring the model storage location for your operating system.

## 7. Troubleshooting

### Ollama is not responding

- Ensure that Ollama is installed and running.
- Check whether the endpoint `http://localhost:11434` is accessible.
- On Linux, inspect the service status using `systemctl status ollama` if systemd is used.

### A required model is missing

Run the corresponding pull command again:

```bash
ollama pull gemma4:12b
ollama pull nomic-embed-text
```

Then verify the installed models with `ollama list`.

### A model download fails because of insufficient disk space

Free up space or configure Ollama to use a drive with sufficient capacity. Retry the model download after verifying that the storage configuration is correct.

Next: **[Test and evaluate the intelligent recommender →](recommender-evaluation-guide.md)**
