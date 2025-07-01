# Cluster Upgrdation Plan
- No downtime to the existing applications but no new deployment during upgradation
- We notify all the teams about cluster upgradation you did not have any acccess to deployment but you can access the application but cannot access cluster 1 hour 2 hours

Each every step we do during cluster upgradation we notify all stake holders

1. Create new called as green NodeGroup
2. Cordon the newly created nodes so that no pods placements to new nodegroup nodes
3. Upgrade Control plane from UI nearly it will took 5 to 10 mins and also update cluster version in terraform
   If Control plane down
   - we cannot access,we cannot do deployments and no releases
4. Upgrade Green Node Group also using console and update version in terraform
5. shift workloads from previous node group to gree node group
6. cordon previous node group
7. uncordon gree node group
8. drain previous node group

Once it is complete inform all the stake holders and applications teams and perform sanity testing

Close the activity

# Questions
1. What is the reason for upgrading cluster version from UI why not through terraform 