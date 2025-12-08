# Configuring Pod Priorities

## Understanding Scheduling Priorities
By default, the kube-scheduler doesnt have any priorities

If you want to determine the order in which Pods are scheduled and evicted when there are resource contstraints, consider using the PriorityClass resource

Each PriorityClass has a value, and adding a higher value to it gives it a higher priority

Pods need to be configured with a `priorityClassName` to use a certain `PriorityClass`

A `PriorityClass` can be set as `globalDefault`, which means that Pods that don't have a certain PriorityClass set will schedule with this PriorityClass

When PriorityClass is used, and the cluster runs out of resources, low priority Pods will be evicted to make place for higher priority resources

While creating a PriorityClass, the `preemptionPolicy` can be set to `never` to ensure that Pods will never be evicted 

## Demo using PriorityClass

```bash
kubectl create priorityclass -h | less

# Create a priority class named high-priority that cannot preempt pods with lower priority
kubectl create priorityclass high-priority --value=1000 --description="high priority" --preemption-policy="Never"

kubectl create deployment highpriority --image=nginx
kubectl edit deployment highpriority
spec.template.spec.priorityClassName: highPriority
```
**Info**
What 'cannot preempt lower-priority pods' means
Kubernetes has only two possible values for `--preemption-policy`:

-`PreemptLowerPriority` → (default) allows this priority class to kick out (preempt) lower-priority pods when nodes are full.
-`Never` → this priority class will never preempt any pod, even if the cluster is full and lower-priority pods are running.

So by setting `--preemption-policy=Never`, your high-priority pods are still very important (value 1 000 000 is higher than almost everything), but they are “polite”:
they will wait until resources become available instead of killing lower-priority workloads.

### GlobalDefault
If you wanted to set a default PriorityClass for all Pods that don't explicitly have a PriotiyClass accross your entire cluster set you would use `--global-default=true` but mostly this would be set
```bash
kubectl create priorityclass mid-priority \
  --value=125 \
  --description="mid priority" \
  --global-default=true
```
Now creating a standard pod, would be assigned to the `global-default` `PriorityClass` which is `mid-prioirty` in this example