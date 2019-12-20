FROM java:8

WORKDIR /

COPY . /

# As default the service connects to the host address where the container is running
#ENV SEPA_HOST "host.docker.internal"

# Default compiled on Ubuntu 18.04 LTS (Use ./CRITERIA1DMacOsX on Mac OS X)
#ENV CMD "./CRITERIA1DMacOsX"

# Uncomment to set a specific day. Otherwise, the service considers the current day
#ENV SET_DATE 2019-08-03			

# Uncomment to set a custom weather DB (input)
#ENV INPUT_DB "myWeatherDB.db"

# Uncomment to set a custom irrigation DB (output)
#ENV OUTPUT_DB "myIrrigationDB.db"

# Uncomment to set the number of days used for forecasts (default is 3)
#ENV FORECAST_DAYS 3

CMD java -jar sepa.apps.swamp.criteria-0.0.1-SNAPSHOT.jar



