# Integrating-the-Jupyter-server-with-claude-desktop-uisng-the-powerful-model-context-protocol
Jupyter MCP Server Jupyter MCP Server is an implementation of the Model Context Protocol (MCP) server that enables interaction with Jupyter notebooks running in any JupyterLab environment, including your local JupyterLab instance

Start JupyterLab
Make sure you have the following installed. The collaboration package is needed as the modifications made on the notebook can be seen thanks to Jupyter Real Time Collaboration.

pip install jupyterlab==4.4.1 jupyter-collaboration==4.0.2 ipykernel
pip uninstall -y pycrdt datalayer_pycrdt
pip install datalayer_pycrdt==0.12.15
Then, start JupyterLab with the following command.

jupyter lab --port 8888 --IdentityProvider.token MY_TOKEN --ip 0.0.0.0
You can also run make jupyterlab.

Note

The --ip is set to 0.0.0.0 to allow the MCP server running in a Docker container to access your local JupyterLab.

Use with Claude Desktop
Claude Desktop can be downloaded from this page for macOS and Windows.

For Linux, we had success using this UNOFFICIAL build script based on nix

# ⚠️ UNOFFICIAL
# You can also run `make claude-linux`
NIXPKGS_ALLOW_UNFREE=1 nix run github:k3d3/claude-desktop-linux-flake \
  --impure \
  --extra-experimental-features flakes \
  --extra-experimental-features nix-command
To use this with Claude Desktop, add the following to your claude_desktop_config.json (read more on the MCP documentation website).

Important

Ensure the port of the SERVER_URLand TOKEN match those used in the jupyter lab command.

The NOTEBOOK_PATH should be relative to the directory where JupyterLab was started.

Claude Configuration on macOS and Windows
{
  "mcpServers": {
    "jupyter": {
      "command": "docker",
      "args": [
        "run",
        "-i",
        "--rm",
        "-e",
        "SERVER_URL",
        "-e",
        "TOKEN",
        "-e",
        "NOTEBOOK_PATH",
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
Claude Configuration on Linux
CLAUDE_CONFIG=${HOME}/.config/Claude/claude_desktop_config.json
cat <<EOF > $CLAUDE_CONFIG
{
  "mcpServers": {
    "jupyter": {
      "command": "docker",
      "args": [
        "run",
        "-i",
        "--rm",
        "-e",
        "SERVER_URL",
        "-e",
        "TOKEN",
        "-e",
        "NOTEBOOK_PATH",
        "--network=host",
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
EOF
cat $CLAUDE_CONFIG
Components
Tools
The server currently offers 2 tools:

add_execute_code_cell
Add and execute a code cell in a Jupyter notebook.
Input:
cell_content(string): Code to be executed.
Returns: Cell output.
add_markdown_cell
Add a markdown cell in a Jupyter notebook.
Input:
cell_content(string): Markdown content.
Returns: Success message.
Building
You can build the Docker image it from source.

make build-docker
Installing via Smithery
To install Jupyter MCP Server for Claude Desktop automatically via Smithery:

npx -y @smithery/cli install @datalayer/jupyter-mcp-server --client claude


For more information, refer to the official documentation and the MCP documentation website
