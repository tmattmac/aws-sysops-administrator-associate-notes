# EC2 High Availability and Scalability

## Elastic Load Balancer Overview 

* ELB can be private (within AWS) or public 
* Don't scale instantaneously, may need to contact support to "pre-warm" balancer for large spikes in traffic
  * Only supported for Classic and Application Load Balancer
* Should configure instance security group to only allow traffic from load balancer
* Enable cross-zone load balancing to ensure equal balance between all instances across AZs

## Load Balancer Types

* Classic (v1)
  * Supports HTTP/HTTPS and TCP
  * Health checks are HTTP- or TCP-based
  * Fixed hostname
  * Instances allocated directly to load balancer
* Application (v2)
  * Supports HTTP/HTTPS, HTTP/2, and WebSockets
  * Fixed hostname
  * Target Groups
    * EC2 instances (ASG)
    * ECS tasks
    * Lambda functions
    * Private IP addresses
  * Routing based on path, hostname, or query string
  * Health checks at target group level
  * Great for microservices and containerized applications
  * Can redirect to dynamic port in ECS
  * Can see original client IP, port, and protocol from instance with headers
    * `X-Forwarded-For`, `X-Forwarded-Port`, and `X-Forwarded-Proto` headers
* Network (v2)
  * Supports TCP, TLS, and UDP
  * Low latency, supports millions of requests per second
  * One static IP per AZ, can assign elastic IP as well
  * Security group configuration at instance level, not load balancer
    * External traffic is seen directly by instance, no headers required

## Sticky Sessions

* Supported by Classic and Application Load Balancers
* Uses cookie with expiration time to associate a user with a specific instance
* May cause single instance to be overloaded

## SSL and TLS

* SSL certificate allows in-flight encryption
  * SSL - Secure Sockets Layer
  * TLS - Transport Layer Security (newer)
* Public SSL certificates are issued by Certificate Authorities 
* Have an expiration date and must be renewed
* Manage SSL certificates with ACM (AWS Certificate Manager)
* SSL connection is terminated at the load balancer, which connects to the instance over regular HTTP via private VPC
* Server Name Indication (SNI) lets you load multiple certificates onto a single server
  * Lets you to serve multiple sites from a single server
  * Requires client to specify hostname of target server
  * Server uses correct certificate based on specified hostname
  * Not supported by Classic Load Balancer
* Need to change ELB security policy to support older browsers

## Connection Draining

* Refers to a period of time to complete current in-flight requests while the instance is being de-registered, but not allowing new requests
* Named as such in Classic Load Balancers
* For Target Groups, named Deregistration Delay
* Default 300 seconds

## Response Codes

* 2xx - successful request
* 3xx - redirect (no error)
* 4xx - client error (bad request or unauthorized)
* 5xx - server error (ELB, application, or gateway issue)

## Monitoring

* ELB access logs - log all access requests to S3
  * Automatically encrypted in S3
  * Data Logged:
    * Time
    * Client IP address
    * Latency
    * Request path
    * Server response
    * Trace ID
* CloudWatch Metrics - aggregate statistics sent from ELB by default
  * Metrics Supported:
    * BackendConnectionErrors
    * HealthyHostCount & UnhealthyHostCount
    * HTTPCode_Backend_2XX, HTTPCode_Backend_3XX
    * HTTPCode_ELB_4XX, HTTPCode_ELB_5XX
    * Latency
    * RequestCount
    * SurgeQueueLength - number of pending requests
    * SpilloverCount - number of rejected requests due to being over capacity
* Request Tracing - use header to trace a single request
  * `X-Amzn-Trace-Id` header
  * Not integrated with X-Ray (yet)

## Auto Scaling Groups

* Lets you add or remove EC2 instances to match load
* Ensure minimum or maximum number of instances, as well as desired number
* Can automatically terminate and replace unhealthy instances
* Uses launch configuration or launch template to define instance settings
* Scaling Policies
  * CloudWatch Alarm - scale when alarm is triggered
  * Target Tracking - define target value for a metric and scale to meet target
  * Custom Metric - scale based on custom metric (PutMetric API)
* IAM role attached to ASG will be assigned to EC2 instances
* Can use instance status check or ELB health check
* Processes
  * Can be suspended
  * Important processes:
    * Launch
    * Terminate
    * ReplaceUnhealthy
    * HealthCheck
    * AZRebalance - launch new instance in different AZ and terminate old instance
* CloudWatch Metrics (opt-in)
  * Collected every minute
  * Can also monitor underlying EC2 instances
  * Important metrics:
    * GroupMinSize
    * GroupMaxSize
    * GroupDesiredCapacity
    * GroupInServiceInstances
    * GroupPendingInstances
    * GroupStandbyInstances
    * GroupTerminatingInstances
    * GroupTotalInstances