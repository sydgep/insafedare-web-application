# INSAFEDARE Web Application

## Testing Version

This repository provides the **INSAFEDARE Web Application** for testing by INSAFEDARE project partners.

The application is distributed as a standalone Java FAT JAR and can be launched locally from a terminal and accessed through a web browser.

---

## Download the Application

The application JAR is distributed through **GitHub Releases** and is therefore not displayed directly among the repository files.

### Download v1.0

Go to:

https://github.com/sydgep/insafedare-web-application/releases/tag/v1.0

Under **Assets**, download:

```text
insafedare-web-application-v1.jar
```

Alternatively, from the main GitHub repository page:

1. Locate **Releases** on the right-hand side.
2. Select **v1.0**.
3. Expand **Assets** if necessary.
4. Download `insafedare-web-application-v1.jar`.

---

## 1. Prerequisites

The INSAFEDARE Web Application requires:

* **Java 17 or newer** — Java 21 is recommended.
* **Docker** — used to run the PostgreSQL database.
* **PostgreSQL** — provided through a Docker container; no separate PostgreSQL installation is required.
* A modern web browser such as Firefox, Chrome, or Edge.

The application has been tested in a Linux environment. The following installation instructions are intended for **Ubuntu/Debian-based Linux systems**.

### 1.1 Install Java

Update the package index:

```bash
sudo apt update
```

Install OpenJDK 21:

```bash
sudo apt install openjdk-21-jdk
```

Verify the installation:

```bash
java -version
```

You should see a Java version similar to:

```text
openjdk version "21.x.x"
```

If Java 17 or newer is already installed, this step can be skipped.

### 1.2 Install Docker

Install Docker:

```bash
sudo apt install docker.io
```

Start Docker:

```bash
sudo systemctl start docker
```

Enable Docker to start automatically when the computer starts:

```bash
sudo systemctl enable docker
```

Verify the installation:

```bash
docker --version
```

You can also verify that the Docker daemon is running with:

```bash
sudo docker info
```

> Depending on your Docker installation and user configuration, Docker commands may require `sudo`.

### 1.3 Optional: Run Docker Without `sudo`

If Docker requires `sudo` and you want to run Docker commands as your current user:

```bash
sudo usermod -aG docker $USER
```

Log out and log back in for the change to take effect.

Then verify:

```bash
docker info
```

---

## 2. Start the PostgreSQL Database

The INSAFEDARE Web Application requires a PostgreSQL database.

PostgreSQL does **not** need to be installed directly on the computer. It is run as a Docker container.

Create and start the PostgreSQL container with:

```bash
docker run -d \
  --name insafedare-postgres \
  -p 5433:5432 \
  -e POSTGRES_DB=sirius-web-db \
  -e POSTGRES_USER=dbuser \
  -e POSTGRES_PASSWORD=dbpwd \
  -v insafedare-pg-data:/var/lib/postgresql/data \
  postgres:16
```

Docker will automatically download the PostgreSQL 16 image the first time this command is executed.

The configuration used by the application is:

| Parameter          | Value               |
| ------------------ | ------------------- |
| PostgreSQL version | 16                  |
| Host               | localhost           |
| Host port          | 5433                |
| Database           | sirius-web-db       |
| Username           | dbuser              |
| Password           | dbpwd               |
| Docker container   | insafedare-postgres |
| Docker volume      | insafedare-pg-data  |

### Verify PostgreSQL

Check that the container is running:

```bash
docker ps
```

You should see a container named:

```text
insafedare-postgres
```

You can also check PostgreSQL directly:

```bash
docker exec insafedare-postgres pg_isready -U dbuser -d sirius-web-db
```

A successful response should indicate that PostgreSQL is accepting connections.

### Starting PostgreSQL Again

The `docker run` command is required only when creating the container for the first time.

If the container already exists but has been stopped, restart it with:

```bash
docker start insafedare-postgres
```

### Stopping PostgreSQL

To stop the database:

```bash
docker stop insafedare-postgres
```

The database uses the persistent Docker volume:

```text
insafedare-pg-data
```

Therefore, stopping the PostgreSQL container does not remove the database or the pipelines created in the application.

> Do not remove the `insafedare-pg-data` Docker volume unless you intentionally want to delete the stored application data.

---

## 3. Launch the INSAFEDARE Web Application

