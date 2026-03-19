---
hidden: true
---
# Kubernetes Reference Guide
In our TurboStack you have the option to run applications that rely on Kubernetes. This is useful for workloads that benefit from orchestration, automated recovery, service discovery, and controlled rollouts.

Configuration is done on the _application_ level. This means you can enable **Kubernetes** only for the vhosts that need it.

## Configuring the server to use Kubernetes

### Via the TurboStack GUI
You can enable Kubernetes in the TurboStack GUI, via:

`Accounts > AccountName > Applications cogwheel > Technologies > K8s`

### Via the TurboStack YAML
You can add the following line to the application that needs Kubernetes enabled:

```yaml
k8s_enabled: true
```
After adding this, you can click _Save & Publish_ to start the installation.

### Configuration example
The following configuration example shows an application that requires Kubernetes support.
```yaml
system_users:
- username: example
  vhosts:
  - server_name: k8s.example.hosted-power.dev
    cert_type: selfsigned
    k8s_enabled: true
```

### Basic commands

List all pods:
```bash
kubectl get pods
```

List all services:
```bash
kubectl get services
```

List all deployments:
```bash
kubectl get deployments
```

View the logs for a pod:
```bash
kubectl logs <pod_name>
```

Get detailed information for a pod:
```bash
kubectl describe pod <pod_name>
```

Restart a deployment:
```bash
kubectl rollout restart deployment/<deployment_name>
```

Delete a pod:
```bash
kubectl delete pod <pod_name>
```
Within our system, we often work with tools that visualize and ease the way we work on kubernetes environments, please see our [Handy tools guide](handytools.md)
