FROM spack/ubuntu-jammy

LABEL maintainer="Vanessasaurus <@vsoch>"

# Match the default user id for a single system so we aren't root
ARG USERNAME=vscode
ARG USER_UID=1000
ARG USER_GID=1000
ENV USERNAME=${USERNAME}
ENV USER_UID=${USER_UID}
ENV USER_GID=${USER_GID}
USER root

# extra interactive utilities
RUN apt-get update \
 && apt-get -qq install -y --no-install-recommends \
    vim less sudo python3-pip curl tclsh expect tcl-dev

# Ensure regular python is visible
# And environment modules are installed
RUN ln -s /usr/bin/python3 /usr/bin/python && \
    curl -LJO https://github.com/cea-hpc/modules/releases/download/v5.4.0/modules-5.4.0.tar.gz && \
    tar xfz modules-5.4.0.tar.gz && \
    cd modules-5.4.0 && \
    ./configure && make && make install

# Add the group and user that match our ids
RUN ln -s /usr/local/Modules/init/profile.sh /etc/profile.d/modules.sh
RUN groupadd -g ${USER_GID} ${USERNAME} && \
    adduser --disabled-password --uid ${USER_UID} --gid ${USER_GID} --gecos "" ${USERNAME} && \
    echo "${USERNAME} ALL=(ALL) NOPASSWD: ALL" > /etc/sudoers
USER $USERNAME
