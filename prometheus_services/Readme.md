# Logging and monitoring service  
Use Grafana to monitor logs and metrics. Logging is provided by Loki. We are using the python package `loguru` which has been configured to send logs directly to Loki. Metrics are scraped using Prometheus. 

## Setup
Create the following directories: `grafana-storage`, `loki-data`, and `prometheus-data`. This will enable persistent storage between docker sessions.

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