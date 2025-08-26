# Influx DB

## Getting Started

```sh
influxdb3 create token --admin
influxdb3 create database demo
```

* Configure Environment Variable:

* export INFLUXDB3_AUTH_TOKEN=

## Deploy

```yaml
services:
  influxdb3-core:
    image: influxdb:3-core
    ports:
      - 8181:8181
    command:
      - influxdb3
      - serve
      - --node-id=node0
      - --object-store=file
      - --data-dir=/var/lib/influxdb3/data
      - --plugin-dir=/var/lib/influxdb3/plugins
    user: "0:0" # Root user => don't use on production
    volumes:
      - influxdb-data:/var/lib/influxdb3/data
      - influxdb-plugins:/etc/influxdb3/plugins
  grafana:
    image: grafana/grafana:latest
    ports:
      - 3000:3000
    volumes:
      - grafana-storage:/var/lib/grafana

volumes:
  influxdb-data:
  influxdb-plugins:
  grafana-storage:
```

