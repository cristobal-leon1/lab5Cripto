# Usar la imagen base de Ubuntu 22.10
FROM ubuntu:22.10

# Actualizar los repositorios para usar los de old-releases
RUN sed -i 's|archive.ubuntu.com|old-releases.ubuntu.com|g' /etc/apt/sources.list
RUN sed -i 's|security.ubuntu.com|old-releases.ubuntu.com|g' /etc/apt/sources.list
RUN sed -i '/^# deb-src/s/^# //' /etc/apt/sources.list

# Actualizar la lista de paquetes disponibles e instalar dependencias necesarias
RUN apt-get update && apt-get install -y \
    openssh-server \
    openssh-client \
    zlib1g-dev \
    libssl-dev \
    autoconf \
    automake \
    libtool \
    build-essential \
    dpkg-dev \
    wget \
    && rm -rf /var/lib/apt/lists/*

# Crear el directorio necesario para el demonio de sshd
RUN mkdir -p /var/run/sshd

# Configurar una nueva variable de entorno para el nombre de usuario y su directorio home
ENV USER_NAME=test
ENV USER_HOME=/home/$USER_NAME

# Crear un nuevo usuario y su directorio home
RUN useradd -m -d $USER_HOME $USER_NAME

# Establecer la contraseña para el usuario creado (ajusta según tus necesidades de seguridad)
RUN echo 'test:test' | chpasswd

# Permitir el inicio de sesión root mediante contraseña (ajusta según tus necesidades de seguridad)
RUN sed -i 's/PermitRootLogin prohibit-password/PermitRootLogin yes/' /etc/ssh/sshd_config

# Exponer el puerto 22 para conexiones SSH
EXPOSE 22

# Establecer el comando por defecto para iniciar el servidor SSH en primer plano
CMD ["/usr/sbin/sshd", "-D"]
