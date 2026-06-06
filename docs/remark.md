## Remarks

### Kubernetes

After a bit of studying on my project, I managed to understand something important that needs to be underlined.

There is a reason why we do use `selector.matchLabels`. It is not a descriptive tag only, it servers a more important purpose: to define which Pods the ReplicaSet has to deal with.

If the selector label does not match with the one from the template, Kubernetes answers with an error since ReplicaSet is not able to adopt the Pods being made.
