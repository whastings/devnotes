# Load Balancer Notes

* Server that passes requests to other servers
* Uses a scheme to route requests
  * e.g. round robin
* Can be used to terminate domain's SSL encryption
  * Then pass unencrypted request to app server
* Can run health checks on hosts
  * And stop routing to hosts that aren't passing
* Examples
  * Amazon ALB and ELB
  * HAProxy
