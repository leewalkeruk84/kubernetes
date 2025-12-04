# Using StatefulSets

[StatefulSets](https://kubernetes.io/docs/concepts/workloads/controllers/statefulset/)

## Understanding Stateful and Stateless Applications

A stateless application is an application that doesnt store any session data. Redirecting traffic in a stateless application is easy, the traffic can just be directed to another Pod instance

A statefull application saves session data to persistant storage. Databases are a example of a statefull application

## Userstanding StatefulSet
A StatefulSet offers features that are needed by stateful applications
- It provides guarantees about ordering and uniqueness of Pods
- It maintains a sticky identifier for each of the Pods it creates
- Pods in a StatefulSet are not interchangeable: each Pod has a persistent identifier that it maintains while being rescheduled
- The unique Pod identifiers make it easier to match existing volumes to replaced Pods

## When to Use
StatefullSet is used for applications that require one or more of the following:
- Stable and unique network identifiers
- Stable persistent storage
- Ordered, gracefull deployment and scaling
- Ordered and autoamted rolling update
If none of these are needed, a Deployment should be used

## Stateful Considerations
Storage must be automatically provisioned by a persistent volume provisioner. Pre-provisioning is challenging, as volumes need to be dynamically added when new Pods are scheduled

When a StatefulSet is deleted, associated volumes will not be deleted

A headless Service resource must be created in order to manage the network identity of Pods

Pods are not guarenteed to be stopped while deleting a StatefulSet, and it is recommended to scale down to zero Pods before deleting the StatefulSet

## Demo / Tutorial

Follow along here [StatefulSet Basics](https://kubernetes.io/docs/tutorials/stateful-application/basic-stateful-set/)

