FROM centos:centos7
MAINTAINER Julio Gonzalez Gil <git@juliogonzalez.es>

USER root

# Default UID/GID for mounting folders (used at jenkins slaves)
ARG UID=500
ARG GID=500

# Create fet group and user according to arguments UID/GID
RUN groupadd -g ${GID} fet && \
    useradd -d /home/fet -m -u ${UID} -g ${GID} -G wheel fet

# Create the wrappers to check for updates and to keep the container running
ADD files/*.sh /opt/

# Pass the output of date command as DATE argument if you want 
# make sure that the image is generated using the lastest 
# packages (cache will be used for previous steps)
ARG DATE=None

# Generate a list of original packages, install EPEL repository, update,
# install needed packages and create symlink for qmake-qt5
RUN rpm -qa --qf "%{NAME}\n" > /opt/packages-image.txt && \
    yum -y -q install epel-release && \
    yum -y -q update && \
    yum -y -q install \
     cppcheck \
     gcc-c++ \
     git \
     make \
     qt5-qtbase-devel \
     sudo && \
    ln -s /usr/bin/qmake-qt5 /usr/local/bin/qmake && \
    yum -q clean all # ${DATE}

USER fet

CMD ["/bin/bash", "/opt/start_wait.sh"]
