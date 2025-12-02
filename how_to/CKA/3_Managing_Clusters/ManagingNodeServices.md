# Managing Node Services

The container runtime (often containerd) and kubelet are managed by the Linux systemd service manager

To check the status of the `kubelet`, `systemctl status kubelet`

You may use this if a node isnt available, and you checked if it isnt cordoned or not (it isnt), but node is still available. The next thing to check would be if the Kubelet is available, and if it isnt, manually restart it with `systemctl start kubelet` and possibly `systemctl enable kubelet`