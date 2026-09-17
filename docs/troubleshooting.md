# Troubleshooting

[← Back to the main README](../README.md)

## Java is not found

If `java -version` reports that the command is not found, install Java 17 or newer by following the [prerequisites guide](prerequisites.md). Close and reopen the terminal after installation.

## The JAR file is not found

If Java reports `Unable to access jarfile`, confirm that the terminal is in the folder containing:

```text
insafedare-web-application-v1.jar
```

You can also provide the complete path to the file.

## Docker is not found or not running

Verify Docker:

```bash
docker --version
docker info
```

On Windows and macOS, open Docker Desktop and wait until it is running. On Linux, start Docker with:

```bash
sudo systemctl start docker
```

## Docker permission denied on Linux

Temporarily use `sudo` with the Docker command, or add your user to the Docker group:

```bash
sudo usermod -aG docker $USER
```

Log out and back in before trying again.

## PostgreSQL connection refused

If the application cannot connect to `localhost:5433`, check the container:

```bash
docker ps -a --filter name=insafedare-postgres
```

If it exists but is stopped:

```bash
docker start insafedare-postgres
```

Check readiness:

```bash
docker exec insafedare-postgres pg_isready -U dbuser -d sirius-web-db
```

## The PostgreSQL container name already exists

The initial `docker run` command must not be repeated after the container has been created. Start the existing container:

```bash
docker start insafedare-postgres
```

## Port 5433 is already in use

Check whether the INSAFEDARE database is already running:

```bash
docker ps
```

If another service occupies port `5433`, stop that service before starting the INSAFEDARE container. Changing the PostgreSQL host port also requires the application's database configuration to be changed, so it is not recommended for ordinary users.

## Port 8080 is already in use

Launch INSAFEDARE on another port:

```bash
java -jar insafedare-web-application-v1.jar --server.port=8082
```

Then open <http://localhost:8082>.

## The browser cannot open INSAFEDARE

1. Confirm that the terminal running the JAR is still open.
2. Wait for application startup to finish.
3. Check the terminal for an error.
4. Open the exact address <http://localhost:8080>.
5. If a different port was selected, use that port instead.

## A pipeline fails after upload

Uploaded pipelines may contain paths or settings from the computer on which they were created. Check:

- source and target directories;
- input and output filenames;
- required datasets;
- column names and identifiers;
- Docker availability; and
- permissions for the selected directories.

Save the changes, regenerate the executable workflow, and try again.

## Projects or pipelines appear to be missing

Confirm that you started the same `insafedare-postgres` container and did not create a different database volume. Stored data is held in the `insafedare-pg-data` volume.

Do **not** remove this volume unless you intentionally want to delete the stored application data.

## Requesting support

If the problem persists, send the development team:

- your operating system;
- output from `java -version` and `docker --version`;
- the command you ran;
- the complete error or terminal log;
- the pipeline and component involved, if applicable; and
- a screenshot, when it helps show the problem.

Remove passwords, confidential data, and patient information before sharing logs or screenshots.
