## Remarks

### Kubernetes

After a bit of studying on my project, I managed to understand something important that needs to be underlined.

There is a reason why we do use `selector.matchLabels`. It is not a descriptive tag only, it servers a more important purpose: to define which Pods the ReplicaSet has to deal with.

If the selector label does not match with the one from the template, Kubernetes answers with an error since ReplicaSet is not able to adopt the Pods being made.

This behavior was tested by removing the `app` label from a running Pod with `kubectl label pod <name> app-`. The ReplicaSet immediately created a new Pod to restore 3 replicas. The orphaned Pod kept running but lost the `app=nginx-demo` label, verified with `--show-labels`.

The ReplicaSEt uses labels, not names, to track ownership. Kubernetes also adds `pod-template-hash` automatically to every Pod it creates, it is not written in the manifest.

---

NAME                         READY   STATUS    RESTARTS   AGE     LABELS
nginx-demo-f66d69596-xhnsn   1/1     Running   0          2m28s   pod-template-hash=f66d69596
NAME                         READY   STATUS    RESTARTS   AGE   LABELS
nginx-demo-f66d69596-pq2vq   1/1     Running   0          84s   app=nginx-demo,pod-template-hash=f66d69596
