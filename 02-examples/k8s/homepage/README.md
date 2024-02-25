# Homepage

## Info

A modern, fully static, fast, secure fully proxied, highly customizable application dashboard with integrations for over 100 services and translations into multiple languages. Easily configured via YAML files or through docker label discovery.

***SOURCE***: https://gethomepage.dev/latest/

[<img src="https://gethomepage.dev/latest/assets/homepage_demo.png" width="600" height="300"
/>](https://gethomepage.dev/latest/)

- [Install Homepage Dashboard on Kubernetes](https://gethomepage.dev/latest/installation/k8s)

## TL;DR;

### Step-by-step approach

1. Deploy helm chart
```bash
helm repo add jameswynn https://jameswynn.github.io/helm-charts
helm install homepage jameswynn/homepage --set serviceAccount.create=true
```