Open a terminal and navigate to the directory containing the downloaded JAR.

For example, if the JAR was downloaded to the `Downloads` directory:

```bash
cd ~/Downloads
```

Make sure the PostgreSQL container is running:

```bash
docker ps
```

If `insafedare-postgres` is not running, start it:

```bash
docker start insafedare-postgres
```

Then launch the application:

```bash
java -jar insafedare-web-application-v1.jar
```

Keep the terminal open while the application is running.

Wait for the application startup to complete before accessing the web interface.

---

## 4. Access the Application

Once the application has started, open a web browser and go to:

```text
http://localhost:8080
```

The **INSAFEDARE Web Application** should now be accessible.

---

## 5. Stop the Application

The application runs in the foreground of the terminal.

To stop the INSAFEDARE Web Application, return to the terminal where it is running and press:

```text
Ctrl+C
```

The PostgreSQL Docker container will continue running.

If you also want to stop PostgreSQL:

```bash
docker stop insafedare-postgres
```

The database contents will remain stored in the Docker volume and will be available the next time PostgreSQL is started.

---

## 6. Starting the Application Again

After the initial setup, subsequent launches require only two steps.

First, start PostgreSQL if it is not already running:

```bash
docker start insafedare-postgres
```

Then launch the application:

```bash
java -jar insafedare-web-application-v1.jar
```

Open:

```text
http://localhost:8080
```

---

## 7. Troubleshooting

### Java Command Not Found

If:

```bash
java -version
```

returns:

```text
java: command not found
```

install Java 21:

```bash
sudo apt update
sudo apt install openjdk-21-jdk
```

Then verify:

```bash
java -version
```

### Docker Command Not Found

If:

```bash
docker --version
```

returns:

```text
docker: command not found
```

install Docker:

```bash
sudo apt update
sudo apt install docker.io
```

Then start it:

```bash
sudo systemctl enable --now docker
```

### Permission Denied When Running Docker

If you receive a Docker permission error, either run the Docker command with `sudo` or add your user to the Docker group:

```bash
sudo usermod -aG docker $USER
```

Then log out and log back in.

### PostgreSQL Connection Refused

If the application reports an error similar to:

```text
Connection to localhost:5433 refused
```

verify that PostgreSQL is running:

```bash
docker ps
```

If the `insafedare-postgres` container exists but is stopped:

```bash
docker start insafedare-postgres
```

Then launch the application again.

### PostgreSQL Container Already Exists

If you run the initial `docker run` command again and receive an error indicating that the container name `insafedare-postgres` is already in use, do not create another container.

Start the existing one:

```bash
docker start insafedare-postgres
```

### Port 8080 Already in Use

If the application reports:

```text
Web server failed to start. Port 8080 was already in use.
```

another application is already using port `8080`.

Either stop the process using that port or launch INSAFEDARE on another port, for example:

```bash
java -jar insafedare-web-application-v1.jar --server.port=8082
```

Then access:

```text
http://localhost:8082
```

### Port 5433 Already in Use

If Docker cannot start PostgreSQL because port `5433` is already occupied, check whether another PostgreSQL or Docker container is already using that port:

```bash
docker ps
```

If an existing INSAFEDARE PostgreSQL container is already running, you do not need to create another one.

### Application Does Not Start

If the application does not start successfully, keep the complete terminal output and provide it to the INSAFEDARE development team. The startup logs can be used to identify the problem.

---

## Quick Start

For a system where Java and Docker are already installed:

### First launch

Create PostgreSQL:

```bash
docker run -d \
  --name insafedare-postgres \
  -p 5433:5432 \
  -e POSTGRES_DB=sirius-web-db \
  -e POSTGRES_USER=dbuser \
  -e POSTGRES_PASSWORD=dbpwd \
  -v insafedare-pg-data:/var/lib/postgresql/data \
  postgres:16
```

Launch INSAFEDARE:

```bash
java -jar insafedare-web-application-v1.jar
```

Then open:

```text
http://localhost:8080
```

### Subsequent launches

```bash
docker start insafedare-postgres
java -jar insafedare-web-application-v1.jar
```

Then open:

```text
http://localhost:8080
```

To stop the application, press `Ctrl+C`.

---

**INSAFEDARE Web Application — Testing Version v1.0**
