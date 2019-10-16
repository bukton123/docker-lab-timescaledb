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

```

## Setup Database Zabbix
```bash
sudo -u postgres -h localhost -p 5000 createuser --pwprompt zabbix

sudo -u postgres createdb -O zabbix -E Unicode -T template0 zabbix

mkdir /downloads

cd /downloads

wget https://ufpr.dl.sourceforge.net/project/zabbix/ZABBIX%20Latest%20Stable/4.2.0/zabbix-4.2.0.tar.gz

tar -zxvf zabbix-4.2.0.tar.gz

cd zabbix-4.2.0/database/postgresql

sudo -u zabbix psql zabbix < schema.sql
sudo -u zabbix psql zabbix < images.sql
sudo -u zabbix psql zabbix < data.sql

echo "CREATE EXTENSION IF NOT EXISTS timescaledb CASCADE;" | sudo -u postgres psql zabbix

sudo -u zabbix psql zabbix < timescaledb.sql
```
