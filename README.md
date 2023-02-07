# Monitor your Airzone system through the API

This docker-compose TIG stack (telegraf, influxdb, grafana) deploys what it takes to easily monitor your Airzone system through the webserver API.

 # Prerequisites :
- docker & docker-compose
- Airzone system with the webserver

# Using

 1. Pull the repo
 2. Update the `set-retention.iql` file to set your retention limit (default : 60 weeks)
 3. Update the `docker-compose.yml` file to set the password for your database and grafana admin user.
 4. Follow the template below to set up `telegraf.conf` and add this template as many times as you have zones:
```
    [[inputs.http]]
    urls = ["http://WEB_SERVER_IP:3000/api/v1/hvac"]
    data_format = "json"
    name_override = "Airzone-XXX"
    method = "POST"
    json_query = "data"
    headers = {"cache-control" =  "no-cache","content-type" =  "application/json"}
    body = '{"systemID": X, "zoneID": Y}'
```
Replace:
- **WEB_SERVER_IP** by the IP of your Airzone webserver
- **Airzone-XXX** ; update XXX by the name of the room (*Airzone-* is mandatory)
- **"systemID": X, "zoneID": Y** ; Update X and Y by the configuration of the room. (values can be found on the thermostats of your zones)

5. Start your docker-compose stack: `sudo docker-compose up -d`
5. Login to grafana (http://SERVER_IP:3000) and import the dashboard `Airzone-dashboard-grafana.json`
6. done :)

# Doc
API Airzone : https://community.jeedom.com/uploads/short-url/VLbPoMjHEQ4DyYuyeYdYeIl85S.pdf
