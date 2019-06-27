# Cassandra ELK monitoring

1. Identifying the metrics
2. Configure Jolokia JVM Agent In Cassandra
3. Configuring metric-beat and enable http beat module
4. Configuring metric-beat.yml and http.yml files
5. Analysing json in Kibana dashboard
4. Configuring scripted fields in Kibana
5. Configure visualization and dashboards for monitoring

## 1. Identifying metrics

Identify correct and important metrics to monitor.

- SSTable count
- Space used (live)
- SSTable Compression Ratio
- Number of partitions (estimate)
- Local read latency
- Local write latency
- Bloom filter false ratio
- Maximum tombstones per slice (last five minutes)-TombstoneScannedHistogram

## 2.Configure Jolokia JVM Agent In Cassandra

The steps to configure Jolokia JVM Agent in Cassandra are as below:

1. Download the latest Jolokia JVM agent jar file (e.g. jolokia-jvm-1.6.0-agent.jar) from https://jolokia.org/download.html.

2. Copy the downloaded jar file to Cassandra’s lib folder (e.g. /usr/share/cassandra/lib)

3. In cassandra-env.sh file, enable/add the following lines:


`JVM_OPTS="$JVM_OPTS -javaagent:$CASSANDRA_HOME/lib/jolokia-jvm-1.6.0-agent.jar"`

or 

`echo "export JVM_EXTRA_OPTS=\"-javaagent:/usr/share/cassandra/lib/jolokia-jvm-1.6.0-agent.jar=port=8778,host=localhost\"" | sudo tee -a /etc/default/cassandra`

4.Restart Cassandra service.

sudo service cassandra restart

source:https://blog.pythian.com/two-easy-ways-poll-apache-cassandra-metrics-using-jmx-http-bridge/

## 3.Configuring metric-beat and enable http beat module:

- curl -L -O https://artifacts.elastic.co/downloads/beats/metricbeat/metricbeat-7.0.0-amd64.deb
- sudo dpkg -i metricbeat-7.0.0-amd64.deb
- sudo metricbeat setup --template
- sudo metricbeat modules enable http
- sudo metricbeat setup
- sudo service metricbeat start

- sudo nano /etc/metricbeat/metricbeat.yml
- sudo nano /etc/metricbeat/modules.d/http.yml

#### Note: Any changes in above files reuquires metric-beat to be restarted.

## 4.Configuring metric-beat.yml and http.yml files
- Adding kibana output address and other configuration in metricbeat file.
- Adding cassandra jmx values in http file

## 5.Analysing json in Kibana dashboard
- Analyse json structure and modify accordingly by adding scripted fields.

## 6.Configuring scripted fields in Kibana
- link to create scripted field in kibana https://www.elastic.co/guide/en/kibana/current/scripted-fields.html
- scripted fields are recorded in Scripted Kibana fields file where metricname,tablename,metricvalue and all metrics with their values are recorded.




