# Sora

A lightweight, self-hosted serverless function runner that executes user-defined applications or scripts based on configuration files. This project allows developers to write small functions and run them on demand—similar to AWS Lambda—but without relying on any cloud provider.

## Project Goal

This project aims to build a minimal, self-hosted serverless function runner — inspired by platforms like AWS Lambda — but without the complexity or dependency on cloud infrastructure.

The core objective is to create a system where functions can be:

- Defined in a dedicated folder with a lightweight config file (`sora.yaml`)
- Written in familiar programming languages, starting with JavaScript (Node.js)
- Automatically set up via predefined shell commands (e.g., `npm install`)
- Invoked on demand through HTTP requests or other triggers
- Executed in isolation, with event data passed in and results returned

By starting with JavaScript as the first supported language, the project establishes a base for adding multi-language support, sandboxing, scheduling, and other serverless patterns over time.

Ultimately, this project explores how to implement the foundational behaviors of a serverless compute host from scratch — with full control over how functions are deployed, managed, and executed.

## Features

- JavaScript (Node.js) function execution
- Function setup via shell commands (e.g., `npm install`)
- HTTP-based invocation
- Modular and minimal: no frameworks or cloud dependency

## Deployment

To deploy the serverless host application, follow these steps:

### Prepare the Deployment Config

Create a configuration file that specifies the paths to the function project directories the host should manage. This allows the host to locate the `sora.yaml` files and associated code.

Each path should point to a folder containing a `sora.yaml` and the function source code.

---

### Start the Serverless Host with Config

When launching the host application, provide the path to the deployment config file. The host will:

- Read all specified function paths
- Parse each `sora.yaml`
- Run setup commands (e.g., install dependencies)
- Prepare the functions for invocation
- Ensure the serverless host is accessible over a secure HTTPS port to allow encrypted communication with clients.
- Configure your firewall and network settings to allow inbound traffic on the HTTPS port.

---

## Serverless Host Configuration

The serverless host requires a configuration file to specify where your functions are located and how the server should run.

### Example: `host-config.yaml`

```yaml
# List of directories containing serverless functions
functionPaths:
  - ./project_1/
  - ./project_2/

# Server settings
server:
  # Port for HTTP (optional if using HTTPS only)
  httpPort: 8080

  # Port for HTTPS - recommended for secure communication
  httpsPort: 443

  # Path to SSL certificate and key (if HTTPS is enabled)
  sslCertPath: /path/to/cert.pem
  sslKeyPath: /path/to/key.pem
```

- `functionPaths`:  
  An array of directories where the host will look for serverless functions. Each directory should contain a `sora.yaml` file and the related function code.

- `server`:  
  Configuration for the server itself, including:
  - `httpPort`: The port to listen for HTTP traffic (optional if using HTTPS only).
  - `httpsPort`: The port to listen for HTTPS traffic (recommended for secure communication).
  - `sslCertPath` and `sslKeyPath`: Paths to your SSL/TLS certificate and private key files, used for HTTPS.
    
---

## Function Configuration

Each application must include a `sora.yaml` in its folder:

```yaml
name: my-app-js
language: nodejs
entrypoint: index.js
handler: handler
setup:
  - npm install
trigger: http
```

## Folder Structure
```
functions/
├── hello-js/
│ ├── sora.yaml
│ ├── index.js
│ └── package.json
```


## Roadmap

The following features are planned for future development:

- [ ] **Language support**: Add support for Python, Go, and other runtimes
- [ ] **Docker isolation**: Run functions inside containers for better security and reproducibility
- [ ] **Cold start simulation**: Optional teardown and re-setup between invocations
- [ ] **Scheduled triggers**: Enable time-based invocation (e.g., cron-style jobs)
- [ ] **File-based triggers**: Add filesystem or event-based triggers
- [ ] **Function logs**: Capture and store stdout/stderr logs per invocation
- [ ] **CLI tool**: Add a command-line interface for deploying and invoking functions locally
- [ ] **Web dashboard**: Basic UI for inspecting functions, logs, and metrics
- [ ] **Secrets/config injection**: Add support for passing secrets or environment variables securely
- [ ] **Function versioning**: Deploy and invoke specific versions of a function
- [ ] **Auto-scaling logic**: Optional worker pools or load-based scaling
