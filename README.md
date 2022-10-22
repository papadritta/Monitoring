# Monitoring &amp; Alerting solution for Calamari Network (Monitoring Stack)

>**The Polkadot & Kusama ecosystems have raised an incredible attention in the past few years.**

>**Calamari Network is a private layer built for the entire Kusama ecosystem. Built on the substrate framework, Calamari Network is natively compatible with other projects and parachain assets including wrapped major cryptoassets.**

>**Calamari, Manta Network's canary net, is the plug-and-play privacy-preservation parachain built to serve the Kusama world.**

>**The Calamari parachain will be Privacy Hub for Kusama with multiple privacy solutions for all web3 projects.**

>**Many individuals joined the adventure and set up their own validator node, which is extraordinary for decentralization. However, maintaining a validator node is a huge responsibility as you are basically securing millions of dollars on the blockchain. Health and security of your node have to be the top priority for you as a validator.**



**NOTE: THIS GUIDE PROVIDES HELPFUL MONITORING AND ALERTING SOLUTION FOR CALAMARI VALIDATORS, BUT MOST OF THE CONFIGURATION IS EXACTLY THE SAME FOR A POLKADOT, KUSAMA, OR ANY SUBSTRATE BASED NODE.**



## The set of services required:

## 1. Set UFW (open ports)
```
sudo ufw allow 9100
sudo ufw allow 9090
sudo ufw allow 9093
sudo ufw allow 9615
sudo ufw allow 3000
```
**NOTE: This is additional rules for UFW. Don't forget to set all the rules and add Default ports: 22, 80, 443 for your SSH client to avoid loose the control of your server.**

## 2. Node-exporter [Check the lastest release](https://github.com/prometheus/node_exporter/releases)

#### Installation

- create user
```
sudo useradd --no-create-home --shell /bin/false node_exporter
```

- download last version
```
wget https://github.com/prometheus/node_exporter/releases/download/v1.4.0/node_exporter-1.4.0.linux-amd64.tar.gz
```

- check sha256sum
```
sha256sum node_exporter-1.4.0.linux-amd64.tar.gz
```

- extract the archive
```
tar xvf node_exporter-1.4.0.linux-amd64.tar.gz
```

- copy the binary to the following location and set ownership
```
sudo cp node_exporter-1.4.0.linux-amd64/node_exporter /usr/local/bin
sudo chown -R node_exporter:node_exporter /usr/local/bin/node_exporter
```
- remove the download leftovers
```
rm -rf node_exporter-1.4.0.linux-amd64
rm node_exporter-1.4.0.linux-amd64.tar.gz
```
#### Setting service

- Create a systemd service
```
sudo nano /etc/systemd/system/node_exporter.service
```

- Copy & paste
```
[Unit]
Description=Node Exporter
Wants=network-online.target
After=network-online.target

[Service]
Type=simple
User=node_exporter
Group=node_exporter
Restart=always
RestartSec=5
ExecStart=/usr/local/bin/node_exporter --web.listen-address="localhost:9100"

[Install]
WantedBy=multi-user.target
```
- Reload systemd
```
sudo systemctl daemon-reload
```

- Start the service and check the status
```
sudo systemctl start node_exporter
sudo systemctl status node_exporter
```

- Enable Node Exporter to start on boot
```
sudo systemctl enable node_exporter
```



