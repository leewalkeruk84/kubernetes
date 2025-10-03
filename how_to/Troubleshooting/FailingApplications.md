# Analyzing Failing Applications

## Pod States
> In its lifetime a Pod will go through different states
> - Pending: the Pod has been validated by the API Server and an entry has been created in the etcd, but some prerequisite conditions have not been met
> - Running: the Pod currently is successfully running
> - Completed: the Pod has completed its work
> - Failed: the Pod has finished but something has gone wrong
> - CrashLoopBackOff: the Pod has failed, and the cluster has restarted it
> - Unknown: the Pod status could not be obtained
> Current Pod status can be observed using
> - ```kubectl get pods ```

## Troubleshooting Failed Applications
> First use
> ```kubectl describe ```
> - First look at the 'events'. then look at application state
> - Check last state, particulary the app exit code
> - if exit code is zero, the app started successfully, and no further investigation is needed here
> - If it is not 0, then you need to use kubectl logs to investigate the app logs
> - If app is continually restarting use the --previous to check logs for Pod that is no longer running
> - ```kubectl logs --previous ```