General stack:
hostapd to create the network
config at /etc/hostapd/hostapd.conf

dnsmasq as the dns service
gateway at 192.168.22.1
autoassign 100-200
pi: .1
laptop/pc: .50-.74
phones: .75-.99
config at /etc/dnsmasq.d/router.conf
make sure to
sudo systemctl restart dnsmasq
after changes
view current leases:
cat /var/lib/misc/dnsmasq.leases

iptables for firewall
saved with iptables-persistent
to save current rules:
sudo netfilter-persistent save
saved at /etc/iptables/rules.v4
iptables gives internet to ips in the ipset allowed_internet
view current rules:
sudo iptables -L INPUT -v -n --line-numbers
sudo iptables -L FORWARD -v -n --line-numbers
sudo iptables -L OUTPUT -v -n --line-numbers
sudo iptables -t nat -L POSTROUTING -v -n

ipset
config saved at /etc/ipset.conf
to add a device to allow internet access:
sudo ipset add allowed_internet 192.168.22.100
sudo ipset save | sudo tee /etc/ipset.conf > /dev/null
to remove internet access:
sudo ipset del allowed_internet 192.168.22.100
sudo ipset save | sudo tee /etc/ipset.conf > /dev/null
to view current devices:
sudo ipset list allowed_internet

docker ipset
to allow docker containers (only ha rn) access
sudo ipset add allowed_docker 172.19.0.10
to block
sudo ipset del allowed_docker 172.19.0.10
to view current
sudo ipset list allowed_docker
DO NOT SAVE HA AS allowed_docker.

ha: 172.19.0.10
config at /home/pays0n/homeassistant
using rootful docker with port publishing 8123:8123
to view logs: sudo docker logs -f homeassistant
to restart: 
	sudo docker stop homeassistant
	sudo docker ps
	sudo docker rm <container name>
	sudo docker compose up -d

If a crash happens again, save dmesg; consider disabling p2p in driver.

