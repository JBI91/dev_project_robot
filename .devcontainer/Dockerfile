
ARG UBUNTU_VERSION=24.04
FROM ubuntu:${UBUNTU_VERSION}

# Variables de entorno generales
ARG ROS_DISTRO=jazzy


ENV DEBIAN_FRONTEND=noninteractive \
    LANG=C.UTF-8 \
    LC_ALL=C.UTF-8 \
    ROS_DISTRO=${ROS_DISTRO} 

# Instalar dependencias básicas
RUN apt-get update && apt-get install -y --no-install-recommends \
    ca-certificates curl gnupg2 lsb-release \
    python3 python3-pip git wget build-essential \
    && rm -rf /var/lib/apt/lists/*

# Configurar repositorio ROS 2
RUN mkdir -p /etc/apt/keyrings && \
    curl -sSL https://raw.githubusercontent.com/ros/rosdistro/master/ros.key -o /etc/apt/keyrings/ros-archive-keyring.gpg && \
    echo "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/ros-archive-keyring.gpg] \
         http://packages.ros.org/ros2/ubuntu $(lsb_release -cs) main" \
         > /etc/apt/sources.list.d/ros2.list

# Instalar paquetes ROS mínimos
RUN apt-get update && apt-get install -y --no-install-recommends \
    ros-${ROS_DISTRO}-desktop \
    ros-${ROS_DISTRO}-ros-core \
    ros-${ROS_DISTRO}-ament-cmake \
    ros-${ROS_DISTRO}-ament-cmake-core \
    ros-${ROS_DISTRO}-rosbridge-server \
    ros-${ROS_DISTRO}-joy \
    ros-${ROS_DISTRO}-xacro \
    python3-colcon-common-extensions \
    python3-rosdep \
    && rm -rf /var/lib/apt/lists/*

# Inicializar rosdep (capa independiente)
RUN rosdep init && rosdep update

# Herramientas Python
#RUN pip3 install --no-cache-dir --upgrade pip setuptools wheel && \
#    pip3 install --no-cache-dir -U vcstool lark colcon-bundle colcon-cmake

# Crear usuario no‑root con sudo
RUN apt-get update && apt-get install -y --no-install-recommends sudo && rm -rf /var/lib/apt/lists/* && \
    useradd -m -s /bin/bash rosuser && \
    usermod -aG sudo rosuser && \
    echo 'rosuser ALL=(ALL) NOPASSWD:ALL' > /etc/sudoers.d/rosuser

# Configurar workspace
USER rosuser
WORKDIR /workspace

# Dependencias adicionales (root)
USER root
RUN apt-get update && apt-get install -y --no-install-recommends \
    libtinyxml2-dev libasio-dev libssl-dev && \
    rm -rf /var/lib/apt/lists/*

# CMD final
WORKDIR /workspace
CMD ["/bin/bash"]
