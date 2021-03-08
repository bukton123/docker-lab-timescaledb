# docker-lab-timescalesb

## Setup Step Docker Compose
```bash
# recommend
docker-compose up -d etcd

docker-compose up --build -d timescaledb1

docker-compose up --build -d timescaledb2

docker-compose up --build -d timescaledb3

docker-compose up -d haproxy

# grafana monitoring postgres, timescaledb
docker-compose up -d grafana

# zabbix server, zabbix web nginx
docker-compose up -d zabbix

docker-compose up -d zabbixweb

# pgadmin for psql gui access
docker-compose up -d pgadmin

```

## Upgrade to timescalesb
```bash
PGPORT=5000; export PGPORT
PGPASSWORD=password; export PGPASSWORD
PGHOSTADDR=192.168.144.6; export PGHOSTADDR
PGUSER=postgres; export PGUSER

createuser --pwprompt zabbix
createdb -O zabbix -E Unicode -T template0 zabbix
cd /tmp

wget https://cdn.zabbix.com/zabbix/sources/stable/5.2/zabbix-5.2.5.tar.gz 
tar -zxvf zabbix-5.2.5.tar.gz
cd zabbix-5.2.5/database/postgresql

PGUSER=zabbix; export PGUSER
PGPASSWORD=zabbix; export PGPASSWORD

psql zabbix < schema.sql
psql zabbix < images.sql
psql zabbix < data.sql

PGPASSWORD=password psql -U postgres -h 192.168.144.6 --dbname=zabbix -p 5000 -c "CREATE EXTENSION IF NOT EXISTS timescaledb CASCADE;"
PGPASSWORD=zabbix psql -U zabbix -h 192.168.144.6 -p 5000 -c "\dx"

psql zabbix < timescaledb.sql


cd ~/docker-lab-timescaledb
docker-compose up -d zabbix

PGPASSWORD=zabbix psql -U zabbix -h 192.168.144.6 -p 5000 -c "\dx timescaledb"
```
