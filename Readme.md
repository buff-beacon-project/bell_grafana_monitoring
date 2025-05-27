# Logging and monitoring service  
Use Grafana to monitor logs and metrics. Logging is provided by Loki. We are using the python package `loguru` which has been configured to send logs directly to Loki. Metrics are scraped using Prometheus. 

## Setup
Create the following directories: `grafana-storage`, `loki-data`, and `prometheus-data`. This will enable persistent storage between docker sessions.

These may need to operate with the correct user permissions corresponding to the Grafana, Loki, and Prometheus user IDs.

```
mkdir -p grafana-storage
sudo chown -R 472:472 grafana-storage
```

```
mkdir -p loki-data
sudo chown -R 10001:10001 loki-data
chmod 644 loki-config.yml
```

```
mkdir -p prometheus-data
sudo chown -R 65534:65534 prometheus-data
sudo chown -R 65534:65534 prometheus_services
chmod 644 prometheus.yml
```


### Prometheus setup
For each service that you wish to scrape metrics for, add a `JSON` file to the the `prometheus-services` directory. It should have this format. Metrics are expected to be scraped from `http://ip:port/metrics`.

```
[
    {
        "targets": [
            "ip:port"
        ],
        "labels": {
            "job": "service_name",
            "instance": "service_name_01"
        }
    }
]
```


## Running
From the command line, type:

```
docker-compose up -d
```

To stop:
```
docker-compose down
```

# Configuring Grafana  
We need to add a Loki datasource and a Prometheus data source.

## Loki Setup  
datasource url: http://loki:3100 

## Prometheus Setup  
datasource url: http://prometheus:9090
Prometheus Type: Prometheus
Fetch method: GET