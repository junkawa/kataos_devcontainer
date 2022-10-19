# See here for image contents: https://github.com/microsoft/vscode-dev-containers/tree/v0.245.2/containers/python-3/.devcontainer/base.Dockerfile

# [Choice] Python version (use -bullseye variants on local arm64/Apple Silicon): 3, 3.10, 3.9, 3.8, 3.7, 3.6, 3-bullseye, 3.10-bullseye, 3.9-bullseye, 3.8-bullseye, 3.7-bullseye, 3.6-bullseye, 3-buster, 3.10-buster, 3.9-buster, 3.8-buster, 3.7-buster, 3.6-buster
ARG VARIANT="3.10-bullseye"
FROM mcr.microsoft.com/vscode/devcontainers/python:0-${VARIANT}

# [Choice] Node.js version: none, lts/*, 16, 14, 12, 10
ARG NODE_VERSION="none"
RUN if [ "${NODE_VERSION}" != "none" ]; then su vscode -c "umask 0002 && . /usr/local/share/nvm/nvm.sh && nvm install ${NODE_VERSION} 2>&1"; fi

# [Optional] Uncomment this section to install additional OS packages.
RUN apt-get update && export DEBIAN_FRONTEND=noninteractive \
    && apt-get -y install --no-install-recommends cmake ninja-build device-tree-compiler libxml2-utils cpio libarchive-dev \
       qemu-utils qemu-efi-aarch64 qemu-system-arm \
    && apt clean \
    && rm -rf /var/lib/apt/lists/*

# [Optional] If your pip requirements rarely change, uncomment this section to add them to the image.
# COPY requirements.txt /tmp/pip-tmp/
# RUN pip3 --disable-pip-version-check --no-cache-dir install -r /tmp/pip-tmp/requirements.txt \
#    && rm -rf /tmp/pip-tmp
RUN pip3 --disable-pip-version-check --no-cache-dir install tempita pyyaml pyfdt jinja2 six plyplus future aenum pyelftools sortedcontainers ordered_set libarchive-c jsonschema

# [Optional] Uncomment this line to install global node packages.
# RUN su vscode -c "source /usr/local/share/nvm/nvm.sh && npm install -g <your-package-here>" 2>&1

RUN curl -sSL https://get.haskellstack.org/ | sh

USER vscode

RUN mkdir -p ~/.local/bin && curl https://storage.googleapis.com/git-repo-downloads/repo > ~/.local/bin/repo && chmod 0755 ~/.local/bin/repo

RUN curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh -s -- -y
RUN . "$HOME/.cargo/env" && rustup toolchain add nightly-2021-11-05 \
    && rustup target add --toolchain nightly-2021-11-05-x86_64-unknown-linux-gnu aarch64-unknown-none

RUN cd && wget https://developer.arm.com/-/media/Files/downloads/gnu/11.2-2022.02/binrel/gcc-arm-11.2-2022.02-x86_64-aarch64-none-linux-gnu.tar.xz \
    && tar xf gcc-arm-11.2-2022.02-x86_64-aarch64-none-linux-gnu.tar.xz \
    && rm -f gcc-arm-11.2-2022.02-x86_64-aarch64-none-linux-gnu.tar.xz
RUN echo 'PATH="$HOME/gcc-arm-11.2-2022.02-x86_64-aarch64-none-linux-gnu/bin:$PATH"' >> ~/.profile
