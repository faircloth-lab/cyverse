# Dockerfile to build phyluce app for CyVerse
# Based on https://github.com/cyverse/docker-builds/tree/master/vice/dockerfiles/qiime2/2018.11

FROM discoenv/jupyter-lab:beta
LABEL maintainer "Brant Faircloth *(brantfaircloth on Github)*"

USER root

# Update the packages
RUN apt-get update \
    && apt-get clean \
    && rm -rf /usr/lib/apt/lists/*

USER jovyan

# Install Phyluce and Dependencies
RUN conda config --add channels conda-forge
RUN conda config --add channels bioconda
RUN conda create -y -n phyluce phyluce && conda clean -tipsy

# Install bash kernel and Biopython
RUN pip install bash_kernel==0.7.1 \
    && python3 -m bash_kernel.install \
    && pip install biopython==1.73

## Other jupyter lab extensions and server extensions are installed
# Copy over entry.sh
COPY entry.sh /bin
RUN mkdir /home/jovyan/.irods
RUN mkdir /home/jovyan/phyluce/

WORKDIR /home/jovyan/phyluce

ENTRYPOINT ["bash", "/bin/entry.sh"]