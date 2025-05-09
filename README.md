# Integrating-the-Jupyter-server-with-claude-desktop-uisng-the-powerful-model-context-protocol
Jupyter MCP Server Jupyter MCP Server is an implementation of the Model Context Protocol (MCP) server that enables interaction with Jupyter notebooks running in any JupyterLab environment, including your local JupyterLab instance


# Jupyter MCP Server for Claude Desktop

## Features

- Provides an MCP server interface for Jupyter notebooks.
- Supports real-time collaboration using JupyterLab's collaboration features.
- Integrates with Claude Desktop (macOS, Windows, Linux).
- Offers tools to add and execute code or markdown cells in notebooks programmatically.

---

## Getting Started

### Prerequisites

Make sure you have the following installed:

- `JupyterLab` (version 4.4.1)
- `jupyter-collaboration` (version 4.0.2)
- `ipykernel`
- `datalayer_pycrdt` (version 0.12.15)

### Installation

```bash
pip install jupyterlab==4.4.1 jupyter-collaboration==4.0.2 ipykernel
pip uninstall -y pycrdt datalayer_pycrdt
pip install datalayer_pycrdt==0.12.15

-----
## Starting JupyterLab

```bash
jupyter lab --port 8888 --IdentityProvider.token MY_TOKEN --ip 0.0.0.0


Alternatively, use:
```bash
make jupyterlab

The --ip 0.0.0.0 option allows the MCP server running in Docker to access your local JupyterLab.


Integration with Claude Desktop
Claude Desktop is available for macOS and Windows. For Linux, you can use an unofficial build script based on Nix.

Configuration (macOS/Windows)
{
  "mcpServers": {
    "jupyter": {
      "command": "docker",
      "args": [
        "run", "-i", "--rm", "-e", "SERVER_URL", "-e", "TOKEN", "-e", "NOTEBOOK_PATH",
        "datalayer/jupyter-mcp-server:latest"
      ],
      "env": {
        "SERVER_URL": "http://host.docker.internal:8888",
        "TOKEN": "MY_TOKEN",
        "NOTEBOOK_PATH": "notebook.ipynb"
      }
    }
  }
}


Configuration (Linux)
{
  "mcpServers": {
    "jupyter": {
      "command": "docker",
      "args": [
        "run", "-i", "--rm", "-e", "SERVER_URL", "-e", "TOKEN", "-e", "NOTEBOOK_PATH", "--network=host",
        "datalayer/jupyter-mcp-server:latest"
      ],
      "env": {
        "SERVER_URL": "http://localhost:8888",
        "TOKEN": "MY_TOKEN",
        "NOTEBOOK_PATH": "notebook.ipynb"
      }
    }
  }
}


Ensure the SERVER_URL and TOKEN match those used when starting JupyterLab. The NOTEBOOK_PATH should be relative to the directory where JupyterLab was started.

Tools
The server currently provides two main tools:

| Tool                    | Description                               | Input Parameter      | Output          |
| ----------------------- | ----------------------------------------- | -------------------- | --------------- |
| `add_execute_code_cell` | Add and execute a code cell in a notebook | `cell_content` (str) | Cell output     |
| `add_markdown_cell`     | Add a markdown cell in a notebook         | `cell_content` (str) | Success message |


Building the Docker Image
To build the Docker image from source:

```bash
make build-docker

Installation via Smithery
To install Jupyter MCP Server for Claude Desktop automatically:

```bash
npx -y @smithery/cli install @datalayer/jupyter-mcp-server --client claude

License
See the repository for license details.

Resources
Official Documentation <!-- https://github.com/datalayer/jupyter-mcp-server -->

MCP Documentation Website <!-- https://modelcontextprotocol.io/quickstart/user#2-add-the-filesystem-mcp-server -->