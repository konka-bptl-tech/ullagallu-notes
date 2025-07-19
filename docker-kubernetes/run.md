# Cloud With VarJosh
### k8s
docker is excellent tool for containrization applications
It is great for local development
There certain challenges with docker and docker does not offer
- auto scaling
- load balancing
- self healing
- Rollingupdates & Rollbacks
- single point of failure[dockr is a single host]

To address such challenges we were going for k8s
- By default k8s have distributed architecture

- HPA ensures automatically add or remove more workloads based on traffic
- K8s services provides load balacing to the workloads
- when pod goes down any reason k8s spin up another pod
- Deployment provide 0 downtime to during new release easy rollback when something goes wrong

k8s is a opensource container orchestration platform that automates complete management of workloads
Management = autoscaling,self healing, config & secrets,volumes,rollouts and rollbacks,HA


Introduction
------------





