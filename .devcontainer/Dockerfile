FROM ubuntu:24.04

# Define username as arg. Weirdly this is necessary.
ARG USERNAME=${USERNAME}

RUN apt update --fix-missing

RUN apt install -y sudo wget curl git

RUN userdel -r ubuntu

ARG PUID=1000
ARG PGID=1000
RUN printenv
RUN set -x && groupadd --gid $PGID ${USERNAME} && \
  useradd --no-log-init --gid $PGID --create-home --shell /bin/bash --uid $PUID ${USERNAME} --groups sudo && \
  echo "${USERNAME}:${USERNAME}" | chpasswd && \
  echo "${USERNAME} ALL=(ALL) NOPASSWD:ALL" > /etc/sudoers.d/${USERNAME}

RUN apt update --fix-missing

RUN apt install -y software-properties-common

# Install C/Gcc compiler (for Rust)
RUN apt install -y build-essential

#####################################################################
# FROM THIS POINT ON, INSTALL DEPENDENCIES AS LOCAL USER (Not root)
#####################################################################
USER "${USERNAME}"

# Install Rust and Cargo (keep in sync with prod deply docker)
ENV RUST_VERSION=1.93.0
RUN curl https://sh.rustup.rs -sSf | bash -s -- -y

ENV PATH="/home/${USERNAME}/.cargo/bin:${PATH}"

RUN rustup component add rustfmt

RUN printenv

