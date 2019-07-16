FROM hortonworks/sandbox-hdp:2.6.5
# copying the repository
COPY . .

# using root
USER root
RUN yum update && \
    yum install python3 python3-pip -y

# ensuring the installation of the notebook package
RUN pip install --no-cache notebook jupyter jupyterlab apt-select
# making building process faster by selecting best mirror
RUN apt-select --country BR && mv sources.list /etc/apt/

# installing apt packages
RUN apt-get update && \
    apt-get install $(grep -vE "^\s*#" ./binder/apt.txt  | tr "\n" " ") -y

# enviroment info
ARG  NB_USER=jovyan
ARG  NB_UID=1000
ENV  USER   ${NB_USER}
ENV  NB_UID ${NB_UID}
ENV  HOME   /home/${NB_USER}

# allowing ssh within the container (for hadoop)
RUN mkdir /var/run/sshd
RUN ssh-keygen -A && chmod 600 /etc/ssh* -R
RUN mkdir .ssh
RUN adduser ${NB_USER} root
RUN adduser ${NB_USER} sudo
RUN echo "${NB_USER} ALL=(root) NOPASSWD:ALL" > /etc/sudoers.d/user && \
    chmod 0440 /etc/sudoers.d/user

RUN  chown -R ${NB_USER} ${HOME} 

### moving to user setup
USER ${NB_USER}

# binder user requirements
RUN pip install -r ./binder/requirements.txt

# binder user postBuild 
RUN chmod +x ${HOME}/binder/postBuild && \
    sh       ${HOME}/binder/postBuild

