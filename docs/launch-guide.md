# Launch the Application

[← Back to the main README](../README.md)

Before starting, [install Java and Docker](prerequisites.md) and [download the application JAR](download-guide.md). Make sure Docker is running.

## Open the JAR Folder in a Terminal

Open a terminal and change to the folder containing `insafedare-web-application-v1.jar`.

**Linux and macOS:** If the JAR is in your Downloads folder, run:

```bash
cd ~/Downloads
```

**Windows:** Open PowerShell. If the JAR is in your Downloads folder, run:

```powershell
cd "$HOME\Downloads"
```

If you saved the JAR elsewhere, replace `Downloads` with the path to that folder. Keep the terminal open for the following steps.

## First Launch

### 1. Create and Start the PostgreSQL Container

Run this command once to create the database container:

```bash
docker run -d --name insafedare-postgres -p 5433:5432 -e POSTGRES_DB=sirius-web-db -e POSTGRES_USER=dbuser -e POSTGRES_PASSWORD=dbpwd -v insafedare-pg-data:/var/lib/postgresql/data postgres:16
```

Verify that PostgreSQL is ready:

```bash
docker exec insafedare-postgres pg_isready -U dbuser -d sirius-web-db
```

The response should say that PostgreSQL is **accepting connections**.

### 2. Launch the Application

From the folder containing the JAR, run:

```bash
java -jar insafedare-web-application-v1.jar
```

Keep the terminal open while using the application. Once startup is complete, open <http://localhost:8080> in your browser.

## Subsequent Launches

Start Docker if it is not already running. Then start the existing PostgreSQL container:

```bash
docker start insafedare-postgres
```

From the folder containing the JAR, launch the application:

```bash
java -jar insafedare-web-application-v1.jar
```

Open <http://localhost:8080> in your browser.

## Stop the Application

Return to the terminal running the application and press **Ctrl+C**.

To stop the PostgreSQL container as well, run this command in a separate terminal:

```bash
docker stop insafedare-postgres
```

Stopping the container does not delete its stored data.

If a step fails, see the [Troubleshooting Guide](troubleshooting.md).

Next: **[Upload an Existing Pipeline →](upload-pipelines.md)**
