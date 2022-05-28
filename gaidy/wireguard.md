---
description: Не используется в итоговой версии проекта
---

# Wireguard

```bash
apt update && apt upgrade -y
apt install -y wireguard

cd /etc/wireguard
mkdir server
wg genkey | tee /etc/wireguard/server/privatekey | wg pubkey | tee /etc/wireguard/server/publickey
chmod 600 /etc/wireguard/server/privatekey

ip a
vim /etc/wireguard/wg0.conf

[Interface]
PrivateKey = <privatekey>
Address = 10.0.0.1/24
ListenPort = 51830
PostUp = iptables -A FORWARD -i %i -j ACCEPT; iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE
PostDown = iptables -D FORWARD -i %i -j ACCEPT; iptables -t nat -D POSTROUTING -o eth0 -j MASQUERADE

echo "net.ipv4.ip_forward=1" >> /etc/sysctl.conf\
sysctl -p

systemctl enable wg-quick@wg0.service
systemctl start wg-quick@wg0.service
systemctl status wg-quick@wg0.service

mkdir masterPC
wg genkey | tee /etc/wireguard/masterPC/masterPC_privatekey | wg pubkey | tee /etc/wireguard/masterPC/masterPC_publickey

mkdir robot 
wg genkey | tee /etc/wireguard/robot/robot_privatekey | wg pubkey | tee /etc/wireguard/robot/robot_publickey

systemctl restart wg-quick@wg0
systemctl status wg-quick@wg0
```
