# Usar la imagen base de Ubuntu 20.10
FROM ubuntu:20.10

# Actualizar los repositorios para usar los de old-releases
RUN sed -i 's|archive.ubuntu.com|old-releases.ubuntu.com|g' /etc/apt/sources.list
RUN sed -i 's|security.ubuntu.com|old-releases.ubuntu.com|g' /etc/apt/sources.list
RUN sed -i '/^# deb-src/s/^# //' /etc/apt/sources.list

# Actualizar la lista de paquetes disponibles
RUN apt-get update

# Instalar openssh-server y openssh-client junto con otras herramientas necesarias
RUN apt-get install -y openssh-server openssh-client dpkg-dev nano autoconf automake libtool zlib1g-dev libssl-dev --fix-missing

# Descargar el código fuente de openssh-client
RUN apt-get source openssh-client

# Crear el directorio necesario para el demonio de sshd
RUN mkdir /var/run/sshd

# Permitir el inicio de sesión root mediante contraseña
RUN sed -i 's/PermitRootLogin prohibit-password/PermitRootLogin yes/' /etc/ssh/sshd_config

# Exponer el puerto 22 para conexiones SSH
EXPOSE 22

# Establecer el comando por defecto para iniciar el servidor SSH en primer plano
CMD ["/usr/sbin/sshd", "-D"]

