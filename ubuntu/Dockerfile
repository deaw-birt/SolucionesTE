# Imagen 
FROM ubuntu:18.04

# Descarga de paquetes
RUN \
    apt-get update && apt-get install nano unzip wget default-jdk -y && \
    mkdir /usr/local/tomcat && \
    wget https://dlcdn.apache.org/tomcat/tomcat-8/v8.5.88/bin/apache-tomcat-8.5.88.zip && \
    unzip apache-tomcat-8.5.88.zip && cp -Rv apache-tomcat-8.5.88/* /usr/local/tomcat && \
    rm -rf apache-tomcat-8.5.88.zip apache-tomcat-8.5.88 && \
    chmod +x /usr/local/tomcat/bin/* 

# Usuario: isola - Password: 1234
COPY conf/my-tomcat-users.xml /usr/local/tomcat/conf/tomcat-users.xml

# Copia archivo context.xml con <Valve> comentada
COPY META-INF/context.xml /

# Copiar archivo anterior <context.xml> a las aplicaciones para acceder a las mismas
# Borrar dicho archivo tras las copias
RUN \
    echo \
        /usr/local/tomcat/webapps/manager/META-INF/ \
        /usr/local/tomcat/webapps/host-manager/META-INF/ \
        /usr/local/tomcat/webapps/docs/META-INF/ \
        /usr/local/tomcat/webapps/examples/META-INF/ \
        | xargs -n 1 cp context.xml && \
    rm -rf context.xml

# Copiar archivos war a la carpeta webapps
COPY war/* /usr/local/tomcat/webapps

# Puertos
EXPOSE 8080

# Arrancar despliegue de aplicaciones
CMD /usr/local/tomcat/bin/catalina.sh run; 