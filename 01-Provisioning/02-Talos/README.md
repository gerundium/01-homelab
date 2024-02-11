# 01-Homelab/01-Provisioning/02-Talos
This section describes how to deploy a talos on virtual machines.

### 1. Prerequisites

#### 1.1 Install talosctl
This tool manages the talos k8s cluster.

```bash
curl -sL https://talos.dev/install | sh
```

### 2 Configuration
*Create configuration files*
```bash
talosctl gen config <NAME-OF-TALOS-CLUSTER> https://<CONTROLPLANE-IP>:6443 --output-dir config
```

### 3 Deployment
*Install Controlplane*
```bash
talosctl apply-config --insecure \
    --nodes <CONTROLPLANE-IP> \
    --file config/controlplane.yaml
```
*Install Worker*
```bash
# Worker 1
talosctl apply-config --insecure \
    --nodes <WORKER-1-IP> \
    --file config/worker.yaml
# Worker 2
talosctl apply-config --insecure \
    --nodes <WORKER-2-IP> \
    --file config/worker.yaml
```
*Verify configuration*
```bash
talosctl --talosconfig=./talosconfig \
    --nodes <CONTROLPLANE-IP> -e <CONTROLPLANE-IP> version
```
*Bootstrap etcd*
```bash
talosctl bootstrap --nodes <CONTROLPLANE-IP> --endpoints <CONTROLPLANE-IP> \
  --talosconfig=./config/talosconfig
```
*Export kubeconfig*
```bash
talosctl --talosconfig=./config/talosconfig kubeconfig --nodes <CONTROLPLANE-IP> --endpoints <CONTROLPLANE-IP>

# Alternative if kubeconfig should NOT be merged
talosctl --talosconfig=./config/talosconfig kubeconfig alternative-kubeconfig --nodes <CONTROLPLANE-IP> --endpoints <CONTROLPLANE-IP>

```