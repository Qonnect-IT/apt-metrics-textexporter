# apt-metrics-textexporter

APT Metrics exporter compatible with Ubuntu 18 and higher. 

Original project from: [Tom Henderson](https://tom-henderson.github.io/2020/12/04/apt-grafana-prometheus.html)

## Dependencies ##
* prometheus-node-exporter
* prometheus-node-exporter-collectors
* sponge (part of moreutils)

Install with: ```apt update && apt install prometheus-node-exporter prometheus-node-exporter-collectors moreutils```

## Usage ##

1. Copy apt-metrics to /usr/share/apt-metrics
2. Copy 60prometheus-metrics to /etc/apt/apt.conf.d/60prometheus-metrics
3. if you have not already enabled text-metrics, add the following line to /etc/default/prometheus-node-exporter
   ```--collector.textfile.directory=/var/lib/prometheus/node-exporter"``` 

## Reason for creating this repo ##
The provided configs by Tom Henderson did not work (anymore) on my ubuntu version, mainly because of /usr/lib/update-notifier/apt-check returning its output via stderr for some reason
