# Usar la imagen base de Ubuntu 22.10
FROM ubuntu:22.10

# Actualizar los repositorios para usar los de old-releases
RUN sed -i 's|archive.ubuntu.com|old-releases.ubuntu.com|g' /etc/apt/sources.list
RUN sed -i 's|security.ubuntu.com|old-releases.ubuntu.com|g' /etc/apt/sources.list
RUN sed -i '/^# deb-src/s/^# //' /etc/apt/sources.list

# Actualizar la lista de paquetes disponibles
RUN apt-get update

# Instalar openssh-server y openssh-client junto con otras herramientas necesarias
RUN apt-get install -y openssh-server openssh-client zlib1g-dev libssl-dev autoconf automake libtool

# Instalar dpkg-dev y corregir posibles problemas de dependencias faltantes
RUN apt-get install -y dpkg-dev --fix-missing

# Descargar el código fuente de openssh-client
RUN apt-get source openssh-client

# Crear el directorio necesario para el demonio de sshd
RUN mkdir -p /var/run/sshd

# Configurar una nueva variable de entorno para el nombre de usuario y su directorio home
ENV USER_NAME=test
ENV USER_HOME=/home/$USER_NAME

# Crear un nuevo usuario y su directorio home
RUN useradd -m -d $USER_HOME $USER_NAME

# Establecer la contraseña para el usuario creado
RUN echo 'test:test' | chpasswd

# Permitir el inicio de sesión root mediante contraseña
RUN sed -i 's/PermitRootLogin prohibit-password/PermitRootLogin yes/' /etc/ssh/sshd_config

# Exponer el puerto 22 para conexiones SSH
EXPOSE 22

# Establecer el comando por defecto para iniciar el servidor SSH en primer plano
CMD ["/usr/sbin/sshd", "-D"]

