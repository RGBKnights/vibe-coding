#------------------------------------------------------------------------------
# Dockerfile for a Dev Container based on universal:2.9.0-linux,
# which has .NET 8.0.201, Node.js, Python, Ruby, Go, etc.
# The default non-root user in this image is "codespace".
#------------------------------------------------------------------------------

# Use the updated universal dev container image (Ubuntu 20.04 base).
FROM mcr.microsoft.com/devcontainers/universal:2.9.0-linux

# Set a couple optional environment variables
ARG TZ
ENV TZ="$TZ"
ENV DEVCONTAINER=true

# ------------------------------------------------------------------------------
# 1. Switch to root to install additional packages/tools
# ------------------------------------------------------------------------------
USER root

# Overwrite the built-in notice with your own custom message
RUN echo "Welcome Vibe Coding with Claude! To get started type 'claude'" \
  > /usr/local/etc/vscode-dev-containers/first-run-notice.txt

RUN echo "" > /usr/local/etc/vscode-dev-containers/conda-notice.txt

# (Optional) Install additional packages not already in the universal image.
RUN apt-get update && apt-get install -y \
    iptables \
    ipset \
    iproute2 \
    dnsutils \
    aggregate \
    fzf \
    man-db \
    jq \
    unzip \
    zsh \
    procps \
    less \
    sudo \
    && apt-get clean && rm -rf /var/lib/apt/lists/*

# (Optional) If you want to install a custom Zsh prompt (the universal image
# already includes oh-my-zsh, but this is an example of the 'zsh-in-docker' script)
RUN sh -c "$(wget -O- https://github.com/deluan/zsh-in-docker/releases/download/v1.2.0/zsh-in-docker.sh)" -- \
  -p git \
  -p fzf \
  -a "source /usr/share/doc/fzf/examples/key-bindings.zsh" \
  -a "source /usr/share/doc/fzf/examples/completion.zsh" \
  -a "export PROMPT_COMMAND='history -a' && export HISTFILE=/commandhistory/.bash_history" \
  -x

# Create a directory to persist shell history, then fix ownership to 'codespace'
RUN mkdir -p /commandhistory \
    && touch /commandhistory/.bash_history \
    && chown -R codespace:codespace /commandhistory

# Create a workspace folder (if not already) and fix ownership
RUN mkdir -p /workspace \
    && chown -R codespace:codespace /workspace
WORKDIR /workspace

# (Optional) Install git-delta for fancy diffs
RUN ARCH=$(dpkg --print-architecture) \
  && wget -q "https://github.com/dandavison/delta/releases/download/0.18.2/git-delta_0.18.2_${ARCH}.deb" \
  && dpkg -i "git-delta_0.18.2_${ARCH}.deb" \
  && rm "git-delta_0.18.2_${ARCH}.deb"

# (Optional) Install a global npm package like Claude code
RUN npm install -g @anthropic-ai/claude-code

# Copy your firewall script into the container
COPY init-firewall.sh /usr/local/bin/
RUN chmod +x /usr/local/bin/init-firewall.sh

# Allow user "codespace" to run the firewall script without a password
RUN echo "codespace ALL=(root) NOPASSWD: /usr/local/bin/init-firewall.sh" > /etc/sudoers.d/codespace-firewall \
  && chmod 0440 /etc/sudoers.d/codespace-firewall

# ------------------------------------------------------------------------------
# 2. Switch back to the default non-root user "codespace"
# ------------------------------------------------------------------------------
USER codespace
