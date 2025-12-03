# Managing Security Context
For exam use [SecurityContext](https://kubernetes.io/docs/tasks/configure-pod-container/security-context/)

## Understanding SecrutiyContext
A SecurityContext defines privilege and access control settings for Pods or containers and can include the following:
- UID- and GID- based Dicrectionary Access Control
- SELinux security labels
- Linux Capabilities
- AppArmor
- Seccomp
- The AllowPrivilegeEscalation setting
- The runAs NonRoot setting

## Setting SecurityContext
- SecurityContext can be set at Pod level as well as container level
- See `kubectl explain pod.spec.secuurityContext`
- See `kubectl explain pod.spec.containers.secuurityContext`
- Settings applied at the container level will overwrite settings applied at the Pod level

### Demo
```bash
# security-context.yaml
apiVersion: v1
kind: Pod
metadata:
  name: security-context-demo
spec:
  securityContext:
    runAsUser: 1000
    runAsGroup: 1000
    fsGroup: 2000
  volumes:
  - name: securevol
    emptyDir: {}
  containers:
  - name: sec-demo
    image: busybox
    command: ["sh", "-c", "sleep 3600"]
    volumeMounts:
    - name: securevol
      mountPath: /data/demo
    securityContext:
      allowPrivilegeEscalation: false
```

```bash
kubectl apply -f security-context.yaml

kubectl get pod security-text-demo

kubectl exec -it security-context-demo -- sh
 - ps
 - cd /data; ls -l
 - id
 - exit
```