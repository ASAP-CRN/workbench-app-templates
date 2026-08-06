# ASAP CRN Workbench App Templates

This repository contains example Dev Container configurations for building custom apps in Verily Workbench.

These templates are intended to help CRN users create Workbench apps with customized software environments, installed tools, startup behavior, and development workflows.

## Purpose

Verily Workbench supports custom apps through Dev Container-based configurations. This repository provides starter templates that can be forked, modified, tested locally, and then used to create custom apps in Workbench.

These examples can help users:

* Build custom Workbench app environments
* Add project-specific tools and packages
* Test apps locally before launching them in Workbench
* Create command-line-friendly or IDE-friendly development environments
* Support reproducible analysis and pipeline development workflows
* Experiment with AI-assisted development tools in a Workbench-compatible environment

## Repository Structure

```text
asapcrn-workbench-app-devcontainers/
├── ubuntu/
│   ├── startupscript/
│   ├── .devcontainer-lock.json
│   ├── .devcontainer.json
│   ├── devcontainer-template.json
│   ├── docker-compose.yaml
│   ├── Dockerfile
│   ├── README.md
│   └── requirements.txt
├── vscode/
│   ├── .devcontainer/
│   ├── startupscript/
│   ├── .devcontainer-lock.json
│   ├── .devcontainer.json
│   ├── devcontainer-debug.log
│   ├── devcontainer-template.json
│   ├── docker-compose.yaml
│   ├── Dockerfile
│   ├── README.md
│   ├── requirements.txt
│   └── sudo-passwordless.sh
├── LICENSE
└── README.md
```

Each app template lives in its own folder. Start with the app-specific `README.md` for setup, customization, local testing, and launch instructions.

> Note:
> The `ubuntu/` and `vscode/` templates are independent examples. Changes made to one template do not automatically apply to the other.

## Available Templates

### Ubuntu App

The `ubuntu/` template is a minimal Ubuntu-based custom app that runs a browser-accessible terminal using `ttyd`.

This template is useful for:

* Command-line workflows
* Testing custom package installation
* Building lightweight development environments
* Creating a base app for pipeline development or analysis workflows

Key files include:

| File                         | Purpose                                                                                                     |
| ---------------------------- | ----------------------------------------------------------------------------------------------------------- |
| `Dockerfile`                 | Defines the Ubuntu-based app image and installs system dependencies, Python, `ttyd`, and required packages. |
| `docker-compose.yaml`        | Defines how the app container runs, including ports, users, volumes, and networking.                        |
| `.devcontainer.json`         | Defines the Dev Container configuration used for local testing and Workbench app setup.                     |
| `devcontainer-template.json` | Defines template metadata and options used by the Workbench custom app flow.                                |
| `requirements.txt`           | Lists Python packages installed into the app environment.                                                   |
| `startupscript/`             | Contains startup and remount scripts used to prepare the app environment and support Workbench behavior.    |

### VS Code App

The `vscode/` template is a browser-accessible VS Code-style development environment built with `code-server`.

This template is useful for:

* IDE-friendly development workflows
* Editing notebooks, scripts, documentation, and pipeline code
* Working with GitHub repositories from inside Workbench
* Running Python-based analysis environments
* Testing AI-assisted development workflows in a Workbench-compatible environment

The VS Code template includes support for:

* Gemini Code Assist
* Claude Code / Claude API-enabled development workflows
* A custom Python virtual environment
* Project-specific packages from `requirements.txt`
* Workbench-compatible Dev Container configuration

Key files include:

| File                         | Purpose                                                                                                            |
| ---------------------------- | ------------------------------------------------------------------------------------------------------------------ |
| `Dockerfile`                 | Defines the `code-server` app image, installs VS Code extensions, system dependencies, and the Python environment. |
| `docker-compose.yaml`        | Defines how the VS Code app container runs, including ports, users, volumes, and networking.                       |
| `.devcontainer.json`         | Defines the Dev Container configuration used for local testing and Workbench app setup.                            |
| `.devcontainer/`             | Contains additional Dev Container-related configuration files, if needed by the template.                          |
| `devcontainer-template.json` | Defines template metadata and options used by the Workbench custom app flow.                                       |
| `requirements.txt`           | Lists Python packages installed into the VS Code app environment.                                                  |
| `startupscript/`             | Contains startup and remount scripts used to prepare the app environment and support Workbench resource mounting.  |
| `sudo-passwordless.sh`       | Configures passwordless sudo behavior for the app user when required.                                              |

## General Workflow

To use one of the app templates:

1. Fork this repository.
2. Choose an app template, such as `ubuntu/` or `vscode/`.
3. Modify the app files as needed.
4. Test the app locally.
5. Push your changes to your fork.
6. Create a custom app in Verily Workbench that points to your forked repository and the selected app folder.

## Files Commonly Customized

Most app templates include the following files.

### `requirements.txt`

Defines Python packages installed into the app environment.

Use this file to add or update Python packages required for your analysis workflow.

### `Dockerfile`

Defines the app image and system-level dependencies.

Use this file to install operating system packages, command-line tools, language runtimes, extensions, and other system requirements.

For example:

* The `ubuntu/` template uses the Dockerfile to install terminal and analysis dependencies.
* The `vscode/` template uses the Dockerfile to install VS Code/code-server extensions and configure the Python environment.

### `docker-compose.yaml`

Defines how the app container runs.

Use this file to configure:

* Ports
* Commands
* Users
* Volume mounts
* Runtime behavior
* Workbench-compatible networking

### `.devcontainer.json`

Defines the Dev Container configuration.

Use this file to configure:

* Dev Container features
* Startup commands
* Workspace folder settings
* App behavior
* Workbench custom app metadata

### `devcontainer-template.json`

Defines template metadata and options used by the Workbench custom app flow.

Use this file to update the template name, description, and user-facing options.

## Local Testing

Before using an app in Workbench, test it locally.

From the app template folder, create the required Docker network:

```bash
docker network create app-network
```

If the network already exists, Docker may return a message saying it already exists. That is okay.

Run the app with the Dev Containers CLI:

```bash
devcontainer up --workspace-folder .
```

Check the app-specific README for the correct port, local URL, and launch instructions.

## Using an App in Verily Workbench

After testing locally:

1. Push your changes to GitHub.
2. Open Verily Workbench.
3. Go to your workspace.
4. Select **Apps**.
5. Select **+ New app instance**.
6. Choose **Custom**.
7. Provide the GitHub URL for your forked repository.
8. Select the folder for the app you want to use, such as `ubuntu/` or `vscode/`.
9. Configure compute settings.
10. Create and launch the app.

## Workbench Compatibility Notes

When modifying an app template, keep the following in mind:

* Do not mount local files over the directory where Workbench resources are expected to appear.
* Keep the app port aligned between `docker-compose.yaml`, the app command, and the Workbench app configuration.
* Use absolute paths in startup commands when possible.
* Keep startup scripts executable.
* Test locally before creating or updating the app in Workbench.
* Rebuild the app after changing the Dockerfile, requirements, or container configuration.
* Confirm that workspace resources mount correctly inside the app before sharing the template with others.

