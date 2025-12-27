# gpu-cli/base - Minimal bootstrap image for GPU CLI
# ~50MB compressed, fast to pull
#
# Usage:
#   FROM ghcr.io/gpu-cli/base:latest
#   # Your ML code here
#
# Includes:
#   - curl, wget, ca-certificates (for downloading gpu-pod-agent)
#   - python3 (for running Python scripts)
#   - bash, basic coreutils
#
# Note: No SSH server - GPU CLI uses gpu-pod-agent for communication

FROM ubuntu:22.04

# Prevent interactive prompts during package installation
ENV DEBIAN_FRONTEND=noninteractive

# Install minimal bootstrap dependencies in single layer
RUN apt-get update && apt-get install -y --no-install-recommends \
    # Network tools for downloading binaries
    curl \
    wget \
    ca-certificates \
    # Python runtime
    python3 \
    python-is-python3 \
    # Basic utilities
    bash \
    coreutils \
    # Networking utilities
    netcat-openbsd \
    && rm -rf /var/lib/apt/lists/*

WORKDIR /workspace

# Default command - container stays alive for gpu-pod-agent
CMD ["sleep", "infinity"]
