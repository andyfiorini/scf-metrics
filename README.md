# Simplified CF-Metrics
Simplified cf-metrics, is a mod from the original cf-metrics from MonsantoCo(https://github.com/MonsantoCo/cf-metrics).

It is a more simpler setup, with firehose-nozzle feeding influxdb.  For viewing and alerting, I used grafana.

It works, it is simpler to setup than the original.

# Setup

You need a working cf deployment and a host with docker and docker-compose.

1.- Modify the docker-compose.yml file with the necesary vars.

Here is a brief list of variables to modify:
```
  environment:
    PRE_CREATE_DB=cf_prd;cf_np;bosh_prd;bosh_np 
```
  
2.- Modify enviroment Variables for nozzle:

```
  NOZZLE_UAAURL=https://uaa.cf-np.company.com
  NOZZLE_CLIENT=client_id
  NOZZLE_CLIENT_SECRET=secret
  NOZZLE_TRAFFICCONTROLLERURL=wss://doppler.cf-np.company.com:443
  NOZZLE_DEPLOYMENT=cf_aws_np
```

Set the appropiate values.
  
3.-  Firehose Nozzle Configuration

For each CloudFoundry deployment that you'll be monitoring, you'll need a nozzle defined in the docker-compose.yml file and a corresponding nozzle configuration file. Update the parameters in the nozzle json as explained in the upstream project  

The config file for the nozzle has a name like this:

./firehose-nozzle/influxdb-firehose-nozzle-*.json

4.- Grafana Configuration

Update the following section to reflect the fqdn of your docker host:


./grafana/grafana.ini
```
 # The public facing domain name used to access grafana from a browser
 domain = server.company.com
```
5.-  Cloud Foundry Setup

Update the following section in your cloud foundry manifest and redeploy cf to enable a uaa client for the firehose nozzle:
```
properties:
  uaa:
    clients:
      influxdb-firehose-nozzle:
        access-token-validity: 1209600
        authorized-grant-types: authorization_code,client_credentials,refresh_token
        override: true
        secret: <password>
        scope: openid,oauth.approvals,doppler.firehose
        authorities: oauth.login,doppler.firehose
```
6.-  Run docker-compose
```
$  docker-compose up -d
```
NOTE:  You can configure the slack notifications, or email notifications in Grafana directly.
