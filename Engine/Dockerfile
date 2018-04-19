FROM dockerbase/service-java8

RUN mkdir /sepa
WORKDIR /sepa

ADD sepa.jks /sepa
ADD log4j2.xml /sepa
ADD engine.jpar /sepa
ADD defaults.jpar /sepa
ADD jmx.properties /sepa
ADD jmxremote.access /sepa
ADD jmxremote.password /sepa
ADD SEPAEngine_0.8.4.jar /sepa
ADD endpoint-blazegraph-docker.jpar /sepa/endpoint.jpar
CMD java -jar SEPAEngine_0.8.4.jar