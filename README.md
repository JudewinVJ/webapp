# webapp
A simple WebApp API that connects to Postgresql HA Database

### Helm chart can be installed using the following command: 
```helm install ./webapp --name webapp```
This installs a simple webapp that connects to a highly available Postgresql database. 
This is a URL shortner webapp that helps redirect to other url's which are longer, using a code for each.

## Architecture
![Image description](SimpleArchitecture.png)	

## How does it work ?
  Once the chart is installed, you will be able to access the service via the configured ingress in the webservice.
  1. Now lets hit a url which is not present in the DB - curl -v http://(svc-url)/pingpong! --> Returns 404
  2. ets add a url to the DB - curl -X POST http://(svc-url)/ -d "full_url=https://redhat.com" --> Returns EZpNfRi
  3. Now lets hit the url added to the DB
  curl -X GET http://(svc-url)/EZpNfRi -v
*   Trying 192.168.99.100...
* Connected to 192.168.99.100 (192.168.99.100) port 31317 (#0)
> GET /EZpNfRi HTTP/1.1
> Host: 192.168.99.100:31317
> User-Agent: curl/7.43.0
> Accept: */*
>
< HTTP/1.1 307 Temporary Redirect
< Location: https://redhat.com
< Content-Length: 0
< Content-Type: text/plain; charset=utf-8

## How to ensure backend is highly available ?
   PGPool is an internal loadbalancer for our frontend to connect with the database. 
   ### How to ensure PGpool's availability ?
   Create multiple replicas of PGpool and ensure using PDB that atleast 1 instance is available at any point in time.
   ### How to ensure automatic fail over ?
   If the primary pod fails, the pgpool detects that using health check and fails it over to the next in order.
   Even if the node in which the primary pod fails, within seconds it redirects the connections over to secondary pod.
   ### What happens when we update DB version ? 
   In that case, pgpool kicks in and does an automatic fail over to the other available pod.
   ### Scope to Improve
    1. Set Affinity to different AZ for each pod or anti-affinity within the same AZ to schedule on different nodes.
    2. Also Postgresql allows various slaves from various regions to join the replication (for DR purposes).
### Source Project References 
#### Postgresql - Helm Chart - https://github.com/bitnami/charts/tree/master/bitnami/postgresql-ha
#### WebAPP - https://github.com/xcoulon/go-url-shortener
