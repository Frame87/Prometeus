# Prometeus, node-exporter, postgres-exporter, grafana

## Getting Started:

You can add as many hosts as you like to the
`conf/prometheus.yml` file, and run it with:

Then you can add hostname:port to the prometheus scrapes config:

```yml
# node-exporter
- job_name: 'node'
    scrape_interval: 60s
    static_configs:
     - targets: ['<ip_host/dns_name_host>:9100','<ip_host/dns_name_host>:9100']
     
# postgres-exporter
- job_name: 'pg'
    scrape_interval: 60s
    static_configs:
    - targets: ['<ip_host_with_postgres/dns_name_host_with_postgres>:9187']
```

Also you need add postgres credential for postgres-exporter to the `docker-compose.yaml` file:

```yaml
  postgres-exporter:
    image: wrouesnel/postgres_exporter
    container_name: postgresexporter
    restart: always
    volumes:
      - ./queries.yaml:/etc/conf_exporter/queries.yaml
    #command:
    #  - '--disable-default-metrics'
    #  - '--disable-settings-metrics'
    environment:
      - DATA_SOURCE_URI=<ip_database>:<port>/<db_name>?sslmode=disable
      - DATA_SOURCE_USER=<user>
      - DATA_SOURCE_PASS=<password>
      - PG_EXPORTER_EXTEND_QUERY_PATH=etc/conf_exporter/queries.yaml
      #- PG_EXPORTER_DISABLE_DEFAULT_METRICS=true 
      #- PG_EXPORTER_DISABLE_SETTINGS_METRICS=true
    ports:
      - "9187:9187"
    networks:
      - monitor-net
```

## Before starts docker-compose you need create directories with privilege:

```console
cd /home/prometheus
mkdir -m 777 conf data 
mkdir grafana 
cd grafana/
mkdir -m 777 data
```

## Running it within docker-compose

```console
# docker-compose up -d 
```
