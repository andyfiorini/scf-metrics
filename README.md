# Simplified CF-Metrics
Simplified cf-metrics, is a mod from the original cf-metrics from MonsantoCo(https://github.com/MonsantoCo/cf-metrics).

It is a more simpler setup, with firehose-nozzle feeding influxdb.  For viewing and alerting, I used grafana.

It works, it is simpler to setup than the original.

# Setup
=====

You need a working cf deployment and a host with docker and docker-compose.

1.- Modify the docker-compose.yml file with the necesary vars.

Here is a brief list of variables to modify:
  environment:
  - PRE_CREATE_DB=cf_prd;cf_np;bosh_prd;bosh_np 
  
  
  
  
  
  
