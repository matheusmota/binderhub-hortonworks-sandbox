FROM hortonworks/sandbox-hdp:2.6.5
# copying the repository
COPY . .

# using root
USER root
RUN yum -y install https://centos7.iuscommunity.org/ius-release.rpm  && \
    yum -y install yum-utils python36u python36u-pip python36u-devel htop wget curl 

# ensuring the installation of the notebook package
RUN pip3.6 -v && \
    pip3.6 install --upgrade pip && \ 
    pip3.6 install notebook jupyter jupyterlab 
# making building process faster by selecting best mirror
