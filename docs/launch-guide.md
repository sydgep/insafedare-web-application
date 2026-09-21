# Launch the INSAFEDARE Web Application

[← Back to the main README](../README.md)

## First launch

### Create and start the PostgreSQL database container

Run Docker commands below. 

```bash
docker run -d --name insafedare-postgres -p 5433:5432 -e POSTGRES_DB=sirius-web-db -e POSTGRES_USER=dbuser -e POSTGRES_PASSWORD=dbpwd -v insafedare-pg-data:/var/lib/postgresql/data postgres:16
```

Verify that PostgreSQL is ready:

```bash
docker exec insafedare-postgres pg_isready -U dbuser -d sirius-web-db
```

### Launch the application

Run the command below. 

```bash
java -jar insafedare-web-application-v1.jar
```
Keep this terminal open while using the application. Wait until the startup messages indicate that the application is ready.

Open a web browser and visit: <http://localhost:8080>

NB: 

The terminal working directory must be the folder containing the JAR file. 

Information about navigating file paths can be found [here](https://dev.to/imperatoroz/navigating-file-paths-across-windows-linux-and-wsl-a-devops-essential-1n03)

## Launch the application next time

### Start the existing PostgreSQL database container

   ```bash
   docker start insafedare-postgres
   ```

### Launch the application

   ```bash
   java -jar insafedare-web-application-v1.jar
   ```
Open a web browser and visit: <http://localhost:8080>

## Stop the application

Return to the terminal running the application and press `Ctrl+C`.

To stop PostgreSQL database container, run the command below:

```bash
docker stop insafedare-postgres
```

Stopping the container does not delete its stored data.

Ensure that the prerequisites have been [installed](prerequisites.md) and the application JAR has been [downloaded](download-guide.md);

If any step fails, see the [troubleshooting guide](troubleshooting.md).

Next: **[Upload an Existing Pipeline →](upload-pipelines.md)**
