# rdm-broadcast-sniffer
Gateway RDM Broadcast to MQTT

## Setup for Mikrotik

Ich binde das Script ins PPP Profil ein. Das weiterleiten von Broadcasts macht nur Sinn wenn der VPN Tunnel zur Zentrale aufgebaut ist.
Beim Abbau des VPN Tunnels wird die weiterleitung ebenfalls gestoppt.

### PPP-Profil Scripts "On Up"

[on-up](/on-up.txt)

### PPP-Profil Scripts "On Down"

[on-down](/on-down.txt)

## Scripts for NodeRed

![image](https://user-images.githubusercontent.com/44277174/173178037-622129f6-4331-44a9-bdc5-874c79692cd4.png)


```
var new_msg1 = { payload: msg.payload, topic: null, measurement: null };
 
    new_msg1.payload = msg.payload.substring(msg.payload.search("<?xml")-2,msg.payload.search("</TdbBcast>")+11);

return new_msg1;
```

```
var msg1 = { payload: msg.payload,topic:msg.topic };
var msg2 = { payload: null, topic: null};
var msg3 = { payload: null, topic: null, standort: null, controller: null};

var o = new Object(null);

controller = msg.payload.TdbBcast.controller[0];
var temp1 = controller.substring(0,controller.search("!"));
msg3.standort = temp1.substring(1,temp1.length);
var temp2 = controller.substring(controller.search("!")+1,controller.length);
msg3.controller = temp2.substring(0,temp2.length-1);

if (controller!=="") {
    node.status({fill:"green",shape:"dot",text:controller+":"+Object.keys(msg.payload.TdbBcast.item).length})
    for (i=0; i<Object.keys(msg.payload.TdbBcast.item).length; i++) 
        if (typeof(msg.payload.TdbBcast.item[i]) == "object") {
            tdb_name=msg.payload.TdbBcast.item[i]._;
            tdb_wert=msg.payload.TdbBcast.item[i].$["value"];
            if (tdb_wert=="On") {tdb_wert=1}
            if (tdb_wert=="Off") {tdb_wert=0}
            if (tdb_wert=="?????") {tdb_wert=-99}
            o[tdb_name] = Number(tdb_wert);
            msg2.topic = "projekt/"+msg3.standort+"/"+msg3.controller+"/"+tdb_name;
            msg2.payload = tdb_wert;
            node.send(msg2);
            }
                
    } else {
        node.status({fill:"red",shape:"dot",text:"Fehler"})
        }

msg.measurement = controller;
msg.payload = o;

return [null, msg];

```
