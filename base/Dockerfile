# gpu-cli/base - Minimal bootstrap image for GPU CLI
# ~50MB compressed, fast to pull
#
# Usage:
#   FROM ghcr.io/gpu-cli/base:latest
#   # Your ML code here
#
# Includes:
#   - gpu-bootstrap v0.2.0 (agent delivery via WebRTC signaling socket)
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
    && rm -rf /var/lib/apt/lists/*

# Download gpu-bootstrap binary for dev mode agent delivery
# v0.2.0: Uses WebRTC signaling socket for NAT traversal (reads GPU_WEBRTC_OFFER)
ARG TARGETARCH
ARG GPU_BOOTSTRAP_VERSION=v0.2.0
RUN ARCH=$([ "$TARGETARCH" = "arm64" ] && echo "aarch64" || echo "x86_64") && \
    curl -fsSL "https://github.com/gpu-cli/gpu-bootstrap/releases/download/${GPU_BOOTSTRAP_VERSION}/gpu-bootstrap-linux-${ARCH}" \
    -o /usr/local/bin/gpu-bootstrap && \
    chmod +x /usr/local/bin/gpu-bootstrap

WORKDIR /workspace

# Default command - container stays alive for gpu-pod-agent
CMD ["sleep", "infinity"]
