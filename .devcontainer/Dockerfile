# Build with offical support (https://github.com/osrf/docker_images)
FROM osrf/ros:humble-desktop

ARG USERNAME=ros
ARG USER_UID=1000
ARG USER_GID=$USER_UID
# Create a non-root user
RUN groupadd --gid $USER_GID $USERNAME \
  && useradd -s /bin/bash --uid $USER_UID --gid $USER_GID -m $USERNAME \
  # [Optional] Add sudo support for the non-root user
  && apt-get update \
  && apt-get install -y sudo \
  && echo $USERNAME ALL=\(root\) NOPASSWD:ALL > /etc/sudoers.d/$USERNAME\
  && chmod 0440 /etc/sudoers.d/$USERNAME \
  # Cleanup
  && rm -rf /var/lib/apt/lists/* \
  && echo "source /usr/share/bash-completion/completions/git" >> /home/$USERNAME/.bashrc \
  && echo "if [ -f /opt/ros/${ROS_DISTRO}/setup.bash ]; then source /opt/ros/${ROS_DISTRO}/setup.bash; fi" >> /home/$USERNAME/.bashrc

# Default workspace when building with devcontainer.
# see https://code.visualstudio.com/remote/advancedcontainers/change-default-source-mount
ARG WORKSPACE=/workspaces/${USERNAME}
RUN echo "if [ -f ${WORKSPACE}/install/setup.bash ]; then source ${WORKSPACE}/install/setup.bash; fi" >> /home/ros/.bashrc


# Preferred dep to me.
RUN apt-get update && apt-get install -y python3-pip \
  && apt-get install -y vim \ 
  && apt-get -y install curl \
  && apt install -y libxtst6 \
  && apt-get install -y libasound2 \ 
  && apt install -y libcanberra-gtk-module libcanberra-gtk3-module \
  && apt-get install -y net-tools \
  && apt-get install -y software-properties-common

# install mqtt
RUN apt-add-repository ppa:mosquitto-dev/mosquitto-ppa && apt-get update && apt-get install -y mosquitto && apt-get install -y mosquitto-clients
# run mqtt bridge

# install mqtt-bridge
RUN apt-get install -y ros-$ROS_DISTRO-mqtt-client
