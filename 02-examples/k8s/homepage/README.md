# Homepage

- [Install Homepage Dashboard on Kubernetes](https://gethomepage.dev/latest/installation/k8s)

## Step-by-step approach

1. Deploy helm chart
```bash
helm repo add jameswynn https://jameswynn.github.io/helm-charts
helm install homepage jameswynn/homepage --set serviceAccount.create=true
```