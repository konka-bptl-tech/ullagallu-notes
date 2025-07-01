# How Karpenter works
1. Monitor Pending Pods:
Watches the Kubernetes Scheduler for Unschedulable pods

2. Evaluates Pod Requirements:
Consider CPU, Memory, Architecture , Zones and other Constriaints

3. Selects Optimal Instance Type:
Choose best-fitting and cost-effective EC2 instance

4. Launches EC2 instance directly:
USes the AWS APIs to quickly spin up nodes without relying on the node groups

5. Register Nodes Automatically:
New nodes automatically joined the cluster

6. Scales Down Unsed Nodes
Continuously monitors usage and deprovisions idle nodes to save costs


pending pods         --->       Existing Capacicty
                                                   -->Karpenter       Optimized Capacity
                                                   -->Karpenter       Optimized Capacity
Unschedualable Pods  --->    Just in time capacity  


Exercise:
---------
1. Create EKS Cluster
2. Install Karpenter
3. EC2 Node Class
4. NodePool
5. Sample Workload


```yaml
apiVersion: karpenter.sh/v1beta1
kind: Provisioner
metadata:
  name: general
spec:
  requirements:
    - key: "node.kubernetes.io/instance-type"
      operator: In
      values: ["t3.large", "m5.large", "c6a.large"]
  limits:
    resources:
      cpu: 1000
  provider:
    subnetSelector:
      karpenter.sh/discovery: my-cluster
    securityGroupSelector:
      karpenter.sh/discovery: my-cluster
  ttlSecondsAfterEmpty: 30
```

---

## ❌ Problems with Cluster Autoscaler

1. **Slow Reaction Time**
   Scans nodes and pods every 10 seconds or more — **not event-driven**, so scale-up is slower.

2. **ASG Dependency**
   Only works with **pre-defined Auto Scaling Groups (ASGs)** — you must manually create/manage instance types and sizes.

3. **Limited Instance Flexibility**
   Can only scale within **specific instance types** defined in the ASG — no dynamic selection of newer, cheaper instances.

4. **Harder Multi-AZ Support**
   Requires you to manage multiple node groups across Availability Zones — adds complexity.

5. **Complex Configuration**
   Scaling decisions are based on what’s defined in the ASGs — you need to guess instance types ahead of time.

6. **Inefficient Scale-Down**
   Doesn’t consolidate underutilized nodes smartly — idle resources may stick around longer than needed.

---

## ✅ How Karpenter Addresses These Problems

1. **Fast, Event-Driven Scaling**
   Reacts **instantly** to unschedulable pods — no polling loops, no delays.

2. **No ASG Required**
   Launches **EC2 instances directly** via AWS APIs — removes the need to manage ASGs or node groups.

3. **Flexible, Smart Instance Selection**
   Dynamically chooses the **best instance type** based on cost, availability, and pod requirements (e.g., CPU, memory, zone).

4. **Built-in Multi-AZ Support**
   Automatically spreads nodes across zones based on pod topology and availability.

5. **Simplified Provisioning**
   Just define general constraints (like instance families, zones, or taints) — Karpenter handles the rest.

6. **Smart Deprovisioning**
   Continuously monitors nodes and **removes unused or underutilized ones** — saves costs automatically.

---

## 🔁 How Karpenter Works

1. **Monitor Pending Pods**
   Watches the Kubernetes scheduler for unschedulable pods in real time.

2. **Evaluates Pod Requirements**
   Considers CPU, memory, architecture (x86/ARM), availability zones, and other constraints like taints and labels.

3. **Selects Optimal Instance Type**
   Chooses the most cost-effective and best-fitting EC2 instance — even Spot instances if allowed.

4. **Launches EC2 Instance Directly**
   Uses AWS APIs to spin up new EC2 nodes without relying on node groups or ASGs.

5. **Registers Nodes Automatically**
   New nodes automatically join the cluster and become ready for pod scheduling.

6. **Scales Down Unused Nodes**
   Monitors node usage and **terminates idle or underutilized nodes** to reduce cost.

---


# Node Group / CLuster Auto Scaler / Karpenter
A Node Group is the set of EC2 instances in your cluster, and Cluster Autoscaler is the tool that watches your cluster and decides when to add or remove nodes from that group.
They work together — but they are not the same.


---------------------aws $300 credentials
1. Buy Domain & Email in Hostinger
2. Create New AWS account with above Email
3. aws.amazon.com/free/offers
4. Click on AWS Rapid Ramp Credits
5. Fill the Form
   FirstName        LastName
   AWS AccountID
   Business Email Address
   Job Role: IT professional / Technical Manager
   Project Description
   Project Intent: Production Workload
   Implementation Time Frame: 1-3 months
   Budget-Range: $500-%900/month
   Workload Type: Website & Webapps
   Company Name:
   Company Size: 
   Phone Number:
   Industry:
   Country/Region:
   Postal Code:
   Business Interests:
