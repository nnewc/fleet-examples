# Multi-Cluster Helm Example

This example will deploy the [Kubernetes sample guestbook](https://github.com/kubernetes/examples/tree/master/guestbook/) application as
packaged as a Helm chart.
The app will be deployed into the `fleet-mc-helm-example` namespace.

The application will be customized as follows per environment:

* Dev clusters: Only the redis leader is deployed and not the followers.
* Test clusters: Scale the front deployment to 3
* Prod clusters: Scale the front deployment to 3 and set the service type to LoadBalancer

```yaml
kind: GitRepo
apiVersion: fleet.cattle.io/v1alpha1
metadata:
  name: helm
  namespace: fleet-default
spec:
  repo: https://github.com/rancher/fleet-examples
  paths:
  - multi-cluster/helm
  targets:
  - name: dev
    clusterSelector:
      matchLabels:
        env: dev

  - name: test
    clusterSelector:
      matchLabels:
        env: test

  - name: prod
    clusterSelector:
      matchLabels:
        env: prod
```

## Step-by-Step
These are the steps I did to create a GitOps workflow
### Forked fleet-examples
Just hit the "Fork" button above
### Created GitRepo Resource in Rancher
- Select "Continuous Delivery" from the drop down left navigation menu
- Create a "Git Repo" resource using the form, making sure to set the repo desired path (multi-cluster/helm).
- Edited the YAML of "fleet-local" Clusters resource to add a `env: prod` label. This enabled the "prod" cluster selector in fleet.yaml

