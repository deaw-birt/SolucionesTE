FROM mysql

RUN \
    echo [mysqld] >> /etc/mysql/my.cnf && \
    echo "innodb_use_native_aio=0" >> /etc/mysql/my.cnf

# Copiar y mover los scripts SQL a la carpeta del contenedor
# donde se ejecutarán al arrancar el servidor
COPY sql/student.sql sql/create-user-table.sql /
RUN mv create-user-table.sql student.sql docker-entrypoint-initdb.d/

EXPOSE 3306