# rdm-broadcast-sniffer
Gateway RDM Broadcast to MQTT

## Setup for Microtik

Ich binde das Script ins PPP Profil ein. Das weiterleiten von Broadcasts macht nur Sinn wenn der VPN Tunnel zur Zentrale aufgebaut ist.
Beim Abbau des VPN Tunnels wird die weiterleitung ebenfalls gestoppt.

### PPP-Profil Scripts "On Up"

```
/ip route add dst-address=10.1.1.0/24 gateway="superkalt" distance=99
/tool sniffer set filter-direction=rx filter-interface=bridge filter-ip-address=10.1.11.255/32 filter-ip-protocol=udp filter-operator-between-entries= and filter-port=55010 filter-stream=yes streaming-enabled=yes streaming-server=10.1.1.2 streaming-port=37008
```

### PPP-Profil Scripts "On Up"

```
/ip route remove [find dst-address="10.1.1.0/24"]
/tool sniffer set streaming-enabled=no

```

## Scripts for NodeRed

```
hier fehlt noch was
```
