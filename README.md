# grafana-proxmox
from: https://www.apalrd.net/posts/2023/pve_influx/


# We need a few things for this
apt install gpg apt-transport-https -y

# Load GPG fingerprint and debian repo from Influx
# influxdata-archive_compat.key GPG fingerprint:
#     9D53 9D90 D332 8DC7 D6C8 D3B9 D8FF 8E1F 7DF8 B07E
wget -q https://repos.influxdata.com/influxdata-archive_compat.key
#Verify integrity of key download
echo '393e8779c89ac8d958f81f942f9ad7fb82a25e133faddaf92e15b16e6ac9ce4c influxdata-archive_compat.key' | sha256sum -c
#Dearmor key into trusted.gpg.d
cat influxdata-archive_compat.key | gpg --dearmor > /etc/apt/trusted.gpg.d/influxdata-archive_compat.gpg
#Add repo to influxdata.list
echo 'deb [signed-by=/etc/apt/trusted.gpg.d/influxdata-archive_compat.gpg] https://repos.influxdata.com/debian stable main' > /etc/apt/sources.list.d/influxdata.list

#Actually apt update and install (or update)
apt update && apt install influxdb2 -y

#Start the servyce
systemctl enable --now influxdb



#Install the signing key
wget -q -O /usr/share/keyrings/grafana.key https://apt.grafana.com/gpg.key

#Add repository to grafana.list
echo "deb [signed-by=/usr/share/keyrings/grafana.key] https://apt.grafana.com stable main" > /etc/apt/sources.list.d/grafana.list

#apt update and install (or update)
apt update && apt install grafana -y

#Start the server
systemctl enable --now grafana-server
