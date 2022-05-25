# -----------------------------------------------------------------------------
# dadaroce (davidson@kiwibot.com)
# -----------------------------------------------------------------------------

ARG CUDA_VERSION=11.4.2
ARG OS_VERSION=20.04


FROM nvidia/cuda:${CUDA_VERSION}-cudnn8-devel-ubuntu${OS_VERSION}

# Or your actual UID, GID on Linux if not the default 1000
ARG USERNAME=koda
ARG USER_UID=1000
ARG USER_GID=$USER_UID

ENV DEBIAN_FRONTEND noninteractive

# -----------------------------------------------------------------------------
# Install essentials
RUN apt-get update && \
    apt-get install -y --no-install-recommends \
    # ------------------------------
    # Basics
    apt-utils \
    bash-completion \
    curl \
    git \
    jq \
    # ------------------------------
    # Python
    python3 \
    python3-pip \
    python3-argcomplete \
    python3-distutils \
    # ------------------------------
    # Free memory
    && apt autoremove -y && apt clean -y \
    && rm -rf /var/lib/apt/lists/*

# -----------------------------------------------------------------------------
# PYTHON LIBRARIES
RUN pip3 install -U pip \
    && python3 -m pip install \
    # Add EXTRA DEV Python dependencies in here
    # ------------------------------
    # Tools to check code compliance
    black~=20.8b1 \
    pylint~=2.11.1 \
    # ------------------------------
    # Installing pytorch
    # For RTX 30XX series we need to use torch >=1.8 because they only work with cuda>=11.1
    https://download.pytorch.org/whl/cu111/torch-1.8.0%2Bcu111-cp38-cp38-linux_x86_64.whl \
    # ------------------------------
    # Installing basics
    setuptools \
    /koda/.cache/pip

# -----------------------------------------------------------------------------
# Install GCP client
RUN echo "deb [signed-by=/usr/share/keyrings/cloud.google.gpg] http://packages.cloud.google.com/apt cloud-sdk main" | tee -a /etc/apt/sources.list.d/google-cloud-sdk.list \
    && curl https://packages.cloud.google.com/apt/doc/apt-key.gpg | apt-key --keyring /usr/share/keyrings/cloud.google.gpg  add - \
    && apt-get update -y \
    && apt-get install google-cloud-sdk=369.0.0-0 -y \
    # ---------------------------------
    && apt autoremove -y && apt clean -y \
    && rm -rf /var/lib/apt/lists/*

# # Authenticate with local gcp-key.json
# COPY configs/gcp-key.json .
# RUN gcloud auth activate-service-account --key-file ./gcp-key.json

CMD ["/bin/bash"]