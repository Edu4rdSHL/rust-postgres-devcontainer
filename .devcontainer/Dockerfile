FROM rust:latest
# Install the development dependencies
RUN apt-get update && apt-get upgrade -y && apt-get install -y build-essential curl git libssl-dev pkg-config make postgresql-client \
    postgresql clang lld sudo vim bash-completion && apt-get clean && rm -rf /var/lib/apt/lists/*
# Create a non-root user
ENV USER_NAME=vscode
RUN useradd -m $USER_NAME -s /bin/bash
USER $USER_NAME
# Install nightly toolchain and rustfmt for it and the stable version
RUN rustup toolchain install nightly --component rustfmt clippy && rustup component add rustfmt clippy \
    && cargo install diesel_cli cargo-edit cargo-update cargo-audit cargo-udeps && mkdir -p /home/$USER_NAME/workspace
# Copy .bash_aliases, it contains several useful aliases for cargo
COPY configs/.bash_aliases /home/$USER_NAME/.bash_aliases
# Detect the postgres version and set the volume
ENV POSTGRES_VERSION=15
VOLUME /var/lib/postgresql/$POSTGRES_VERSION/main
# trust all local connections to postgres
COPY configs/pg_hba.conf /etc/postgresql/$POSTGRES_VERSION/main/pg_hba.conf
# Allow the user to run sudo without password, generate locales and set the default one
USER root
RUN echo "$USER_NAME ALL=(ALL:ALL) NOPASSWD: ALL" | tee /etc/sudoers.d/$USER_NAME && \
    echo "en_US.UTF-8 UTF-8" > /etc/locale.gen && locale-gen && update-locale
# Delete all the cargo cache and the apt cache
RUN rm -rf /usr/local/cargo/registry && apt-get clean
