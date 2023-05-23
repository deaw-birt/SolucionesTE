
FROM tomcat:8.5-jre8-temurin-jammy

# Modo NONINTERACTIVE (solo en ejecucion)
ARG DEBIAN_FRONTEND=noninteractive


RUN \
    apt-get update && apt-get install nano wget unzip -y && \
    cp -R /usr/local/tomcat/webapps.dist/*  /usr/local/tomcat/webapps && \
    rm -R /usr/local/tomcat/webapps.dist &&  rm -R /usr/local/tomcat/webapps/ROOT && \
    wget www.opencms.org/downloads/opencms/opencms-12.0.zip && \ 
    unzip opencms-12.0.zip && mv opencms.war ROOT.war && mv ROOT.war /usr/local/tomcat/webapps



# Usuario: iban - Password: ibansola1
COPY conf/my-tomcat-users.xml /usr/local/tomcat/conf/tomcat-users.xml
COPY META-INF/context.xml /usr/local/tomcat/webapps/manager/META-INF/context.xml

EXPOSE 8080

CMD ["catalina.sh", "run"]