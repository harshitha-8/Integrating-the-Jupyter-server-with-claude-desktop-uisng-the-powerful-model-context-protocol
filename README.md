# Jupyter MCP Server for Claude Desktop

An implementation of the **Model Context Protocol (MCP)** server that enables interaction with Jupyter notebooks running in any JupyterLab environment, including local instances. Designed for seamless integration with **Claude Desktop** across macOS, Windows, and Linux.

---

## 🚀 Features

- Provides an MCP server interface for Jupyter notebooks.
- Supports real-time collaboration via JupyterLab.
- Integrates smoothly with Claude Desktop (macOS, Windows, Linux).
- Programmatically add and execute code or markdown cells in notebooks.

---

## 📦 Getting Started

### ✅ Prerequisites

Ensure you have the following installed:

- `JupyterLab` (version `4.4.1`)
- `jupyter-collaboration` (version `4.0.2`)
- `ipykernel`
- `datalayer_pycrdt` (version `0.12.15`)

### 🛠 Installation

```bash
pip install jupyterlab==4.4.1 jupyter-collaboration==4.0.2 ipykernel
pip uninstall -y pycrdt datalayer_pycrdt
pip install datalayer_pycrdt==0.12.15
```

---

## ▶️ Starting JupyterLab

```bash
jupyter lab --port 8888 --IdentityProvider.token MY_TOKEN --ip 0.0.0.0
```

Alternatively, you can use:

```bash
make jupyterlab
```

> `--ip 0.0.0.0` allows access from the MCP server running inside Docker.

---

## 💻 Integration with Claude Desktop

Claude Desktop is available on macOS and Windows. Linux users can use an unofficial build script via Nix.

### 🔧 Configuration (macOS / Windows)

```json
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
```

### 🔧 Configuration (Linux)

```json
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
```

> Make sure `SERVER_URL` and `TOKEN` match your JupyterLab setup. `NOTEBOOK_PATH` is relative to the directory where JupyterLab is started.

---

## 🛠 Available Tools

| Tool                    | Description                               | Input Parameter      | Output          |
|-------------------------|-------------------------------------------|----------------------|-----------------|
| `add_execute_code_cell` | Add and execute a code cell in a notebook | `cell_content` (str) | Cell output     |
| `add_markdown_cell`     | Add a markdown cell in a notebook         | `cell_content` (str) | Success message |

---

## 🐳 Building the Docker Image

```bash
make build-docker
```

---

## 📦 Installation via Smithery

```bash
npx -y @smithery/cli install @datalayer/jupyter-mcp-server --client claude
```

---

## 📄 License

See the [repository](https://github.com/datalayer/jupyter-mcp-server) for license details.

---

## 🔗 Resources

- **Official Repository:** [datalayer/jupyter-mcp-server](https://github.com/datalayer/jupyter-mcp-server)
- **MCP Protocol Documentation:** [modelcontextprotocol.io](https://modelcontextprotocol.io/quickstart/user#2-add-the-filesystem-mcp-server)
