MQTT-ST: a Mosquitto fork
=================

MQTT-ST is a MQTT broker which is able to create a distribute architecture of brokers, organized thorugh a spanning tree.
It based on the open source Mosquitto broker implementation. Our version mainly adds a feature for interconnecting MQTT brokers automatically in a loop-free topology. Other Mosquitto features remain inalterated. 

## Links

See the following links for more information on MQTT-ST:

- Arxiv paper: <https://arxiv.org/pdf/1911.07622.pdf>
- Source code Mosquitto repository: <https://github.com/eclipse/mosquitto>
- Source code repository: <https://github.com/edoardesd/mosquitto>

Creator information is available at the following locations:

- Research group webpage: <http://www.antlab.polimi.it>
- Developer github: <https://github.com/edoardesd>


## Installing

Installing MQTT-ST is as installing Mosquitto. 
See the official documentation at <https://mosquitto.org/download/> for details on installing binaries for
various platforms. 

## Cluster quick start

Also the quick start for a single broker is the same as Mosquitto. More information at: <https://github.com/eclipse/mosquitto#quick-start>.

If you want to create a cluster of brokers, each broker must be spawned manually with a valid configuration file, i.e. `mosquitto -c mosquitto.conf`

## Configuration file

MQTT-ST provides a mechanism for interconnecting automatically Mosquitto brokers in a loop free fashion. It is based on the bridging feature provided by Mosquitto (<https://mosquitto.org/man/mosquitto-conf-5.html>). 

The bridges are created through the `mosquitto.conf` file. Every broker in the cluster must have a valid configuration file. The file includes a valid bridge to all the others brokers in the cluster. It consists in:
- arbitrary connection name
- broker destination address
- broker destination port (if different to the default port 1883)
- topic included in the cluster
- topic forwarding option: it **must** be `out` for ensuring the correct behaviour. See <https://mosquitto.org/man/mosquitto-conf-5.html> for more details
- QoS option: high QoS is suggested
- remote broker id: anyone

### Example
Here we show an example of a cluster composed by MQTT-ST brokers. For the sake of simplicity, only 3 brokers are used: A, B and C. However, MQTT-ST can support as many brokers as your machine can start.

Every broker in the cluster must be bridged to every one else. Each broker has a different .conf file containing bridges towards all the others brokers.

For instance, broker A configuration file:
```
connection AtoB
address 192.168.1.3:1883
topic # out 2
remote_clientid AtoB

connection AtoC
address 192.168.1.4:1883
topic # out 2
remote_clientid AtoC
```
Broker C configuration file:
```
connection BtoA
address 192.168.1.2:1883
topic # out 2
remote_clientid BtoA

connection BtoC
address 192.168.1.4:1883
topic # out 2
remote_clientid BtoC
```
And so on...

## Future works
- Develop a script which creates valid configuration files
- Automatic discovery of MQTT-SN brokers
- Create a tree for each topic in the syste 
