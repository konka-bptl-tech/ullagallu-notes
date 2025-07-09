# 🧑‍💼 Day 3 – Reporting a Cluster Upgrade to Manager

## 🎯 Scenario
You have successfully upgraded the EKS cluster and need to report it to your manager, mentioning what was done, how it was tested, and how you ensured zero downtime.

---

## 🗣️ Sample Message

Hi, the EKS cluster upgrade from version 1.30 to 1.31 is complete.  
I upgraded the control plane first, followed by the node groups with automated drain and rollout.  
There was no downtime — all workloads were tested after the upgrade.  
Monitoring dashboards and logs were checked to confirm stability.

---

## ✅ 5 Useful Phrases

- The EKS cluster upgrade has been successfully completed.
- We followed the rolling update approach with no downtime.
- Node groups were drained and upgraded in batches.
- Post-upgrade testing confirmed all services are healthy.
- Monitoring shows normal behavior after the change.

---

## 🧠 Word Bank (Industry Terms)

| Term | Meaning |
|------|---------|
| **EKS** | AWS Kubernetes service |
| **Control Plane** | Core components that manage the cluster |
| **Node Group** | Group of EC2 worker nodes in EKS |
| **Drain** | Safely evict pods before node upgrade |
| **Rolling Update** | Upgrade process without downtime |
