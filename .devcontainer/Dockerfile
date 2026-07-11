FROM ros:lyrical

# create a non-root user with UID 1000
#   catch the related arguments
ARG USERNAME
ARG USER_UID
ARG USER_GID
#   delete if a usr with UID 1000 user exists
RUN if id -u $USER_UID ; then userdel `id -un $USER_UID` ; fi
#   add and user and setup sudo
RUN groupadd --gid $USER_GID $USERNAME \
    && useradd --uid $USER_UID --gid $USER_GID -m $USERNAME -s /bin/bash \
    && apt-get update \
    && apt-get install -y sudo \
    && echo $USERNAME ALL=\(root\) NOPASSWD:ALL > /etc/sudoers.d/$USERNAME \
    && chmod 0440 /etc/sudoers.d/$USERNAME

# update the container
RUN apt-get update \
    && apt-get upgrade -y    

# set up sshd
RUN apt-get install -y \
    openssh-server 

# additional general packages
RUN apt-get install -y \
    vim \
    htop \
    tmux \
    iproute2 \
    x11-apps

# additional ros packages
RUN apt-get install -y \
    ros-lyrical-rviz2

# clean up a bit
RUN apt-get autoremove --purge -y \
    && apt-get clean
    
# set the default user for the container
USER $USERNAME

# set the ros environment variables automatically when opening a new shell
RUN echo "source /opt/ros/lyrical/setup.bash" >> /home/$USERNAME/.bashrc