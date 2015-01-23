FROM zer0touch/artifactory-base:3.4.1
RUN mv /var/lib/apt/lists* /tmp && \
    mv /var/cache/apt/archives/partial* /tmp
#RUN apt-get install --reinstall locales
#RUN dpkg-reconfigure locales
RUN apt-get update && DEBIAN_FRONTEND=noninteractive apt-get install -y -q postgresql-9.3 postgresql-contrib-9.3 postgresql-9.3-postgis-2.1 libpq-dev sudo curl

#RUN locale-gen en_US.UTF-8
# /etc/ssl/private can't be accessed from within container for some reason
#RUN update-locale LANG=en_US.UTF-8
# (@andrewgodwin says it's something AUFS related)
RUN mkdir /etc/ssl/private-copy; mv /etc/ssl/private/* /etc/ssl/private-copy/; rm -r /etc/ssl/private; mv /etc/ssl/private-copy /etc/ssl/private; chmod -R 0700 /etc/ssl/private; chown -R postgres /etc/ssl/private

ADD postgresql.conf /etc/postgresql/9.3/main/postgresql.conf
ADD pg_hba.conf /etc/postgresql/9.3/main/pg_hba.conf
RUN chown postgres:postgres /etc/postgresql/9.3/main/*.conf
ADD run /usr/local/bin/run
RUN chmod +x /usr/local/bin/run
RUN mkdir /artifactory/etc
ADD storage.properties /artifactory/etc/storage.properties
#ADD artifactory.config.import.xml /artifactory/etc/artifactory.config.import.xml
ADD http://jdbc.postgresql.org/download/postgresql-9.3-1102.jdbc41.jar /tomcat/lib/postgresql-9.3-1102.jdbc41.jar

VOLUME ["/var/lib/postgresql"]
EXPOSE 5432
EXPOSE 8080
CMD ["/usr/local/bin/run"]
