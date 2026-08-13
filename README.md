# INSAFEDARE Web Application

## Testing Version

This repository provides the **INSAFEDARE Web Application** for testing by INSAFEDARE project partners.

The application is distributed as a standalone Java FAT JAR and can be launched locally from a terminal and accessed through a web browser.

## Download the Application

The application JAR is available under **GitHub Releases** and is therefore not displayed directly among the repository files.

### Download v1.0

Go to:

https://github.com/sydgep/insafedare-web-application/releases/tag/v1.0

Under **Assets**, download:

```text
insafedare-web-application-v1.jar
```

Alternatively, from the main repository page:

1. Locate **Releases** on the right-hand side of the repository.
2. Select **v1.0**.
3. Expand **Assets** if necessary.
4. Download `insafedare-web-application-v1.jar`.

---

## 1. Prerequisites

### Java

Java **17 or newer** is required. **Java 21 is recommended.**

On Ubuntu/Debian Linux, Java 21 can be installed using:

```bash
sudo apt update
sudo apt install openjdk-21-jdk
```

After installation, verify the Java version:

```bash
java -version
```

You should see a version similar to:

```text
openjdk version "21.x.x"
```

If Java 17 or newer is already installed, you can skip the installation step.

---

## 2. Package Contents

After downloading the application, you only need the JAR file:

```text
INSAFEDARE-Web-Application/
└── insafedare-web-application-v1.jar
```

The `.jar` is a Java FAT JAR containing the application and its required Java dependencies.

Maven, Node.js, npm, and the application source code are **not required** to launch the application.

---

## 3. Launching the Application

Open a terminal and navigate to the directory containing the downloaded JAR.

For example, if the JAR is in your Downloads directory:

```bash
cd ~/Downloads
```

Launch the application:

```bash
java -jar insafedare-web-application-v1.jar
```

Keep the terminal open while the application is running.

Wait for the application to finish starting before accessing the web interface.

---

## 4. Accessing the Application

Once the application has started, open a web browser and go to:

```text
http://localhost:8080
```

The **INSAFEDARE Web Application** should now be accessible.

---

## 5. Stopping the Application

The application runs in the foreground of the terminal.

To stop it, return to the terminal where the application is running and press:

```text
Ctrl+C
```

---

## 6. Troubleshooting

### Java command not found

If:

```bash
java -version
```

returns an error such as:

```text
java: command not found
```

install Java 21:

```bash
sudo apt update
sudo apt install openjdk-21-jdk
```

Then verify the installation:

```bash
java -version
```

### Port 8080 is already in use

If the application fails to start with a message similar to:

```text
Web server failed to start. Port 8080 was already in use.
```

another application is already using port `8080`.

You can either stop the process using port 8080 or launch INSAFEDARE on another port, for example:

```bash
java -jar insafedare-web-application-v1.jar --server.port=8082
```

Then access the application at:

```text
http://localhost:8082
```

### Application does not start

If the application does not start successfully, keep the complete terminal output and provide it to the INSAFEDARE development team. The startup logs can be used to identify the problem.

---

## Quick Start

If Java 17 or newer is already installed:

```bash
java -version
java -jar insafedare-web-application-v1.jar
```

Then open:

```text
http://localhost:8080
```

To stop the application, press `Ctrl+C` in the terminal.

---

**INSAFEDARE Web Application — Testing Version v1.0**
