# Install-Prometheus-on-Ubuntu-22.04

### Step 1 - Update System Packages
```
sudo apt update
```
### Step 2 - Create a System User for Prometheus
```
sudo groupadd --system prometheus
```
```
sudo useradd -s /sbin/nologin --system -g prometheus prometheus
```
### Step 3 - Create Directories for Prometheus
```
sudo mkdir /etc/prometheus
```
```
sudo mkdir /var/lib/prometheus
```
### Step 4 - Download Prometheus and Extract Files
```
wget https://github.com/prometheus/prometheus/releases/download/v2.43.0/prometheus-2.43.0.linux-amd64.tar.gz
```
```
tar vxf prometheus*.tar.gz
```
### Step 5 - Navigate to the Prometheus Directory
```
cd prometheus*/
```
# Configuring Prometheus on Ubuntu 22.04

### Step 1 - Move the Binary Files & Set Owner
```
sudo mv prometheus /usr/local/bin
```
```
sudo mv promtool /usr/local/bin
```
```
sudo chown prometheus:prometheus /usr/local/bin/prometheus
```
```
sudo chown prometheus:prometheus /usr/local/bin/promtool
```
### Step 2 - Move the Configuration Files & Set Owner
```
sudo mv consoles /etc/prometheus
```
```
sudo mv console_libraries /etc/prometheus
```
```
sudo mv prometheus.yml /etc/prometheus
```
Set Owner
```
sudo chown prometheus:prometheus /etc/prometheus
```
```
sudo chown -R prometheus:prometheus /etc/prometheus/consoles
```
```
sudo chown -R prometheus:prometheus /etc/prometheus/console_libraries
```
```
sudo chown -R prometheus:prometheus /var/lib/prometheus
```
The prometheus.yml file is the main Prometheus configuration file
```
vi /etc/prometheus/prometheus.yml
```
### Step 3 - Create Prometheus Systemd Service
```
vi /etc/systemd/system/prometheus.service
```
```
[Unit]
Description=Prometheus
Wants=network-online.target
After=network-online.target

[Service]
User=prometheus
Group=prometheus
Type=simple
ExecStart=/usr/local/bin/prometheus \
    --config.file /etc/prometheus/prometheus.yml \
    --storage.tsdb.path /var/lib/prometheus/ \
    --web.console.templates=/etc/prometheus/consoles \
    --web.console.libraries=/etc/prometheus/console_libraries

[Install]
WantedBy=multi-user.target
```
### Step 4 - Reload Systemd
```
sudo systemctl daemon-reload
```
### Step 5 - Start Prometheus Service
```
sudo systemctl enable prometheus
```
```
sudo systemctl start prometheus
```
### Step 6 - Check Prometheus Status
```
sudo systemctl status prometheus
```

## Access Prometheus Web Interface
```
sudo ufw allow 9090/tcp
```
With Prometheus running successfully, you can access it via your web browser using localhost:9090 or <ip_address>:9090













