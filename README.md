# ASAP CRN Workbench App Templates

This repository contains example Dev Container configurations for building custom apps in Verily Workbench.

The goal of this repository is to provide reusable starter templates that can help researchers and developers create Workbench apps with customized environments, tools, and startup behavior.

## Purpose

Verily Workbench supports custom apps through Dev Container-based configurations. These examples are intended to help users:

* Build custom Workbench app environments
* Add project-specific tools and packages
* Test apps locally before using them in Workbench
* Create command-line-friendly or IDE-friendly development environments
* Support reproducible analysis and pipeline development workflows

## Repository Structure

```text
asapcrn-workbench-app-devcontainers/
├──  ubuntu/
└── README.md
```


## Available Examples

### Ubuntu App

A minimal Ubuntu-based custom app that runs a browser-accessible terminal using `ttyd`.

## General Workflow

To use one of the app examples:

1. Fork this repository.
2. Choose an app example
3. Modify the app files as needed.
4. Test the app locally.
5. Push your changes to your fork.
6. Create a custom app in Verily Workbench that points to your forked repository and the selected app folder.

## Files Commonly Customized

Most app templates include the following files:

### `Dockerfile`

Defines the app image and installed tools.

Use this file to add packages, command-line tools, Python/R dependencies, or other system requirements.

### `docker-compose.yaml`

Defines how the app container runs.

Use this file to configure ports, commands, users, volumes, and runtime behavior.

### `.devcontainer.json`

Defines the Dev Container configuration.

Use this file to configure Dev Container features, startup commands, workspace settings, and app behavior.

## Local Testing

Before using an app in Workbench, test it locally.

Create the required Docker network:

```bash
docker network create app-network
```

Run the app with the Dev Containers CLI:

```bash
devcontainer up --workspace-folder .
```

Check the app-specific README for the correct port and launch instructions.

## Using an App in Verily Workbench

After testing locally:

1. Push your changes to GitHub.
2. Open Verily Workbench.
3. Go to your workspace.
4. Select **Apps**.
5. Select **+ New app instance**.
6. Choose **Custom**.
7. Provide the GitHub URL for your forked repository.
8. Select the folder for the app you want to use.
9. Configure compute settings.
10. Create and launch the app.

## Intended Audience

This repository is intended for CRN team members, researchers, and developers who want to create or customize Verily Workbench app environments for analysis, pipeline development, and reproducible workflows.
