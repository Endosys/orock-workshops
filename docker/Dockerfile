FROM registry.redhat.io/ubi8/ubi

RUN INSTALL_PKGS="openssh-clients rsync wget unzip python38 python38-devel python38-setuptools python38-pip \
        atlas-devel gcc-gfortran libffi-devel libtool-ltdl enchant glibc-langpack-en" && \
    yum -y module enable python38:3.8 && \
    yum -y --setopt=tsflags=nodocs install $INSTALL_PKGS && \
    rpm -V $INSTALL_PKGS && \
    yum -y clean all --enablerepo='*'

# install terraform binary
# current release as of this date...check to see if a newer version is availabke
RUN wget https://releases.hashicorp.com/terraform/0.12.26/terraform_0.12.26_linux_amd64.zip && \
    unzip terraform_0.12.26_linux_amd64.zip -d /usr/local/bin terraform

RUN ln -s /usr/bin/python3 /usr/bin/python && \
    ln -s /usr/bin/pip3 /usr/bin/pip

# Install pip requirements
ADD requirements.txt .
RUN python -m pip install -r requirements.txt

WORKDIR /app
#ADD . /app

# Switching to a non-root user, please refer to https://aka.ms/vscode-docker-python-user-rights
RUN useradd appuser && chown -R appuser /app
USER appuser

RUN mkdir ~/.ssh && \
    echo "host *" >> ~/.ssh/config && \
    echo "StrictHostKeyChecking no" >> ~/.ssh/config

# During debugging, this entry point will be overridden. For more information, please refer to https://aka.ms/vscode-docker-python-debug
#CMD ["python", "app.py"]
CMD ["/bin/bash"]
