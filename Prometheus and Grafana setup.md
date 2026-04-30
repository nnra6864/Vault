---
created: 2026-01-12 15:45:15
tags:
  - note
  - podman
  - docker
  - server
  - linux
  - container
  - guide
---
## Introduction

[Prometheus](https://prometheus.io/) and [Grafana](https://grafana.com/) are useful apps for monitoring devices and services.
Setting them up is extremely easy with both [Podman](https://podman.io/) and [Docker](https://www.docker.com/).
This guide will also add [Prometheus Node Exporter](https://github.com/prometheus/node_exporter) to the container as an example.

## Container

1. Create and move to a new directory
	```
	mkdir Monitor
	cd Monitor
	```
2. Create a compose.yml file
	```
	nvim compose.yml
	```
3. Paste the following contents into compose.yml
	```
	nvim compose.yml
	```
	```
	services:
	  prometheus:
	    image: docker.io/prom/prometheus:latest
	    container_name: prometheus
	    ports:
	      - "9090:9090"
	    volumes:
	      - ./prometheus_data:/prometheus
	      - ./prometheus_config:/config
	    restart: unless-stopped
	    command:
	      - '--config.file=/config/prometheus.yml'
	      - '--storage.tsdb.path=/prometheus'
	
	  grafana:
	    image: docker.io/grafana/grafana:latest
	    container_name: grafana
	    ports:
	      - "3001:3000"
	    volumes:
	      - ./grafana_data:/var/lib/grafana
	    restart: unless-stopped
	    environment:
	      - GF_SECURITY_ADMIN_USER=admin
	      - GF_SECURITY_ADMIN_PASSWORD=password
	
	  node_exporter:
	    image: quay.io/prometheus/node-exporter:latest
	    container_name: node_exporter
	    network_mode: host
	    pid: host
	    volumes:
	      - '/:/host:ro,rslave'
	    restart: unless-stopped
	    command:
	      - '--path.rootfs=/host'
	```
	
	> [!note]
	> In this example, grafana is bound to port `3001` instead of `3000` to avoid conflicts with Forgejo.
	
	> [!warning]
	> Make sure to change `admin` and `password` to your login details

4. Paste the following into setup.sh and run it
	```
	nvim setup.sh
	```
	
	```
	#!/bin/bash
	
	mkdir -p prometheus_data prometheus_config grafana_data
	
	if [ ! -f prometheus_config/prometheus.yml ]; then
	  cat > prometheus_config/prometheus.yml <<EOF
	global:
	  scrape_interval: 5s
	
	scrape_configs:
	  - job_name: 'prometheus'
	    static_configs:
	      - targets: ['localhost:9090']
	
	  - job_name: '$(hostname)'
	    static_configs:
	      - targets: ['$(hostname -i | awk "{print \$1}"):9100']
	EOF
	fi
	
	podman unshare chown -R 65534:65534 prometheus_data
	podman unshare chown -R 65534:65534 prometheus_config
	podman unshare chown -R 472:472 grafana_data
	```
	
	```
	sh setup.sh
	```
	
	> [!note]
	> This will create a few directories, as well as a default Prometheus config if one is not found.
	> It also takes care of setting correct permissions for directories, if skipped, this will lead to containers failing to start.

5. Start the container
	```
	podman compose up -d
	```
6. Open Grafana in your browser by visiting one of the following:
	1. Server IP
		```
		192.168.0.10:3001 
		```
	2. Server host name
		```
		ergasterion:3001
		```
7. Login with details set in the compose file, navigate to `Connections -> Data Sources` and add a new Prometheus data source with the following url
	```
	http://prometheus:9090
	```
8. Visit [Grafana Dashboards](https://grafana.com/grafana/dashboards/?dataSource=prometheus), find data sources you'd like to add, e.g. [Node Exporter Full](https://grafana.com/grafana/dashboards/1860-node-exporter-full/), and copy ID to clipboard.
9. Add the dashboard to Grafana
	1. Go back to the Grafana site
	2. Navigate to Dashboards in the sidebar
	3. Click on New and choose Import
	4. Paste the ID you copied earlier and press Load
	5. Select the Prometheus data source if needed
10. Enjoy

## Monitoring other devices

Monitoring other devices is extremely easy.

> [!note]
> Keep in mind that not all dashboards and data sets are compatible.
> For example, you can't use Node Exporter Full dashboard with the Windows Exporter.

### Linux

Lets use Arch as an example.
On the machine to be monitored, lets call it pc, install the needed package for monitoring.

1. Install the `prometheus-node-exporter` package
	```
	sudo pacman -Sy prometheus-node-exporter
	```
2. Start and enable the service
	```
	systemctl enable --now prometheus-node-exporter
	```

### Windows

1. Install the `windows_exporter` package
	```
	winget install -e --id Prometheus.WindowsExporter
	```
2. Add the latest [Windows Exporter Dashboard](https://grafana.com/grafana/dashboards/24390-windows-exporter-dashboard-2025/) to Grafana, at the time of writing this is 2025

### Both

1. Back on in the container, add the following to your Prometheus config
	```
	- job_name: 'pc'
      static_configs:
        - targets: ['pc:9100']
	```

	> [!note]
	> Replace `pc` in `job_name` with the name you'd like displayed in the UI

	> [!note]
	> Replace `pc` in `targets` with either host name or ip of the device you want to monitor

2. Restart the container service
	```
	podman compose restart
	```

You should now be able to select that device in Grafana dashboard of your choice.

## Sources

[Prometheus](https://prometheus.io/)
[Grafana](https://grafana.com/)
[Prometheus Node Exporter](https://github.com/prometheus/node_exporter)
[Prometheus Windows Exporter](https://github.com/prometheus-community/windows_exporter/releases)
[Prometheus Node Exporter Arch Package](https://archlinux.org/packages/extra/x86_64/prometheus-node-exporter/)
[Prometheus Windows Exporter Windows Package](https://winstall.app/apps/Prometheus.WindowsExporter)
[Node Exporter Full](https://grafana.com/grafana/dashboards/1860-node-exporter-full/)
[Windows Exporter Dashboard](https://grafana.com/grafana/dashboards/24390-windows-exporter-dashboard-2025/)
[Podman](https://podman.io/)
[Docker](https://www.docker.com/)
[Video tutorial](https://www.youtube.com/watch?v=9TJx7QTrTyo)
[Initial Claude chat](https://claude.ai/chat/d428dc56-5b79-42d4-be10-02327d075346)
[Last Claude chat](https://claude.ai/chat/0a77bf43-17a4-4d00-932e-b7acd63ab9b1)
[GPT help with networking](https://chatgpt.com/c/69651c02-5ce8-8331-8a2a-465703b5cad2)

## References

[[Podman]]
[[Docker]]
[[Prometheus]]
[[Prometheus Node Exporter]]
[[Grafana]]
[[Forgejo]]
[[Arch]]