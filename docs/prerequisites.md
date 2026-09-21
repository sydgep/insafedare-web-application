# Install Prerequisites

[← Back to the main README](../README.md)

The application requires:

- **Java 17 or newer** — Java 21 is recommended.
- **Docker**.
  
If Java and Docker are already installed, verify them with:

```bash
java -version
docker --version
```
Then continue to [Download Application](download-guide.md).

## Linux (Ubuntu or Debian)

### Install Java 21

```bash
sudo apt update
sudo apt install openjdk-21-jdk
java -version
```

### Install Docker

For a straightforward Ubuntu/Debian installation:

```bash
sudo apt update
sudo apt install docker.io
sudo systemctl enable --now docker
sudo docker info
```

Docker commands may initially require `sudo`. To allow the current user to run Docker without it:

```bash
sudo usermod -aG docker $USER
```

Log out and back in, then verify:

```bash
docker info
```

For Docker's newest supported packages, follow the [official Docker Engine installation guide](https://docs.docker.com/engine/install/).

## Windows

### Install Java 21

1. Download and install a Java 21 JDK, such as [Eclipse Temurin 21](https://adoptium.net/temurin/releases/?version=21).
2. Accept the option to add Java to `PATH` if the installer offers it.
3. Open a new **PowerShell** window and verify:

   ```powershell
   java -version
   ```

### Install Docker Desktop

1. Download [Docker Desktop for Windows](https://docs.docker.com/desktop/setup/install/windows-install/).
2. Run the installer and follow its instructions. Use the WSL 2 backend when recommended.
3. Restart the computer if requested.
4. Open Docker Desktop and wait until it reports that Docker is running.
5. Open PowerShell and verify:

   ```powershell
   docker --version
   docker info
   ```

## macOS

### Install Java 21

1. Download and install a Java 21 JDK, such as [Eclipse Temurin 21](https://adoptium.net/temurin/releases/?version=21). Select the correct package for Apple silicon or Intel.
2. Open a new **Terminal** window and verify:

   ```bash
   java -version
   ```

### Install Docker Desktop

1. Follow the [Docker Desktop for Mac installation guide](https://docs.docker.com/desktop/setup/install/mac-install/).
2. Select the correct installer for Apple silicon or Intel.
3. Start Docker Desktop and wait until Docker is running.
4. Verify:

   ```bash
   docker --version
   docker info
   ```

## Final check

Both commands must complete successfully:

```bash
java -version
docker info
```

Next: **[Download Application →](download-guide.md)**
