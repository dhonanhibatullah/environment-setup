FROM kasmweb/ubuntu-noble-desktop:x86_64-1.19.0

SHELL ["/bin/bash", "-o", "pipefail", "-c"]

ENV DEBIAN_FRONTEND=noninteractive \
    LANG=en_US.UTF-8 \
    LC_ALL=en_US.UTF-8 \
    ROS_DISTRO=jazzy \
    CUDA_HOME=/usr/local/cuda-12.4

ENV PATH=/usr/local/cuda-12.4/bin:/home/kasm-user/.local/bin:${PATH} \
    LD_LIBRARY_PATH=/usr/local/cuda-12.4/lib64

USER root

RUN apt-get update \
    && apt-get install -y --no-install-recommends \
        ca-certificates \
        curl \
        gnupg \
        locales \
        lsb-release \
        software-properties-common \
        sudo \
    && locale-gen en_US en_US.UTF-8 \
    && update-locale LC_ALL=en_US.UTF-8 LANG=en_US.UTF-8 \
    && add-apt-repository universe \
    && echo "kasm-user ALL=(ALL) NOPASSWD:ALL" > /etc/sudoers.d/kasm-user \
    && chmod 0440 /etc/sudoers.d/kasm-user \
    && rm -rf /var/lib/apt/lists/*

RUN apt-get update \
    && ROS_APT_SOURCE_VERSION="$(curl -s https://api.github.com/repos/ros-infrastructure/ros-apt-source/releases/latest | grep -F tag_name | awk -F\" '{print $4}')" \
    && curl -L -o /tmp/ros2-apt-source.deb "https://github.com/ros-infrastructure/ros-apt-source/releases/download/${ROS_APT_SOURCE_VERSION}/ros2-apt-source_${ROS_APT_SOURCE_VERSION}.$(. /etc/os-release && echo "${VERSION_CODENAME}")_all.deb" \
    && apt-get install -y /tmp/ros2-apt-source.deb \
    && rm -f /tmp/ros2-apt-source.deb \
    && rm -rf /var/lib/apt/lists/*

RUN curl -fsSL -o /tmp/cuda-keyring.deb https://developer.download.nvidia.com/compute/cuda/repos/ubuntu2404/x86_64/cuda-keyring_1.1-1_all.deb \
    && dpkg -i /tmp/cuda-keyring.deb \
    && rm -f /tmp/cuda-keyring.deb

RUN apt-get update \
    && apt-get install -y \
        bash-completion \
        build-essential \
        ccache \
        clang \
        cmake \
        curl \
        cuda-toolkit-12-4 \
        dnsutils \
        ffmpeg \
        gdb \
        git \
        htop \
        iputils-ping \
        jq \
        libeigen3-dev \
        libegl1 \
        libgl1 \
        libglvnd0 \
        libhdf5-dev \
        libjpeg-dev \
        liblapack-dev \
        libopenblas-dev \
        libopencv-dev \
        libpng-dev \
        libprotobuf-dev \
        libtiff-dev \
        lld \
        mesa-utils \
        net-tools \
        ninja-build \
        pciutils \
        pipx \
        pkg-config \
        protobuf-compiler \
        python3-colcon-common-extensions \
        python3-dev \
        python3-pip \
        python3-rosdep \
        python3-vcstool \
        python3-venv \
        ripgrep \
        ros-dev-tools \
        ros-jazzy-demo-nodes-cpp \
        ros-jazzy-demo-nodes-py \
        ros-jazzy-desktop \
        ros-jazzy-joint-state-publisher-gui \
        ros-jazzy-navigation2 \
        ros-jazzy-nav2-bringup \
        ros-jazzy-robot-state-publisher \
        ros-jazzy-ros-gz \
        ros-jazzy-rqt \
        ros-jazzy-rqt-common-plugins \
        ros-jazzy-rviz2 \
        ros-jazzy-slam-toolbox \
        ros-jazzy-teleop-twist-keyboard \
        ros-jazzy-tf-transformations \
        ros-jazzy-turtlesim \
        ros-jazzy-xacro \
        tmux \
        usbutils \
        vulkan-tools \
        wget \
    && (rosdep init || true) \
    && (rosdep update || true) \
    && ln -sfn /usr/local/cuda-12.4 /usr/local/cuda \
    && { \
        echo "export CUDA_HOME=/usr/local/cuda-12.4"; \
        echo "export PATH=/usr/local/cuda-12.4/bin:\${PATH}"; \
        echo "export LD_LIBRARY_PATH=/usr/local/cuda-12.4/lib64\${LD_LIBRARY_PATH:+:\${LD_LIBRARY_PATH}}"; \
    } > /etc/profile.d/cuda.sh \
    && chmod 0644 /etc/profile.d/cuda.sh \
    && ldconfig \
    && install -d -o kasm-user -g kasm-user /home/kasm-user/ros2_ws/src \
    && { \
        echo ""; \
        echo "source /opt/ros/jazzy/setup.bash"; \
        echo "if [ -f /home/kasm-user/ros2_ws/install/setup.bash ]; then"; \
        echo "    source /home/kasm-user/ros2_ws/install/setup.bash"; \
        echo "fi"; \
    } >> /etc/bash.bashrc \
    && rm -rf /var/lib/apt/lists/*

USER kasm-user
