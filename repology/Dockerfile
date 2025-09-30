# syntax=docker/dockerfile:1
FROM opensuse/tumbleweed
#RUN zypper ar -f obs://home:jirislaby/ my_home
#RUN zypper --non-interactive --gpg-auto-import-keys refresh
RUN zypper --no-refresh --non-interactive install \
  curl \
  postgresql \
  postgresql-server \
  zstd

RUN mkdir -p /run/postgresql/
RUN chown postgres:postgres /run/postgresql/
RUN chmod 1775 /run/postgresql/

USER postgres

EXPOSE 5432

COPY db.sql.zstd /tmp
RUN echo a233a
RUN /usr/share/postgresql/postgresql-script start
#RUN curl --fail --connect-timeout 2 https://dumps.repology.org/repology-database-dump-latest.sql.zst | zstd -d | psql -U repology -v ON_ERROR_STOP=1
#RUN zstd -d --stdout /tmp/db.sql.zstd | psql -U repology -v ON_ERROR_STOP=1
RUN ps aux
RUN ls /run/postgresql/
#RUN pg_ctl -D /var/lib/pgsql/data/ status
#RUN /usr/share/postgresql/postgresql-script stop

CMD [ "/usr/share/postgresql/postgresql-script", "start" ]
