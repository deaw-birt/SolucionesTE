# Base image
FROM jboss/wildfly
 
# Create user isola with password 1234
RUN /opt/jboss/wildfly/bin/add-user.sh isola 1234 --silent
 
# Add example.war to deployments
COPY war/* /opt/jboss/wildfly/standalone/deployments/
 
# JBoss ports
# 8080 where application is deployed, 9990 for admin console and 8787 to enable remote debugging
EXPOSE 8080 9990 8009
 
# Run
CMD ["/opt/jboss/wildfly/bin/standalone.sh", "-b", "0.0.0.0", "-bmanagement", "0.0.0.0","--debug"]