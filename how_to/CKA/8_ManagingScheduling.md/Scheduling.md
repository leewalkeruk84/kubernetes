# Exploring the Scheduling Process

## Understanding Scheduling
Kube-scheduler takes care of finding a node to schedule new Pods

Nodes are filtered according to specific requirements that may be set
- Resource requirements
- Affinity and anit-affinity
- Taints and tolerations and more

The scheduler first finds feasible nodes then scores them; it picks the node with the highest score

Once the node is found, the scheduler notifies the API server in a process called binding

If anything goes wrong in this phase, the Pod will show as Pending, or show an Error status

## From Scheduler to Kubelet
Once the scheduler decision has been made, it is picked up by the Kubelet

The Kubelet will instruct the CRI to fetch the image of the required container

After fetching the image and storing it on that specific node, the container is created and started