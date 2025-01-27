FROM ubuntu:latest

ENV pip_packages "ansible-core ansible-lint yamllint molecule-plugins[docker] docker testinfra"

# Fix for https://github.com/pypa/pip/issues/10219
ARG LANG="en_US.UTF-8"
ARG LC_ALL="en_US.UTF-8"

# make the "en_US.UTF-8" locale so postgres will be utf-8 enabled by default
#RUN apt-get update && apt-get install -y locales && rm -rf /var/lib/apt/lists/* \
#    && localedef -i en_US -c -f UTF-8 -A /usr/share/locale/locale.alias en_US.UTF-8
#
#ENV LANG en_US.UTF-8

RUN apt-get update \
    && apt-get install -y --no-install-recommends \
        apt-transport-https \
        gcc \
        ca-certificates \
        curl \
        git \
        less \
        gnupg \
        jq \
        rsync \
        python3-dev \
        python3-pip \
        docker.io \
        tree \
        sudo \
        locales \
    && rm -rf /var/lib/apt/lists/* \
    && rm -Rf /usr/share/doc && rm -Rf /usr/share/man \
    && apt-get clean

# Fix potential UTF-8 errors with ansible-test.
RUN locale-gen en_US.UTF-8

RUN pip3 install --break-system-packages --upgrade $pip_packages \
    && echo export PATH=$PATH:/root/.local/bin >> /root/.bashrc \
    && export PATH=$PATH:/root/.local/bin \
    && ansible-galaxy collection install community.general

CMD ["/bin/bash"]
