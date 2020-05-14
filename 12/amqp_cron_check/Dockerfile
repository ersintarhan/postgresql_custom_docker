FROM postgres:12

ARG DEBIAN_FRONTEND=noninteractive
ENV TZ=UTC

RUN apt update
RUN apt install -y git libcurl4-openssl-dev libicu-dev postgresql-12-cron postgresql-server-dev-12 gcc make

RUN mkdir /trash
WORKDIR /trash

RUN git clone https://github.com/omniti-labs/pg_amqp.git
RUN git clone https://github.com/okbob/plpgsql_check.git


WORKDIR /trash/pg_amqp
RUN make
RUN make install

WORKDIR /trash/plpgsql_check/
RUN make clean
RUN make install
 
WORKDIR /
RUN rm /trash -Rf

RUN echo "shared_preload_libraries = 'pg_amqp.so,pg_cron,plpgsql_check'" >> /var/lib/postgresql/data/postgresql.conf
RUN echo "cron.database_name = '${POSTGRES_DB}'" >> /var/lib/postgresql/data/postgresql.conf

RUN apt -y purge git gcc make

RUN apt autoremove --purge -y
