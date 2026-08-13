# INSAFEDARE Web Application

## Evaluation Version

This package contains the standalone **INSAFEDARE Web Application** as a Java FAT JAR.

This version is intended for evaluation and demonstration by INSAFEDARE project partners.

## 1. Prerequisites

### Java

Java **17 or newer** is required. Java 21 is recommended.

Check your Java installation:

```bash
java -version
```

### Operating System

The application has been tested in a Linux environment. A Linux system with Bash and Java 17+ is recommended.

## 2. Package Contents

The distribution should contain:

```text
INSAFEDARE-Web-Application/
├── insafedare-web-application-v1.jar
└── README.md
```

The `.jar` is a Java FAT JAR containing the application and its Java dependencies.

**Maven, Node.js, npm, and the application source code are not required to launch the application.**

## 3. Launching the Application

Open a terminal and navigate to the directory containing the JAR:

```bash
cd /path/to/INSAFEDARE-Web-Application
```

Launch the application:

```bash
java -jar insafedare-web-application-v1.jar
```

Keep the terminal open while the application is running.

## 4. Accessing the Application

After the application has finished starting, open a web browser and go to:

```text
http://localhost:8080
```

## 5. Stopping the Application

The application runs in the foreground.

To stop it, press:

```text
Ctrl+C
```

## 6. Changing the Application Port

The default port is **8080**.

If port 8080 is already in use, launch the application on another port. For example:

```bash
java -jar insafedare-web-application-v1.jar --server.port=8082
```

Then access:

```text
http://localhost:8082
```

## 7. Troubleshooting

### Java is not installed

If you see:

```text
java: command not found
```

check:

```bash
java -version
```

Install Java 17 or newer and try again.

### Port 8080 is already in use

If the application reports:

```text
Web server failed to start. Port 8080 was already in use.
```

either stop the application using port 8080 or use another port:

```bash
java -jar insafedare-web-application-v1.jar --server.port=8082
```

### Application does not start

Keep the complete terminal output and provide it to the INSAFEDARE development team. The startup log will help identify the problem.

## 8. Important Notes

- This is the **INSAFEDARE Web Application Evaluation Version**.
- The application is distributed as a Java FAT JAR.
- The application is launched directly using Java.
- No Maven, Node.js, npm, or source code is required.
- The application is accessed through a web browser.
- The application is stopped with `Ctrl+C`.
- The default address is `http://localhost:8080`.

## 9. Quick Start

If Java 17+ is already installed:

```bash
java -version
java -jar insafedare-web-application-v1.jar
```

Then open:

```text
http://localhost:8080
```

To stop the application:

```text
Ctrl+C
```

## 10. Version

**INSAFEDARE Web Application — Evaluation Version v1**
