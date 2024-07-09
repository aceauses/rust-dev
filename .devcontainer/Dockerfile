FROM --platform=$TARGETPLATFORM mcr.microsoft.com/devcontainers/rust:latest

# Install additional tools
RUN apt-get update && export DEBIAN_FRONTEND=noninteractive \
	&& apt-get -y install --no-install-recommends \
	gdb \
	lldb \
	&& apt-get clean -y && rm -rf /var/lib/apt/lists/*

# Set CARGO_HOME to a directory where vscode user has permissions
ENV CARGO_HOME=/home/vscode/.cargo

# Install Rust components
RUN rustup component add rls rust-analysis rust-src rustfmt clippy

# Install cargo-edit for easier dependency management
RUN cargo install cargo-edit

# Set up Cargo directories with correct permissions
RUN mkdir -p ${CARGO_HOME}/{registry,git} && \
	chown -R vscode:vscode ${CARGO_HOME}

# Set the default user to 'vscode'
USER vscode

# Update Cargo config to use the new CARGO_HOME
RUN echo '[registry]' > ${CARGO_HOME}/config.toml && \
	echo 'cache = "'${CARGO_HOME}'/registry"' >> ${CARGO_HOME}/config.toml

# Verify Cargo setup
RUN cargo --version