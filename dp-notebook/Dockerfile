# Distributed under the terms of the Modified BSD License.
ARG BASE_CONTAINER=jupyter/base-notebook
FROM $BASE_CONTAINER

LABEL maintainer="FumiyukiKato <fumilemon79@gmail.com>"

USER root

ENV DPCOMP_CORE=$HOME/Documents/dpcomp_core \
    PYTHONPATH=$DPCOMP_CORE:$PYTHONPATH \
    DPCOMP_LOG_PATH=$HOME/logs \
    DPCOMP_LOG_LEVEL=DEBUG

# Install all OS dependencies for fully functional notebook server
RUN apt-get update && apt-get install -yq --no-install-recommends \
    build-essential \
    emacs-nox \
    vim-tiny \
    git \
    inkscape \
    jed \
    libsm6 \
    libxext-dev \
    libxrender1 \
    lmodern \
    netcat \
    python3-dev \
    # ---- nbconvert dependencies ----
    texlive-xetex \
    texlive-fonts-recommended \
    texlive-plain-generic \
    # ----
    tzdata \
    unzip \
    nano \
    curl \
    # dpcomp
    gfortran \
    liblapack-dev \
    libblas-dev \
    libpq-dev \
    libncurses5-dev \
    swig \
    && apt-get clean && rm -rf /var/lib/apt/lists/*

RUN git clone https://github.com/dpcomp-org/dpcomp_core.git $DPCOMP_CORE

RUN cd $DPCOMP_CORE \
    && conda install --yes --file resources/requirements.txt

RUN mkdir $DPCOMP_LOG_PATH

RUN cd $DPCOMP_CORE/dpcomp_core/algorithm && ./setup.sh

RUN chmod 777 $DPCOMP_CORE $HOME/work

# # Switch back to jovyan to avoid accidental container runs as root
# USER $NB_UID
