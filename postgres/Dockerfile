FROM postgres:9.6.20-alpine

ARG DATABASE=test

RUN  mkdir /backupDb \
     && echo "PATH=$PATH" > /timing-backup-postgresql
RUN  dbs=$(echo ${DATABASE} | tr ";" "\n") \
     && for db in $dbs ;\
        do \
            echo "0 2 * * * /usr/local/bin/pg_dump -h 127.0.0.1 -p 5432 -U postgres -c -C -f \"/backupDb/$db\$(date '+%Y-%m-%d_%H:%M').sql\" $db" >> /timing-backup-postgresql ;\
        done

RUN chmod 777 /backupDb
RUN chmod 777 /timing-backup-postgresql
RUN chmod 777 /var/lib/postgresql/data

RUN /usr/bin/crontab /timing-backup-postgresql

RUN echo "#!/bin/sh" > /init.sh
RUN echo "" >> /init.sh
RUN echo "# start cron" >> /init.sh
RUN echo "/usr/sbin/crond" >> /init.sh
RUN echo "" >> /init.sh
RUN echo "/docker-entrypoint.sh postgres" >> /init.sh
RUN echo "" >> /init.sh
RUN echo "sed -i 's/#tcp_keepalives_idle = 0/tcp_keepalives_idle = 60/g' /var/lib/postgresql/data/postgresql.conf" >> /init.sh
RUN echo "sed -i 's/#tcp_keepalives_interval = 0/tcp_keepalives_interval = 5/g' /var/lib/postgresql/data/postgresql.conf" >> /init.sh
RUN echo "sed -i 's/#tcp_keepalives_count = 0/tcp_keepalives_count = 5/g' /var/lib/postgresql/data/postgresql.conf" >> /init.sh
RUN chmod 777 /init.sh

ENTRYPOINT ["/init.sh"]
