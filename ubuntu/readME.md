# Ubuntu Example App

A minimal Verily Workbench custom app example that runs an Ubuntu-based `ttyd` web shell.

This app is intended as a starter template for creating command-line-friendly custom Workbench environments. Users can fork the repository, customize the Dockerfile and Dev Container configuration, test the app locally, and then register it as a custom app in Verily Workbench.

## Project Overview

| Setting               | Value                                |
| --------------------- | ------------------------------------ |
| Image                 | Built from the repository Dockerfile |
| Port                  | `7681`                               |
| User                  | `vscode`                             |
| Home directory        | `/home/vscode`                       |
| App interface         | `ttyd` web terminal                  |
| App working directory | `/mycode`                            |
| Startup scripts       | `/mycode/startupscript`              |

The app starts a browser-accessible terminal using `ttyd`. It also includes Dev Container configuration files so the app can be built and launched through the Dev Containers CLI or Verily Workbench custom app flow.

## Repository Setup

To use this app as a starting point:

1. Fork this repository to your own GitHub account.
2. Edit the configuration files as needed.
3. Test the app locally.
4. Create a custom app in Verily Workbench that points to your forked repository and the `ubuntu-example` folder.

## Files to Customize

Edit the following files to customize your app:

### `Dockerfile`

Defines the Ubuntu-based image and installed system packages.

Use this file to customize:

* Linux packages
* Python or R environments
* Command-line tools
* Cloud SDKs
* Bioinformatics or workflow tools
* User setup and permissions

This is usually the main file to edit when adding packages or dependencies for a custom research environment.

### `.devcontainer.json`

Defines the Dev Container configuration, lifecycle commands, features, and app behavior.

Use this file to customize:

* Dev Container features
* Startup commands
* Post-create and post-start behavior
* Workspace folder settings
* Workbench app metadata and file-opening behavior

### `docker-compose.yaml`

Defines the Docker Compose service used to run the app.

Use this file to customize:

* The app command
* Exposed ports
* Volume mounts
* Build context
* Runtime user
* `ttyd` options

For example, the app currently runs:

```bash
ttyd -W -p 7681 bash
```

## Local Testing

### Prerequisites

Before testing locally, make sure you have:

* Docker installed
* Docker Compose installed
* Dev Containers CLI installed, or the VS Code Dev Containers extension

### Create the Docker Network

This Workbench-style app configuration expects an external Docker network named `app-network`.

Create it locally with:

```bash
docker network create app-network
```

If the network already exists, Docker may return a message saying it already exists. That is okay.

### Run the App Locally

From the folder holding .devcontainer.json, run:

```bash
devcontainer up --workspace-folder .
```

This builds the app, starts the container, and runs the Dev Container lifecycle commands.

### Access the App

Once the container is running, open:

```text
http://localhost:7681
```

You should see a browser-based terminal powered by `ttyd`.

## Using the App in Verily Workbench

After testing locally:

1. Push your changes to your forked GitHub repository.
2. Open Verily Workbench.
3. Go to your workspace.
4. Select **Apps**.
5. Select **+ New app instance**.
6. Choose **Custom**.
7. Provide the GitHub URL for your forked repository.
8. Provide the folder containing this Ubuntu example app.
9. Configure compute settings.
10. Create and launch the app.

Once launched, the app opens a browser-based terminal.

## Summary

This Ubuntu example app provides a lightweight starting point for building command-line-friendly custom apps in Verily Workbench. It can be used as-is for shell-based workflows or extended with additional packages, tools, and startup behavior for more specialized research environments.
