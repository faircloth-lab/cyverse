# Ubuntu 20.04 (focal)
# OS/ARCH: linux/amd64
# base image
FROM jupyter/minimal-notebook:latest

# reset user to root for installing additional packages
USER root

# Install a few dependencies for iCommands, text editing, and monitoring instances
RUN apt-get update && apt-get install -y \
      apt-transport-https \
      gcc \
      gnupg \
      htop \
      less \
      libfuse2 \
      libpq-dev \
      lsb \
      nano \
      nodejs \
      python3-requests \
      software-properties-common \
      vim

# Install iCommands.  We need some older packages no longer available in 20.04
RUN wget http://mirrors.kernel.org/ubuntu/pool/main/g/glibc/multiarch-support_2.27-3ubuntu1.4_amd64.deb && \
dpkg -i multiarch-support_2.27-3ubuntu1.4_amd64.deb && \
rm multiarch-support_2.27-3ubuntu1.4_amd64.deb

RUN wget http://ftp.se.debian.org/debian/pool/main/o/openssl/libssl1.0.0_1.0.1t-1+deb8u8_amd64.deb && \
dpkg -i libssl1.0.0_1.0.1t-1+deb8u8_amd64.deb && \
rm libssl1.0.0_1.0.1t-1+deb8u8_amd64.deb

RUN wget https://files.renci.org/pub/irods/releases/4.1.12/ubuntu14/irods-icommands-4.1.12-ubuntu14-x86_64.deb && \
dpkg -i irods-icommands-4.1.12-ubuntu14-x86_64.deb && \
rm irods-icommands-4.1.12-ubuntu14-x86_64.deb

# reset container user to jovyan
USER jovyan

# set the work directory
WORKDIR /home/jovyan

# set conda REPOS
RUN conda config --add channels defaults
RUN conda config --add channels bioconda
RUN conda config --add channels conda-forge

# update conda
RUN conda update conda

RUN conda install --quiet --yes \
    'samtools=1.12' \
    'bwa=0.7.17' \
    'vcftools=0.1.16' \
    'bcftools=1.12' \
    'ipyrad=0.9.74' \
    'bash_kernel=0.7.2'

# copy configuration json and entry file into the container
COPY jupyter_notebook_config.json /opt/conda/etc/jupyter/jupyter_notebook_config.json
COPY entry.sh /bin

# expose the public port we want to run on
EXPOSE 8888

# directory will be populated by iCommands when entry.sh is run
RUN mkdir -p /home/jovyan/.irods

ENTRYPOINT ["bash", "/bin/entry.sh"]