# syntax=docker/dockerfile:1
FROM opensuse/tumbleweed
#RUN zypper ar -f obs://home:jirislaby/ my_home
#RUN zypper --non-interactive --gpg-auto-import-keys refresh
RUN zypper --no-refresh --non-interactive install \
  curl \
  postgresql \
  postgresql-server-implementation \
  zstd

#RUN useradd -r -m -U -d /var/lib/postgresql -s /bin/bash postgres
RUN ls -lR /var/lib/pgsql
RUN mkdir -p /var/lib/pgsql/data
RUN mkdir -p /run/postgresql/
RUN chown -R postgres:postgres /var/lib/pgsql/data /run/postgresql/

USER postgres
RUN initdb -D /var/lib/pgsql/data
RUN echo ahoj5
RUN postgres -D /tmp/psql &
RUN ls -lR /run/postgresql/ /var/lib/pgsql/

EXPOSE 5432

#RUN curl --fail --connect-timeout 2 https://dumps.repology.org/repology-database-dump-latest.sql.zst | zstd -d | psql -U repology -v ON_ERROR_STOP=1
COPY db.sql.zstd /tmp
RUN zstd -d --stdout /tmp/db.sql.zstd | psql -U repology -v ON_ERROR_STOP=1

CMD ["postgres", "-D", "/var/lib/postgresql/data"]
