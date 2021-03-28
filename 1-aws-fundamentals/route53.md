# Route 53

Route 53 is a managed DNS (Domain Name System)

DNS is a collection of rules and records which helps clients understand how to reach a server through URLs.

In AWS, the most common records are (will be on exam):
* A: URL to IPv4
* AAAA: URL to IPv6
* CNAME: URL to URL (hostname to another hostname)
* ALIAS: URL to AWS resource

Route 53 can use:
* Public domain names you own
* Private domain names that can be resolved by your instances in your VPCs

Route53 has advanced features such as:
* Load balancing (through DNS - also called client load balancing)
* Health checks (although limited…)
* Routing policy: simple, failover, geolocation, geoproximity, latency, weighted, multi value

    * You pay $0.50 per month per hosted zone
    * Prefer Alias over CNAME for AWS resources (for performance reasons)

## Route 53 Hands on

## Route 53 TTL

* The time a browser will cache (DNS cache) an IP address returned from Route53 or DNS in general.
* The Highest TTL - 24hr
* The Lowest TTL - 60s
* TTL is mandatory for each DNS record

## CNAME vs Alias
* AWS resources (Loadbalancer, CloudFront) expose and AWS hostname. lb1-1234.us-west-2.elb.amazonaws.com and you want myapp.domain.com
    * CNAME: hostname to hostname. ONLY FOR NON ROOT DOMAIN (ex: something.mydomain.com)
    * ALIAS: Points a hostname to an AWS Resource. Works for ROOT DOMAIN and NON ROOT DOMAIN (aka mydomain.com)  
        * Free of Charge
        * Native health check
## commands
dig hostname

## Routing Policy

## Simple Routing Policy
* Use when you need to redirect to a single resource
* You cant attache health check to sinple routing policy
* if multiple values are returned, a random one is chose by the client
* client side loadbalancing possible by entering multiple IPs to a type A address.

Weighted Routing Policy
* Control the % of the requests that go to specific endpoint.
* Helpful to test 1% of traffic on new app version for examle
* Helpful to split traffic between towo regions
* can be associated with health checks.

Latency Routing Policy
* Redirect to the server that has the least latency close to use
* SUper helfpul when the latency of users is a priority
* latency is evaluated in terms of user to designated AWS regions
* Germany may be directed to the us.
* we need to create multiple record sets

Route 53 Health checks
* Have X health checks failed -> unhealthy default 3
* AFter X health checks passed -> health default 3
* Default health check interval - 30s
* About 15 health checkers will check the endpoint health
* one request every 2 seconds on average
* can have HTTP, TCP and HTTPS health checks (no SSL verfication)
* Health checks can be linked to Route53 DNS queries!

Failover Routing policy
* Primary and secondary (disaster recovery)
* Health check (mandatory)

GeoLocation Routing Policy
* Based on user location
* different from latency based
* traffic from the uk should go to this specific ip
* should create a default policy (in cases theres no match on location)

Multi Value Policy
* use when reouting traffic to multiple resources
* want to associate a route53 helaht checks with records
* up to 8 healthy records are returnes for each multi value query
* multi value is not a substitute for having and elb
