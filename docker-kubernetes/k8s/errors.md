
1. `CrashLoopBackOff`
Your pod starts, fails, and restarts over and over.
 
This usually means your app is crashing during startup.

 Check the logs with: `kubectl logs <pod-name>`


2. `ImagePullBackOff`
Kubernetes can't pull your container image.

Could be a wrong image name, wrong tag, or missing registry credentials.

Double-check your `image:` field in the manifest.


3. `OOMKilled`
Your container used more memory than it was allowed and the system said “nope.”

 This is an Out of Memory (OOM) error.
 Increase memory limits or optimize app usage.


4. `NodeNotReady`
Your pod can’t be scheduled because the node isn’t healthy.

 This could be due to high resource pressure, network issues, or node being cordoned.

 Run: `kubectl describe node <node-name>`


5. `ContainerCreating` (and it’s stuck)
The pod is waiting on something likely storage or networking setup.

 Check for Persistent Volume issues or CNI plugin problems.

 Describe the pod to dig deeper: `kubectl describe pod <pod-name>`


6. Evicted
Kubernetes had to kick out your pod to free up resources (usually memory).

 Happens when the node runs low on available memory.

 Use `kubectl get events` to check for eviction details.

 Debugging Kubernetes is mostly about knowing what the error actually means once you do, troubleshooting becomes way less scary.

Hi, I’m Samuel Happiness I write about DevOps, SRE, and cloud infrastructure.
If you find content like this helpful, let’s connect 😉