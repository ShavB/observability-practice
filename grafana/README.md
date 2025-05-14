# Grafana with loki and promtail

### Grafana

Grafana is a analytics visualization platform (acts as visualization layer). It is used to create interactive, customizable dashbords for monitoring metrics, logs and other data from variety of sources.

Grafana does not collect data it self it uses different tools like

1. Prometheus (Metrics monitoring tool)
2. inFluxDb (time series data)
3. Loki (logs)
4. Elastic search
5. Cloud services (AWS, Azure, GCP)
6. DataDog, Splunk, Dynatrace (uses API's or Plugins)

Key features:

1. Custom Dashbords
2. Templating
3. Alerting (Alert on conditions) - (Set Alerts onm etrics eg: CPU > 95%) and send data alert on slack, etc
4. User Management
5. Data Exploration

### Will use Proxmox LXC container here instead of any Cloud services

### Install Grafana on linux(Ubuntu/Debian)

```bash
sudo apt-get install -y apt-transport-https
sudo apt-get install -y software-properties-common wget
sudo wget -q -O /usr/share/keyrings/grafana.key
https://apt.grafana.com/gpg.key
```

#### Grafana stable keys

```bash
echo "deb [signed-by=/usr/share/keyrings/grafana.key] https://apt.grafana.com stable main" | sudo tee -a /etc/apt/sources.list.d/grafana.list
```

#### Install Grafana

```bash
sudo apt-get update
sudo apt-get install grafana
```

#### start Grafana server

sudo systemctl start grafana-server

#### enable grafana for system restart

sudo systemctl enable grafana-server

#### check grafana-server status

sudo systemctl status grafana-server

#### grafana web ui - open in a browser

http://<you-ip>:3000
default user : admin
password : admin

#### Install Loki using docker

config file for loki:

```bash
wget https://raw.githubusercontent.com/grafana/loki/v2.8.0/cmd/loki/loki-local-config.yaml -O loki-config.yaml
```

#### Run Logi Docker container

```bash
docker run -d --name loki -v $(pwd):/mnt/config -p 3100:3100 grafana/loki:2.8.0 --config.file=/mnt/config/loki-config.yaml
```

#### Install promtail using docker

Config file for promtail:

```bash
wget https://raw.githubusercontent.com/grafana/loki/v2.8.0/clients/cmd/promtail/promtail-docker-config.yaml -O promtail-config.yaml
```

#### Run Logi Docker container

```bash
docker run -d --name promtail -v $(pwd):/mnt/config -v /var/log:/var/log --link loki grafana/promtail:2.8.0 --config.file=/mnt/config/promtail-config.yaml
```

#### Created Dashbord for nginx and system errors

![alt text](<Screenshot 2025-05-14 at 7.04.39 PM.png>)
