
# 1. Imagen
FROM ubuntu:18.04

# 2. Instalacion de paquetes y clonar proyecto GitHub
RUN apt-get update && apt-get install -y apache2 && \
    apt-get -y install libapache2-mod-fcgid && \
    apt-get install git --yes \
    && a2enmod proxy_fcgi && \
    git clone https://github.com/deaw-birt/despliegue-PHP.git /proyecto-birt

# 3. Copia de configuracion de sitio web a contenedor
COPY 000-default.conf /etc/apache2/sites-available

# 4. Mover archivos de GitHub a servidor
# 5. Renombrar archivo .sql (se modifica la primera letra, I x i)
#     El tema de esta modificacion es porque el servidor ejecuta
#     primero 'InsertarDatos.sql' en lugar de 'crearBBDD.sql'. 
#     Puede ser debido al codigo ASCII:
#     c = 99         I = 73          i = 105
RUN \
    mv /proyecto-birt/php/* /var/www/html && \
    mv /proyecto-birt/mysql/InsertarDatos.sql  /proyecto-birt/mysql/insertarDatos.sql


EXPOSE 80

# Arrancar apache2
CMD ["apachectl", "-D", "FOREGROUND"]