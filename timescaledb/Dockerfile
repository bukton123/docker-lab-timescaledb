FROM timescale/timescaledb:1.7.4-pg11
#FROM timescale/timescaledb-postgis:12.1.0-pg1

LABEL MAINTAINER BukTon

RUN echo "**** install Python ****" && \
    apk add --no-cache python3 python3-dev && \
    if [ ! -e /usr/bin/python ]; then ln -sf python3 /usr/bin/python ; fi && \
    \
    echo "**** install pip ****" && \
    python3 -m ensurepip && \
    rm -r /usr/lib/python*/ensurepip && \
    pip3 install --no-cache --upgrade pip setuptools wheel && \
    if [ ! -e /usr/bin/pip ]; then ln -s pip3 /usr/bin/pip ; fi

RUN apk update && apk --no-cache add gcc g++ libpq linux-headers curl
RUN pip3 install --upgrade pi

# install patroni
RUN pip install psycopg2
RUN pip install patroni
RUN pip install python-etcd
RUN ln -s /usr/local/bin/pg_* /usr/sbin/
RUN mkdir -p /data/patroni \
  && chown postgres:postgres /data/patroni \
  && chown postgres:postgres /data \
  && chmod 700 /data/patroni

# create host remote poostgres only
# RUN echo "host all all 0.0.0.0/0 md5" >> /var/lib/postgresql/data/pg_hba.conf

USER postgres

WORKDIR /data/patroni

EXPOSE 5432 8008

CMD patroni /etc/patroni.yml
