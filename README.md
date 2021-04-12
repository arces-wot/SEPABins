The repository includes all you need to test SEPA, a publish-subscribe architecture designed to support information level interoperability by means of Semantic Web technologies. The architecture is built on top of a generic SPARQL endpoint where publishers and subscribers use standard **SPARQL** [Updates](https://www.w3.org/TR/sparql11-update/) and [Queries](https://www.w3.org/TR/sparql11-query/). Notifications about events (i.e., changes in the [**RDF**](https://www.w3.org/RDF/) knowledge base) are expressed in terms of [added and removed SPARQL binding results](http://mml.arces.unibo.it/TR/sparql11-subscribe.html) since the previous notification. 

Developers can benefit of a set of API implementing a [Producer-Aggregator-Consumer design pattern](http://mml.arces.unibo.it/TR/jsap.html).

For more details on the current implementation and how you can contribute please follow this [link](https://github.com/arces-wot/SEPA).

# Installation
Clone the repository: `git clone https://github.com/arces-wot/SEPABins.git`

# Usage
The SEPA broker needs to connect to a SPARQL endpoint supporting the [SPARQL 1.1 Protocol](https://www.w3.org/TR/sparql11-protocol/). For you convenience, the repository includes an instance of [Blazegraph](https://www.blazegraph.com/).

## Start the SPARQL endpoint
From a command shell move to the `Endpoint` folder and run the endpoint: `java -jar blazegraph.jar`

If Blazegraph started correctly, you would see something like the following:

```
Welcome to the Blazegraph(tm) Database.

Go to http://192.168.1.11:9999/blazegraph/ to get started.
```
Otherwise, Blazegraph failed to start. This can be due to the JRE version: If you are using Java 9, try forcing the use of Java 8 JRE: `/Library/Java/JavaVirtualMachines/jdk1.8.0_65.jdk/Contents/Home/bin/java -jar blazegraph.jar`

In any case, please refer to the [Blazegraph web site](https://www.blazegraph.com/) to download and run the last version (the one we provide is just for the purpose of quickly testing SEPA and may not be updated).

## Start the SEPA broker
From a command shell, move to the `Engine` folder and type: `java -Dlog4j.configurationFile=./log4j2.xml -jar engine-0-SNAPSHOT.jar`

The following welcome message would be displayed:

```
##########################################################################################
#                           ____  _____ ____   _                                         #
#                          / ___|| ____|  _ \ / \                                        #
#                          \___ \|  _| | |_) / _ \                                       #
#                           ___) | |___|  __/ ___ \                                      #
#                          |____/|_____|_| /_/   \_\                                     #
#                                                                                        #
#                     SPARQL Event Processing Architecture                               #
#                                                                                        #
#                                                                                        #
# This program comes with ABSOLUTELY NO WARRANTY                                         #
# This is free software, and you are welcome to redistribute it under certain conditions #
# GNU GENERAL PUBLIC LICENSE, Version 3, 29 June 2007                                    #
#                                                                                        #
#                                                                                        #
# @prefix git: <https://github.com/> .                                                   #
# @prefix dc: <http://purl.org/dc/elements/1.1/> .                                       #
#                                                                                        #
# git:arces-wot/sepa dc:title 'SEPA' ;                                                   #
# dc:creator git:lroffia ;                                                               #
# dc:contributor git:relu91 ;                                                            #
# dc:format <https://java.com> ;                                                         #
# dc:publisher <https://github.com> .                                                    #
##########################################################################################

SPARQL 1.1 endpoint
----------------------
SPARQL 1.1 Query     | http://localhost:9999/blazegraph/namespace/kb/sparql (Method: POST)
SPARQL 1.1 Update    | http://localhost:9999/blazegraph/namespace/kb/sparql (Method: POST)
----------------------

SPARQL 1.1 Protocol (https://www.w3.org/TR/sparql11-protocol/)
----------------------
SPARQL 1.1 Query     | http://192.168.1.3:8000/query
SPARQL 1.1 Update    | http://192.168.1.3:8000/update
----------------------

SPARQL 1.1 SE Protocol (http://mml.arces.unibo.it/TR/sparql11-se-protocol.html)
----------------------
SPARQL 1.1 Subscribe | ws://192.168.1.3:9000/subscribe
----------------------

*****************************************************************************************
*                      SEPA Broker Ver 1.0.0 is up and running                          *
*                                Let Things Talk!                                       *
*****************************************************************************************
```

To close the broker just type `CTRL+X`:

```
Stopping HTTP gate...
Stopping WebSocket gate...
Stopping Processor...
Stopped...bye bye :-)
```
# Play with SEPA
SEPA applications are built around the [JSON Semantic Application Profile (JSAP)](http://mml.arces.unibo.it/TR/jsap.html).  A demo application can be found in the `Chat` folder. The JSAP file is named `chat.jsap`. The full chat demo application can be found [here](https://github.com/arces-wot/SEPA-Chat). From a command shell, move to the `Chat` folder and type: `java -jar Chat.jar`

The output should look like:

```
2021-04-12T18:58:36,779 [INFO ] main (DeleteAll.java:23) Delete all
2021-04-12T18:58:36,903 [INFO ] main (SEPAChatTest.java:88) Register client: ChatBot0
2021-04-12T18:58:36,955 [INFO ] main (SEPAChatTest.java:88) Register client: ChatBot1
2021-04-12T18:58:37,272 [INFO ] Thread-0 (ChatMonitor.java:98) *******************************************************
2021-04-12T18:58:37,272 [INFO ] Thread-0 (ChatMonitor.java:99) user|messages|received|sent|removed|brokenRec|brokenRem
2021-04-12T18:58:37,272 [INFO ] Thread-0 (ChatMonitor.java:100) *******************************************************
2021-04-12T18:58:37,273 [INFO ] Thread-0 (ChatMonitor.java:104) http://wot.arces.unibo.it/chat/user/ChatBot0|5|0|0|0|false|false
2021-04-12T18:58:37,273 [INFO ] Thread-0 (ChatMonitor.java:104) http://wot.arces.unibo.it/chat/user/ChatBot1|5|0|0|0|false|false
2021-04-12T18:58:37,308 [INFO ] main (ChatClient.java:34) http://wot.arces.unibo.it/chat/user/ChatBot0 joining the chat...
2021-04-12T18:58:37,366 [INFO ] main (ChatClient.java:49) http://wot.arces.unibo.it/chat/user/ChatBot0 chat joined!
2021-04-12T18:58:37,393 [INFO ] main (ChatClient.java:34) http://wot.arces.unibo.it/chat/user/ChatBot1 joining the chat...
2021-04-12T18:58:37,441 [INFO ] main (ChatClient.java:49) http://wot.arces.unibo.it/chat/user/ChatBot1 chat joined!
2021-04-12T18:58:38,275 [INFO ] Thread-0 (ChatMonitor.java:98) *******************************************************
2021-04-12T18:58:38,276 [INFO ] Thread-0 (ChatMonitor.java:99) user|messages|received|sent|removed|brokenRec|brokenRem
2021-04-12T18:58:38,276 [INFO ] Thread-0 (ChatMonitor.java:100) *******************************************************
2021-04-12T18:58:38,276 [INFO ] Thread-0 (ChatMonitor.java:104) http://wot.arces.unibo.it/chat/user/ChatBot0|5|3|2|1|false|false
2021-04-12T18:58:38,277 [INFO ] Thread-0 (ChatMonitor.java:104) http://wot.arces.unibo.it/chat/user/ChatBot1|5|2|2|0|false|false
2021-04-12T18:58:39,279 [INFO ] Thread-0 (ChatMonitor.java:98) *******************************************************
2021-04-12T18:58:39,280 [INFO ] Thread-0 (ChatMonitor.java:99) user|messages|received|sent|removed|brokenRec|brokenRem
2021-04-12T18:58:39,280 [INFO ] Thread-0 (ChatMonitor.java:100) *******************************************************
2021-04-12T18:58:39,280 [INFO ] Thread-0 (ChatMonitor.java:104) http://wot.arces.unibo.it/chat/user/ChatBot0|5|5|4|3|false|false
2021-04-12T18:58:39,281 [INFO ] Thread-0 (ChatMonitor.java:104) http://wot.arces.unibo.it/chat/user/ChatBot1|5|5|4|2|false|false
2021-04-12T18:58:40,286 [INFO ] Thread-0 (ChatMonitor.java:98) *******************************************************
2021-04-12T18:58:40,286 [INFO ] Thread-0 (ChatMonitor.java:99) user|messages|received|sent|removed|brokenRec|brokenRem
2021-04-12T18:58:40,286 [INFO ] Thread-0 (ChatMonitor.java:100) *******************************************************
2021-04-12T18:58:40,287 [INFO ] Thread-0 (ChatMonitor.java:104) http://wot.arces.unibo.it/chat/user/ChatBot0|5|5|5|5|false|false
2021-04-12T18:58:40,287 [INFO ] Thread-0 (ChatMonitor.java:104) http://wot.arces.unibo.it/chat/user/ChatBot1|5|5|5|5|false|false

```

The demo application is configured to simulate two chat bots, exchanging 5 messages each other. The messagges are sent, received and the removed.


SEPA comes with a dashboard that provides all the functionalities needed to interact with a SEPA broker (using JSAP files). In the `Dashboard` folder run it from a command shell as: `java -jar SepaDashboard-0-SNAPSHOT-jar-with-dependencies.jar` 

Now, let's use the dashboard to inspect what is produced by the chat demo:

1. Click on the `Load JSAP` button. Move to the `Chat` and locate the `chat.jsap`. Open it.
2. Query for registered users: select `USERS` on the right list box and click `QUERY`

![](./Query.png)

What you have done can be done with a common SPARQL endpoint. But SEPA can do more...press `SUBSCRIBE`. A new tab is opened with the notifications on changing in the registered users. Grey lines indicate "old" values, while white lines indicate "new" values.

![](./Subscribe.png)

Run again the chat demo and see the notifications on the dashboard...that's `Dynamic Linked Data`! 
