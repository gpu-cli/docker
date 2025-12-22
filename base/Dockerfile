# gpu-cli/base - Minimal bootstrap image for GPU CLI
# ~50MB compressed, fast to pull
#
# Usage:
#   FROM ghcr.io/gpu-cli/base:latest
#   # Your ML code here
#
# Includes:
#   - curl, wget, ca-certificates (for downloading gpu-pod-agent)
#   - openssh-server (pre-configured for RunPod)
#   - bash, basic coreutils

FROM ubuntu:22.04

# Prevent interactive prompts during package installation
ENV DEBIAN_FRONTEND=noninteractive

# Install minimal bootstrap dependencies in single layer
RUN apt-get update && apt-get install -y --no-install-recommends \
    # Network tools for downloading binaries
    curl \
    wget \
    ca-certificates \
    # SSH server for GPU CLI communication
    openssh-server \
    # Basic utilities
    bash \
    coreutils \
    && rm -rf /var/lib/apt/lists/* \
    # Create SSH directory structure
    && mkdir -p /var/run/sshd /root/.ssh \
    && chmod 700 /root/.ssh \
    # Configure SSH for key-only auth
    && sed -i 's/#PermitRootLogin.*/PermitRootLogin prohibit-password/' /etc/ssh/sshd_config \
    && sed -i 's/#PasswordAuthentication.*/PasswordAuthentication no/' /etc/ssh/sshd_config \
    && sed -i 's/#PubkeyAuthentication.*/PubkeyAuthentication yes/' /etc/ssh/sshd_config

WORKDIR /workspace

# Default command keeps container alive for SSH access
CMD ["bash", "-c", "exec /usr/sbin/sshd -D"]
