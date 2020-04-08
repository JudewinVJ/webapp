# webapp
A simple WebApp API that connects to Postgresql HA Database

### Helm chart can be installed using the following command: 
```helm install ./webapp --name webapp```
This installs a simple webapp that connects to a highly available Postgresql database. 

## Architecture
![Image description](SimpleArchitecture (1).png)	

## How does it work ?
  Once the chart is installed, you will be able to access the service via the configured ingress in the webservice.
  You could access POST API 
