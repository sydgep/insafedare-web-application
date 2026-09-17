# Launch the INSAFEDARE Web Application

[← Back to the main README](../README.md)

Before continuing, confirm that:

- the application JAR has been [downloaded](download-guide.md);
- Java and Docker have been [installed](prerequisites.md); and
- Docker is running.

The same Docker commands below work in a Linux/macOS terminal and in Windows PowerShell.

## First launch

### 1. Start PostgreSQL

Create and start the PostgreSQL 16 container:

```bash
docker run -d --name insafedare-postgres -p 5433:5432 -e POSTGRES_DB=sirius-web-db -e POSTGRES_USER=dbuser -e POSTGRES_PASSWORD=dbpwd -v insafedare-pg-data:/var/lib/postgresql/data postgres:16
```

Docker downloads the image automatically the first time. The command also creates a persistent volume named `insafedare-pg-data`, so projects and pipelines remain available after the container stops.

Verify that PostgreSQL is ready:

```bash
docker exec insafedare-postgres pg_isready -U dbuser -d sirius-web-db
```

A successful response states that the server is accepting connections.

> Run the `docker run` command only once. On later launches, use `docker start insafedare-postgres`.

### 2. Open the application folder

Open a terminal or PowerShell window and move to the folder containing the JAR.

Linux or macOS example:

```bash
cd ~/Downloads
```

Windows PowerShell example:

```powershell
cd "$HOME\Downloads"
```

### 3. Launch INSAFEDARE

```bash
java -jar insafedare-web-application-v1.jar
```

Keep this terminal open while using the application. Wait until the startup messages indicate that the application is ready.

### 4. Open INSAFEDARE

Open a web browser and visit:

<http://localhost:8080>

## Stop the application

Return to the terminal running INSAFEDARE and press `Ctrl+C`.

To stop PostgreSQL as well:

```bash
docker stop insafedare-postgres
```

Stopping the container does not delete its stored data.

## Launch the application next time

1. Start Docker or Docker Desktop.
2. Start the existing database container:

   ```bash
   docker start insafedare-postgres
   ```

3. From the folder containing the JAR, launch INSAFEDARE:

   ```bash
   java -jar insafedare-web-application-v1.jar
   ```

4. Open <http://localhost:8080>.

If any step fails, see the [troubleshooting guide](troubleshooting.md).

Next: **[Upload an Existing Pipeline →](upload-pipelines.md)**
