# SEPABins
The repository includes all you need to test SEPA, a publish-subscribe architecture designed to support information level interoperability by means of Semantic Web technologies. The architecture is built on top of a generic SPARQL endpoint where publishers and subscribers use standard **SPARQL** [Updates](https://www.w3.org/TR/sparql11-update/) and [Queries](https://www.w3.org/TR/sparql11-query/). Notifications about events (i.e., changes in the [**RDF**](https://www.w3.org/RDF/) knowledge base) are expressed in terms of [added and removed SPARQL binding results](http://mml.arces.unibo.it/TR/sparql11-subscribe.html) since the previous notification. 

Developers can benefit of a set of API implementing a [Producer-Aggregator-Consumer design pattern](http://mml.arces.unibo.it/TR/jsap.html).

For more details on the current implementation and how you can contribute please follow this [link](https://github.com/arces-wot/SEPA).

## Installation
Clone the repository: `git clone https://github.com/arces-wot/SEPABins.git`

## Usage
The SEPA broker needs to connect to a SPARQL endpoint supporting the [SPARQL 1.1 Protocol](https://www.w3.org/TR/sparql11-protocol/). For you convenience, the repository includes an instance of [Blazegraph](https://www.blazegraph.com/).

### Start the SPARQL endpoint
1. Move to the `endpoint` folder
2. Run the endpoint: `java -server -Xmx4g -jar blazegraph.jar`

### Start the SEPA broker
1. Move to the `engine` folder
2. Run the broker: `java -jar SEPAEngine_X_Y_Z.jar`

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
# WIKI: https://github.com/arces-wot/SEPA/wiki                                         #
##########################################################################################

SPARQL 1.1 endpoint
----------------------
SPARQL 1.1 Query     | http://localhost:9999/blazegraph/namespace/kb/sparql (Method: POST)
SPARQL 1.1 Update    | http://localhost:9999/blazegraph/namespace/kb/sparql (Method: POST)
----------------------

SPARQL 1.1 Protocol (https://www.w3.org/TR/sparql11-protocol/)
----------------------
SPARQL 1.1 Query     | http://<your local address>:8000/query
SPARQL 1.1 Update    | http://<your local address>:8000/update
----------------------

SPARQL 1.1 SE Protocol (https://mml.arces.unibo.it/TR/sparql11-se-protocol/)
----------------------
SPARQL 1.1 Subscribe | ws://<your local address>:9000/subscribe
----------------------

*****************************************************************************************
*                      SEPA Broker Ver 0.9.1 is up and running                          *
*                                Let Things Talk!                                       *
*****************************************************************************************
```

The broker uses two configuration files: `engine.jpar` and `endpoint.jpar`. The former contains the broker configuration parameters, while the latter contains the endpoint configuration parameters. If these files are not present, they are created with defaults the first time the broker is executed. The following message logs will be shown:

```
[WARN ] main engine.jpar (No such file or directory)
[WARN ] main USING DEFAULTS. Edit "engine.jpar" (if needed) and run again the broker
[WARN ] main endpoint.jpar (No such file or directory)
[WARN ] main USING DEFAULTS. Edit "endpoint.jpar" (if needed) and run again the broker
```

To close a running instance of a broker just type `CTRL+X`:

```
Stopping...
Stopping HTTP gate...
Stopping WS gate...
Stopped...bye bye :-)
```

## Configure
SEPA broker configuration parameters are stored in a JSON file (named `engine.jpar`) like the following:

```json
{
	"parameters": {
		"scheduler": {
			"queueSize": 100
		},
		"processor": {
			"updateTimeout": 5000,
			"queryTimeout": 5000,
			"maxConcurrentRequests": 5
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
				"unsubscribe": "/unsubscribe",
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
```
The `ports`, `paths` and `secure` members within the `gates` object are used to specify the URLs at which the broker is listening for requests. The above default configuration initializes the broker has shown here:

```
SPARQL 1.1 Query     | http://<your local address>:8000/query
SPARQL 1.1 Update    | http://<your local address>:8000/update
SPARQL 1.1 Subscribe | ws://<your local address>:9000/subscribe
```

Timeouts on update and query processing on the SPARQL endpoint are specified respectively by the `updateTimeout` and `queryTimeout` members. It also possible to setup the maximum number of concurrent requests that can be processed by the endpoint (see `maxConcurrentRequests`). The `timeout` member of the `spu` allows to specify the timeout subscription processing. Eventually, the `queueSize`is the maximum number of pending requests after which the broker starts to deny new requests.

## Security issues
The broker security can be enable by setting to `true` the `secure` member of the `engine.jpar` file. The broker uses a JKS for storing the keys and certificates for [SSL](http://docs.oracle.com/cd/E19509-01/820-3503/6nf1il6ek/index.html) and [JWT](https://tools.ietf.org/html/rfc7519) signing/verification. A default `sepa.jks` is provided including a single X.509 certificate (the password for both the store and the key is: `sepa2017`). If you face problems using the provided JKS, please create a new one as follows: `keytool -genkey -keyalg RSA -alias sepakey -keystore sepa.jks -storepass sepa2017 -validity 360 -keysize 2048`
The SEPA broker allows to use a user generated JKS. Run `java -jar engine-x.y.z.jar -help` for a list of options. The Java [Keytool](https://docs.oracle.com/javase/6/docs/technotes/tools/solaris/keytool.html) can be used to create, access and modify a JKS. 

## JMX monitoring
The SEPA broker is also distributed with a default [JMX](http://www.oracle.com/technetwork/articles/java/javamanagement-140525.html) configuration `jmx.properties` (including the `jmxremote.password` and `jmxremote.access` files for password and user grants). Remember to change password file permissions using: `chmod 600 jmxremote.password`. To enable remote JMX, the broker must be run as follows: `java -Dcom.sun.management.config.file=jmx.properties -jar engine-x.y.z.jar`. Using [`jconsole`](http://docs.oracle.com/javase/7/docs/technotes/guides/management/jconsole.html) is possible to monitor and control the most important broker parameters. By default, the port is `5555` and the `root:root` credentials grant full control (read/write).

## Experiment with the Java Dashboard
In the `Tools` folder, you can find an application (`SEPADashboard_X_Y_Z.jar`) that allows you to interact and experiment the functionalities offered by SEPA. Just double click on the jar or run it from a command shell as: `java -jar Dashboard_X_Y_Z.jar` 

SEPA applications are built around a [JSON Semantic Application Profile (JSAP)](http://mml.arces.unibo.it/TR/jsap.html). Examples of JSAP files can be found in the `jsap` folder. You should start by loading the `sepatest.jsap` file into the Dashboard...enjoy!


