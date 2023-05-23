# DEAW - TareaEvaluativa02 - ALTERNATIVAS DE ACCESO A UN SERVIDOR 

# Imagen
FROM ubuntu:18.04


# Informacion
LABEL \
	version="1.0" \
	description="Ubuntu + Apache2 + virtual host" \
	creationDate="17/01/2023" \
	maintainer="Ivan Sola Marti <isola@birt.eus>"


# Modo NONINTERACTIVE (solo en ejecucion)
ARG DEBIAN_FRONTEND=noninteractive


# Instalar paquetes
RUN \
	apt-get update && \ 
	apt-get install \
		nano \
		apache2 -y \
		proftpd \
		ssh \
		git -y && \
	apt-get clean


# Copiar archivos referentes al servidor web
COPY \
	apache/index1.html \
	apache/index2.html \
	apache/sitioweb1.conf \
	apache/sitioweb2.conf \
	apache/sitio1.key \
	apache/sitio1.cer /



# Creacion de directorios
# Mover archivos a contenedor
# Activar sitios web y ssl
RUN \
	mkdir var/www/html/sitioweb1 var/www/html/sitioweb2 /root/.ssh ./start_services && \
	mv index1.html /var/www/html/sitioweb1/index.html && \
	mv index2.html /var/www/html/sitioweb2/index.html && \
	mv sitioweb1.conf /etc/apache2/sites-available && \
	a2ensite sitioweb1 && \
	mv sitioweb2.conf /etc/apache2/sites-available && \
	a2ensite sitioweb2 && \
	mv sitio1.key /etc/ssl/private && \
	mv sitio1.cer /etc/ssl/certs && \
	a2enmod ssl


# Preconfiguracion servidores FTP, SSH
COPY ftp/isola1.crt /etc/ssl/certs/
COPY ftp/isola1.key /etc/ssl/private/
COPY ftp/proftpd.conf /etc/proftpd
COPY ftp/tls.conf /etc/proftpd
COPY ssh/sshd_config /etc/ssh 
COPY init.sh ./start_services


# Montar directorio puntual para usar clave RSA y clonar repositorio GitHub
# Crear usuarios y asignarles parametros
RUN \
	--mount=type=secret,id=klabea,dst=/root/.ssh/id_rsa \ 
	useradd isola1 -m -d /var/www/html/sitioweb1 -g nogroup -p $(openssl passwd -1 isola1) -s /sbin/nologin && \
	useradd isola2 -m -d /var/www/html/sitioweb2 -g nogroup -p $(openssl passwd -1 isola2) -s /bin/bash && \
	useradd isola -m -d /srv/ftp -g nogroup -p $(openssl passwd -1 isola) -s /sbin/nologin && \
	chown isola1 /var/www/html/sitioweb1 && \
	chown isola2 /var/www/html/sitioweb2 && \
	ssh-keyscan github.com >> /root/.ssh/known_hosts && \
	eval $(ssh-agent -s) && ssh-add /root/.ssh/id_rsa && \
	git clone git@github.com:deaw-birt/deaw03-te1-ftp-anonimo.git /deaw-birt && \
	cp deaw-birt/* -r /srv/ftp 


EXPOSE \ 
	80 443 21 22 \ 
	50000-50030



# Iniciar servidores
ENTRYPOINT [ "/bin/sh" ]
CMD [ "./start_services/init.sh" ]