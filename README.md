Complete monitoring solution for Squid proxy using Loki, Promtail, Grafana, and Prometheus:

# Update System

sudo apt-get update

sudo apt-get upgrade -y

# Install Squid Proxy
sudo apt-get install squid -y


# Create working directory
mkdir -p /home/project/squid-stack

cd /home/ project /squid-stack

# Download Loki
wget https://github.com/grafana/loki/releases/download/v2.9.0/loki-linux-amd64.zip

unzip loki-linux-amd64.zip

chmod +x loki-linux-amd64

# Download Promtail
wget https://github.com/grafana/loki/releases/download/v2.9.0/promtail-linux-amd64.zip

unzip promtail-linux-amd64.zip

chmod +x promtail-linux-amd64

# Download Prometheus
wget https://github.com/prometheus/prometheus/releases/download/v2.47.0/prometheus-2.47.0.linux-amd64.tar.gz

tar xvfz prometheus-2.47.0.linux-amd64.tar.gz

# Download Node Exporter
wget https://github.com/prometheus/node_exporter/releases/download/v1.6.1/node_exporter-1.6.1.linux-amd64.tar.gz

tar xvfz node_exporter-1.6.1.linux-amd64.tar.gz

# Configure Squid Proxy
sudo nano /etc/squid/squid.conf

# Configure Loki
sudo mkdir -p /etc/loki

sudo nano /etc/loki/loki-config.yml

# Configure Promtail
sudo mkdir -p /etc/promtail

sudo nano /etc/promtail/ promtail -config.yml

# Configure Prometheus
sudo mkdir -p /etc/prometheus

sudo nano /etc/Prometheus-config/prometheus.yml

# Loki Service
sudo nano /etc/systemd/system/loki.service

# Promtail Service
sudo nano /etc/systemd/system/promtail.service

# Prometheus Service
sudo nano /etc/systemd/system/prometheus.service

# Node Exporter Service
sudo nano /etc/systemd/system/node_exporter.service

# Install Grafana
# Add Grafana repository
sudo apt-get install -y apt-transport-https software-properties-common wget

wget -q -O - https://packages.grafana.com/gpg.key | sudo apt-key add -

echo "deb https://packages.grafana.com/oss/deb stable main" | sudo tee -a /etc/apt/sources.list.d/grafana.list

# Install Grafana
sudo apt-get update

sudo apt-get install -y grafana

# Enable and start Grafana
sudo systemctl enable grafana-server

sudo systemctl start grafana-server

# Allow necessary ports
sudo ufw allow 3128/tcp    # Squid proxy

sudo ufw allow 3000/tcp    # Grafana

sudo ufw allow 22/tcp      # SSH

sudo ufw enable
# Start All Services

sudo squid -z

sudo systemctl enable squid

sudo systemctl restart squid

# Reload systemd
sudo systemctl daemon-reload

# Enable and start services
sudo systemctl enable loki promtail prometheus node_exporter

sudo systemctl start loki promtail prometheus node_exporter

# Check status
sudo systemctl status loki promtail prometheus node_exporter squid grafana-server


# test
curl -G "http://localhost:3100/loki/api/v1/query" --data-urlencode 'query={job="squid"}' --data-urlencode 'limit=5'

curl http://localhost:9090/graph

sudo tail -f /var/log/squid/access.log

sudo journalctl -u loki -f

sudo journalctl -u promtail -f

sudo journalctl -u prometheus -f
# Check if ports are listening
sudo netstat -tlnp | grep -E ':(3100|9080|9090|9100|3128)'

# Test Squid functionality
curl --proxy http://localhost:3128 http://example.com

# Check disk space
df -h /tmp /var/log

Access:

http://your-server-ip:3000

http://localhost:3100

http://localhost:9090


# Restart all services
sudo systemctl restart loki promtail prometheus node_exporter squid

# Stop all services
sudo systemctl stop loki promtail prometheus node_exporter squid

# Enable on boot
sudo systemctl enable loki promtail prometheus node_exporter squid grafana-server
