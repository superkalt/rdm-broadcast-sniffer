# rdm-broadcast-sniffer
Gateway RDM Broadcast to MQTT

## Setup for Microtik

### Start Sniffer

```
/tool sniffer set filter-direction=rx filter-interface=bridge filter-ip-address=10.1.11.255/32 filter-ip-protocol=udp filter-operator-between-entries= and filter-port=55010 filter-stream=yes streaming-enabled=yes streaming-server=10.1.1.2
```

### Stop Sniffer

```
/tool sniffer set streaming-enabled=no
```
