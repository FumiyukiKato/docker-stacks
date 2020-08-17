# Distributed under the terms of the Modified BSD License.
ARG BASE_CONTAINER=jupyter/base-notebook
FROM $BASE_CONTAINER

LABEL maintainer="FumiyukiKato <fumilemon79@gmail.com>"

USER root

ENV EKTELO_HOME=$HOME/Documents/ektelo \
    EKTELO_DATA=/tmp/ektelo \
    EKTELO_LOG_PATH=$HOME/logs \
    EKTELO_LOG_LEVEL=DEBUG

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
    # ----- ektelo dependencies https://github.com/ektelo/ektelo -----
    gfortran \
    liblapack-dev \
    libblas-dev \
    libpq-dev \
    libncurses5-dev \
    swig \
    && apt-get clean && rm -rf /var/lib/apt/lists/*

RUN git clone https://github.com/FumiyukiKato/ektelo.git $EKTELO_HOME

# !!baseはcondaだけどめんどくさいのでpipで全部インストールする，よしなにしてくれるのかは知らない
RUN cd $EKTELO_HOME \
    && conda install --yes --file resources/requirements.txt

RUN mkdir $EKTELO_LOG_PATH

RUN mkdir -p $EKTELO_DATA \
    && curl https://www.dpcomp.org/data/cps.csv > $EKTELO_DATA/cps.csv \
    && curl https://www.dpcomp.org/data/stroke.csv > $EKTELO_DATA/stroke.csv

RUN cd $EKTELO_HOME/ektelo/algorithm && ./setup.sh

RUN chmod 777 $EKTELO_HOME
# # Switch back to jovyan to avoid accidental container runs as root
# USER $NB_UID
