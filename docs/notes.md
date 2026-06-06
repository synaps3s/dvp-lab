# Notes

Personal notes taken while exploring the tools in this lab. Not a tutorial, just some observations and things worth remembering.

---

## Kubernetes

### Core concepts

A Deployment does not manage containers directly. It creates a ReplicaSet, which in turn manage the pods. When you update a Deployment, Kubernetes creates a new ReplicaSet for the new version and scales the old one down gradually. This is why rollback is possible: the old ReplicaSet is kept at zero replicas, not deleted.

### The reconciliation loop

The Controller Manager runs a continuous loop comparing the desired state (what we wrote in the manifest) with the actual state (what is really running). When a pod crashes, the loop detects the gap and creates a new pod immediately. This happened in under 10 seconds during testing.

### Health probes

Two different probes give two different behaviors:
- `readinessProbe` - keeps the pod out of the service rotation until it is ready. Does not restart anything.
- `livenessProbe` - restarts the container if it stops responding. Does not affect traffic routing.

Mixing up their purpose is a common mistake. A pod can be live but not ready, and vice versa.

### Rolling update

Observed during `kubectl set image`:
Kubernetes created a new ReplicaSet with the updated image and replaced pods one at a time. The old ReplicaSet hash was `f66d69596`, the new one `5db6c94ff8`. At no point did the number of running pods drop below 2 out of 3.

Rollback with `kubectl rollout undo` reverses the process using the same mechanism and it just scales the old ReplicaSet back up.

### Commands worth remembering

- `kubectl get pods -w` - Watch pod state changes in real time
- `kubectl describe pod <name>` - Full detail including events (might be first stop when debugging)
- `kubectl logs <name> --previous` - Logs from a crashed container
- `kubectl rollout history deployment/<name>` - Revision history
- `kubectl rollout undo deployment/<name>` - Roll back to previous revision
- `kubectl delete pod <name> --grace-period=0 --force` - Simulate a hard crash

---

### Important to mention

Docker and Kubernetes is not the same. While Docker solves the "it runs on my device but not on the server", Kubernetes is the answer to a different problem: how to manage 50 containers in prod while having 10 different servers? Who decides on which server we have to put a certain container? Who is going to restart it the moment it crashes? Who is going to balance the traffic or who is going to update the containers without downtime?

What we can say is: Docker is the metal container on the cargo, while Kubernetes is the logistic system who decides where to put each container on the ship, how many to bring and what each one of them has to do.

We use both of them.

---

### How to talk to a Pod

In this scenario we are going to talk about Service. What is a Service and why do we use it?
Pods are quite tricky to communicate with, this since they have an unique ip who changes each time the Pod gets recreated. There is no point in sticking it in a hardcoded way.
Here is where we encounter the ClusterIP, the NodePort and the LoadBalancer service. They all service a different purpose.

The one I'm going to consider in this situation is the NodePort, mostly for a personal need.
The NodePort exposes the Service on a host port, or in few words the port of the machine where Kubernetes is running on. You can reach the app from outside, using the `IP-of-the-node:port` url.

The `.yaml` requires you to set up three ports:
- `port`
- `targetPort` - The one we set up inside the Deployment
- `nodePort`

Once we apply the service, all we need to do is to run `minikube service nginx-demo-service --url` (name may change from file to file).

### Important to mention

Inside our service conf, it is important to set the `selector` in order to match the app name we gave to the Pods inside the Deployment.
This all is related to the fact a different name lead to different and unexpected behaviors.

### Not Tested Yet

- ClusterIp: the default type. It creates a stable IP that is reachable from the cluster only. Pods can communicate between eachothers but we can't reach them from outside the cluster.

- LoadBalancer: it asks the cloud provider to make an external load balancer with a public IP who distributes the traffic on the Pods. No limitations.
