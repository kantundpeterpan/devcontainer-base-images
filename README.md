# DevContainer Base Images

This repository contains Dockerfiles for building reusable devcontainer base images with **uv** as the Python package manager.

## Available Images

- **Fullstack**: Python 3.11 + Node.js LTS + Cloud SDK + AI tools + uv
- **Data Science**: Python 3.11 + Jupyter + pandas + numpy + scikit-learn + uv
- **GEN-AI Chainlit**: Python 3.11 + Chainlit + LangChain + LangGraph + transformers + uv

## Key Features

✅ **uv Python Package Manager** - Fast dependency resolution and installation
✅ **DevPod Prebuild Support** - Built-in prebuild repository configuration
✅ **Docker-in-Docker** - Full Docker CLI support
✅ **GitHub Actions** - Pre-installed act for local testing
✅ **Cloud SDK** - Google Cloud CLI pre-configured

## Usage

### Option 1: Simple Image Reference

```json
{
  "image": "ghcr.io/yourusername/devcontainer-base-images/fullstack:latest"
}
```

### Option 2: With DevPod Prebuilds

```json
{
  "image": "ghcr.io/yourusername/devcontainer-base-images/fullstack:latest",
  "customizations": {
    "devpod": {
      "prebuildRepository": "ghcr.io/yourusername/yourproject-devpod"
    }
  }
}
```

### Option 3: Extend with Custom Configuration

```json
{
  "image": "ghcr.io/yourusername/devcontainer-base-images/fullstack:latest",
  "customizations": {
    "vscode": {
      "extensions": ["your-extension"]
    },
    "devpod": {
      "prebuildRepository": "ghcr.io/yourusername/yourproject-devpod"
    }
  }
}
```

## Development

### Building Locally

```bash
# Build a specific variant
docker build -t devcontainer-fullstack -f fullstack/Dockerfile .

# Run the container
docker run -it --rm -v $(pwd):/workspace devcontainer-fullstack
```

### Using uv in the Container

All images come with `uv` pre-installed:

```bash
# Create a virtual environment
uv venv .venv

# Install packages
uv pip install package-name

# Run scripts
uv run script.py
```

## DevPod Prebuilds

All images support DevPod prebuilds out of the box. The prebuild repository can be customized via environment variable:

```json
{
  "image": "ghcr.io/yourusername/devcontainer-base-images/fullstack:latest",
  "customizations": {
    "devpod": {
      "prebuildRepository": "${localEnv:DEVPOD_PREBUILD_REPO}"
    }
  }
}
```

Default prebuild repository: `ghcr.io/yourusername/devpod-cache`

## Image Contents

### Fullstack Image
- Python 3.11 with uv package manager
- Node.js LTS
- Google Cloud SDK
- Docker CLI
- GitHub Actions CLI (act)
- Common development tools

### Data Science Image
- Python 3.11 with uv
- Jupyter Lab with extensions
- pandas, numpy, matplotlib, scikit-learn, seaborn, plotly
- Docker CLI

### GEN-AI Chainlit Image
- Python 3.11 with uv
- Chainlit with VS Code extension
- LangChain + LangGraph
- transformers + sentence-transformers
- faiss-cpu for vector search
- Docker CLI

## Publishing New Versions

Images are automatically built and published to GitHub Container Registry when you push to the `main` branch.

## License

MIT License. See LICENSE file for details.