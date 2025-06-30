# ELB[Elastic-Load-Balancing]
LB:
- Distributes incoming traffic to backend targets such as ec2 instances, containers & lambda functions or ALB
- It will do health checks only routes traffic to healthy targets
- ELB scales your loadbalancer when incoming traffic grows 

Is a Elastic Load Balancing services it handles It provides managed load balancers
- Upgrades
- Patches
- Maintainance
- High Avalability
- Scalability

It provides 4 types of LoadBalancers

1. Classic LoadBalancer[L4&L7][Deperecated]
2. Application LoadBalancer[L7][New-Gen]
3. Network LoadBalancer[L4][New-Gen]
4. GateWay LoadBalancer[L3][Packet Filtering]


ALB & Components
----------------
- ALB is a L7 loadbalancer works on HTTP/HTTPS protocols
- A load balancer serves as the single point of contact for clients. It provides High avaialbity for you applications
- It has intelligence to route traffic based on path based/host based
- The load balancer distributes incoming application traffic across multiple targets, such as EC2 instances, in multiple Availability Zones.
- This increases the availability of your application. You add one or more listeners to your load balancer.



**Listners**
A listener checks for connection requests from clients, using the protocol and port that you configure.The rules that you define for a listener determine how the load balancer routes requests to its registered targets. Each rule consists of a priority, one or more actions, and one or more conditions. When the conditions for a rule are met, then its actions are performed. You must define a default rule for each listener, and you can optionally define additional rules.


**Target Groups**
Each target group routes requests to one or more registered targets, such as EC2 instances, using the protocol and port number that you specify. You can register a target with multiple target groups. You can configure health checks on a per target group basis. Health checks are performed on all targets registered to a target group that is specified in a listener rule for your load balancer.



