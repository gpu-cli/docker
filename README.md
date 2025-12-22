# GPU CLI Docker Images

Minimal bootstrap images for GPU CLI.

## Images

### `ghcr.io/gpu-cli/base`

Minimal Ubuntu 22.04 image (~50MB compressed) with:
- `curl`, `wget`, `ca-certificates` - for downloading gpu-pod-agent
- `openssh-server` - pre-configured for key-only auth
- `bash`, basic coreutils

```dockerfile
FROM ghcr.io/gpu-cli/base:latest

# Add your ML dependencies
RUN apt-get update && apt-get install -y python3 python3-pip
COPY requirements.txt .
RUN pip install -r requirements.txt

COPY . /workspace
```

## Local Testing

```bash
# Build locally
cd base
docker build -t gpu-cli-base:local .

# Test it
docker run -it --rm gpu-cli-base:local bash

# Verify curl/wget work
docker run --rm gpu-cli-base:local curl -fsSL https://gpu-cli.sh
```

## Release

Images are built and pushed on git tags:

```bash
git tag v0.1.0
git push origin v0.1.0
```

This publishes:
- `ghcr.io/gpu-cli/base:0.1.0`
- `ghcr.io/gpu-cli/base:latest`
