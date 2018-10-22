# SEPABins
The repository includes all you need to test SEPA, a publish-subscribe architecture designed to support information level interoperability by means of Semantic Web technologies. The architecture is built on top of a generic SPARQL endpoint where publishers and subscribers use standard **SPARQL** [Updates](https://www.w3.org/TR/sparql11-update/) and [Queries](https://www.w3.org/TR/sparql11-query/). Notifications about events (i.e., changes in the [**RDF**](https://www.w3.org/RDF/) knowledge base) are expressed in terms of [added and removed SPARQL binding results](http://mml.arces.unibo.it/TR/sparql11-subscribe.html) since the previous notification. 

Developers can benefit of a set of API implementing a [Producer-Aggregator-Consumer design pattern](http://mml.arces.unibo.it/TR/jsap.html).

For more details on the current implementation and how you can contribute please follow this [link](https://github.com/arces-wot/SEPA).

## Installation
Clone the repository: `git clone https://github.com/arces-wot/SEPABins.git`

## Usage
The SEPA broker needs to connect to a SPARQL endpoint supporting the [SPARQL 1.1 Protocol](https://www.w3.org/TR/sparql11-protocol/). For you convenience, the repository includes an instance of [Blazegraph](https://www.blazegraph.com/).

### Start the SPARQL endpoint
1. Open a command shell
2. Move to the `endpoint` folder
3. Run the endpoint: `java -server -Xmx4g -jar blazegraph.jar`

If Blazegraph started correctly, you would see something like the following:

```
Welcome to the Blazegraph(tm) Database.

Go to http://192.168.1.11:9999/blazegraph/ to get started.
```
Otherwise, Blazegraph failed to start. This can be due to the JRE version (it seems that Java 9 is not supported). If you see the following message:

```
ERROR: Banner.java:160: Uncaught exception in thread
java.lang.NullPointerException
	at com.bigdata.rdf.sail.webapp.StandaloneNanoSparqlServer.main(StandaloneNanoSparqlServer.java:142)
```
try use the Java 8 JRE as follows:

`/Library/Java/JavaVirtualMachines/jdk1.8.0_65.jdk/Contents/Home/bin/java -server -Xmx4g -jar blazegraph.jar` 

### Start the SEPA broker
1. Open a command shell
2. Move to the `engine` folder
3. Run the broker: `java -jar SEPAEngine_X_Y_Z.jar`

```
##########################################################################################
# SEPA Broker                                                                            #
# Dynamic Linked Data & Web of Things Research - University of Bologna (Italy)           #
# Copyright (C) 2016-2018                                                                #
# This program comes with ABSOLUTELY NO WARRANTY                                         #
# This is free software, and you are welcome to redistribute it under certain conditions #
# GNU GENERAL PUBLIC LICENSE, Version 3, 29 June 2007                                    #
#                                                                                        #
# GITHUB: https://github.com/arces-wot/sepa                                              #
# WEB: http://site.unibo.it/wot                                                          #
# WIKI: https://github.com/arces-wot/SEPA/wiki                                           #
##########################################################################################
SPARQL 1.1 endpoint
----------------------
SPARQL 1.1 Query     | http://localhost:9999/blazegraph/namespace/kb/sparql (Method: POST)
SPARQL 1.1 Update    | http://localhost:9999/blazegraph/namespace/kb/sparql (Method: POST)
----------------------

SPARQL 1.1 Protocol (https://www.w3.org/TR/sparql11-protocol/)
----------------------
SPARQL 1.1 Query     | http://192.168.1.11:8000/query
SPARQL 1.1 Update    | http://192.168.1.11:8000/update
----------------------

SPARQL 1.1 SE Protocol (http://mml.arces.unibo.it/TR/sparql11-se-protocol/)
----------------------
SPARQL 1.1 Subscribe | ws://192.168.1.11:9000/subscribe
----------------------

*****************************************************************************************
*                      SEPA Broker Ver 0.9.6 is up and running                          *
*                                Let Things Talk!                                       *
*****************************************************************************************
```

To close a running instance of a broker just type `CTRL+X`:

```
Stopping HTTP gate...
Stopping WebSocket gate...
Stopping Processor...
Stopped...bye bye :-)
```
## Play with SEPA
SEPA applications are built around the [JSON Semantic Application Profile (JSAP)](http://mml.arces.unibo.it/TR/jsap.html).  An example of an IoT application can be found in the `Apps/mqtt` folder. The JSAP file is named `arces-demo.jsap`.

### MQTTDemo
1. Open a command shell
2. Move to the `Apps/mqtt` folder
3. Run the demo: `java -jar MQTTDemo.jar arces-demo.jsap`

The output should look like:

```
2018-10-22T12:17:00,335 [INFO ] main (MQTTDemo.java:45) Parse places
2018-10-22T12:17:00,352 [INFO ] main (MQTTDemo.java:51) Add places
2018-10-22T12:17:00,398 [INFO ] main (MQTTDemo.java:70) Parse places' links
2018-10-22T12:17:00,411 [INFO ] main (MQTTDemo.java:77) Link places
2018-10-22T12:17:00,424 [INFO ] main (MQTTDemo.java:95) Parse semantic mappings
2018-10-22T12:17:00,452 [INFO ] main (MQTTDemo.java:102) Add observations
2018-10-22T12:17:01,241 [INFO ] main (MQTTSmartifier.java:281) Creating MQTT client...
2018-10-22T12:17:01,274 [INFO ] main (MQTTSmartifier.java:283) Client ID: paho1540203421274000000
2018-10-22T12:17:01,274 [INFO ] main (MQTTSmartifier.java:284) Server URI: tcp://giove.arces.unibo.it:52877
2018-10-22T12:17:01,314 [INFO ] main (MQTTSmartifier.java:306) Connecting...
2018-10-22T12:17:01,463 [INFO ] main (MQTTSmartifier.java:315) Subscribing...
2018-10-22T12:17:01,493 [INFO ] main (MQTTSmartifier.java:320) MQTT client paho1540203421274000000 subscribed to tcp://giove.arces.unibo.it:52877 Topic filter #
2018-10-22T12:17:01,494 [INFO ] main (WebsocketSubscriptionProtocol.java:103) Connect to: ws://localhost:9000/subscribe
2018-10-22T12:17:01,856 [INFO ] Grizzly(1) (WebsocketSubscriptionProtocol.java:166) @onOpen session: f185c0e5-f7d1-4d0e-93d7-796c161b8bd5
2018-10-22T12:17:01,864 [INFO ] main (MQTTDemo.java:130) Press any key to exit...
```

For each new observed sensor value a message would be printed:

```
2018-10-22T12:17:07,018 [INFO ] MQTT Call: paho1540203421274000000 (MQTTSmartifier.java:190) Message received: arces/servers/ares/ercole/cpu/core-1/temperature 67
```
### SEPA dashboard 
In the `Tools` folder, you can find an application (`SEPADashboard_X_Y_Z.jar`) that allows you to interact and experiment the functionalities offered by SEPA. Just double click on the jar or run it from a command shell as: `java -jar Dashboard_X_Y_Z.jar` 

1. Click on the `Load JSAP` button. Move to the `Apps/mqtt` and locate the `arces-demo.jsap`. Open it.
2. Query for observations: select `OBSERVATIONS` on the right box and click `QUERY`

![](dashboard1.png)

Now it's time to use the SEPA magic...press `SUBSCRIBE`. A new tab is opened with the notifications on sensor data changes. 

![](dashboard2.png)

## Configure
The broker uses two configuration files: `engine.jpar` and `endpoint.jpar`. The former contains the broker configuration parameters, while the latter contains the endpoint configuration parameters. If these files are not present, they are created with defaults the first time the broker is executed. The following message logs will be shown:

```
[WARN ] main engine.jpar (No such file or directory)
[WARN ] main USING DEFAULTS. Edit "engine.jpar" (if needed) and run again the broker
[WARN ] main endpoint.jpar (No such file or directory)
[WARN ] main USING DEFAULTS. Edit "endpoint.jpar" (if needed) and run again the broker
```

SEPA broker configuration parameters are stored in a JSON file (named `engine.jpar`):

```json
{
	"parameters": {
		"scheduler": {
			"queueSize": 100,
			"timeout" : 5000
		},
		"processor": {
			"updateTimeout": 60000,
			"queryTimeout": 60000,
			"maxConcurrentRequests": 5,
			"reliableUpdate" : true
		},
		"spu": {
			"timeout": 5000
		},
		"gates": {
			"secure": false,
			"paths": {
				"securePath": "/secure",
				"update": "/update",
				"query": "/query",
				"subscribe": "/subscribe",
				"register": "/oauth/register",
				"tokenRequest": "/oauth/token"
			},
			"ports": {
				"http": 8000,
				"https": 8443,
				"ws": 9000,
				"wss": 9443
			}
		}
	}
}
```
The `ports`, `paths` and `secure` members within the `gates` object are used to specify the URLs at which the broker is listening for requests. The above default configuration initializes the broker has shown here:

```
SPARQL 1.1 Query     | http://<your local address>:8000/query
SPARQL 1.1 Update    | http://<your local address>:8000/update
SPARQL 1.1 Subscribe | ws://<your local address>:9000/subscribe
```

Timeouts on update and query processing on the SPARQL endpoint are specified respectively by the `updateTimeout` and `queryTimeout` members. It also possible to setup the maximum number of concurrent requests that can be processed by the endpoint (see `maxConcurrentRequests`). The `timeout` member of the `spu` allows to specify the timeout subscription processing. Eventually, the `queueSize`is the maximum number of pending requests after which the broker starts to deny new requests.

The SPARQL 1.1 protocol parameters of the SPARQL endpoint are specified in the `endpoint.jpar` file:
```
{
	"host": "localhost",
	"sparql11protocol": {
		"protocol": "http",
		"port": 9999,
		"query": {
			"path": "/blazegraph/namespace/kb/sparql",
			"method": "POST",
			"format": "JSON"
		},
		"update": {
			"path": "/blazegraph/namespace/kb/sparql",
			"method": "POST",
			"format": "JSON"
		}
	}
}
```

## Security issues
The broker security can be enable by setting to `true` the `secure` member of the `engine.jpar` file. The broker uses a JKS for storing the keys and certificates for [SSL](http://docs.oracle.com/cd/E19509-01/820-3503/6nf1il6ek/index.html) and [JWT](https://tools.ietf.org/html/rfc7519) signing/verification. A default `sepa.jks` is provided including a single X.509 certificate (the password for both the store and the key is: `sepa2017`). If you face problems using the provided JKS, please create a new one as follows: `keytool -genkey -keyalg RSA -alias sepakey -keystore sepa.jks -storepass sepa2017 -validity 360 -keysize 2048`
The SEPA broker allows to use a user generated JKS. Run `java -jar engine-x.y.z.jar -help` for a list of options. The Java [Keytool](https://docs.oracle.com/javase/6/docs/technotes/tools/solaris/keytool.html) can be used to create, access and modify a JKS. 

## JMX monitoring
The SEPA broker is also distributed with a default [JMX](http://www.oracle.com/technetwork/articles/java/javamanagement-140525.html) configuration `jmx.properties` (including the `jmxremote.password` and `jmxremote.access` files for password and user grants). Remember to change password file permissions using: `chmod 600 jmxremote.password`. To enable remote JMX, the broker must be run as follows: `java -Dcom.sun.management.config.file=jmx.properties -jar engine-x.y.z.jar`. Using [`jconsole`](http://docs.oracle.com/javase/7/docs/technotes/guides/management/jconsole.html) is possible to monitor and control the most important broker parameters. By default, the port is `5555` and the `root:root` credentials grant full control (read/write).


