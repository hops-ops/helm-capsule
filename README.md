# helm-capsule

Crossplane configuration that wraps the [Capsule](https://github.com/projectcapsule/capsule) Helm chart, providing a minimal, stable interface for deploying multi-tenancy on Kubernetes.

## Usage

### Minimal

```yaml
apiVersion: helm.hops.ops.com.ai/v1alpha1
kind: Capsule
metadata:
  name: capsule
  namespace: my-env
spec:
  clusterName: my-cluster
```

### With Custom Configuration

```yaml
apiVersion: helm.hops.ops.com.ai/v1alpha1
kind: Capsule
metadata:
  name: capsule
  namespace: my-env
spec:
  clusterName: my-cluster

  labels:
    team: platform

  helm:
    namespace: capsule-system
    values:
      manager:
        options:
          forceTenantPrefix: false
          protectedNamespaceRegex: "^kube-|^default$|^capsule-"
      serviceMonitor:
        enabled: true
```

## Configuration

| Field | Description | Default |
|-------|-------------|---------|
| `spec.clusterName` | Target cluster name (used as default providerConfigRef) | Required |
| `spec.labels` | Labels applied to all resources | `{}` |
| `spec.providerConfigRef.name` | Helm ProviderConfig name | `<clusterName>` |
| `spec.providerConfigRef.kind` | Helm ProviderConfig kind | `ProviderConfig` |
| `spec.helm.namespace` | Namespace for the Helm release | `capsule-system` |
| `spec.helm.name` | Name of the Helm release | `capsule` |
| `spec.helm.values` | Helm values (merged with defaults) | `{}` |
| `spec.helm.overrideAllValues` | Helm values that replace all defaults | `{}` |

## Chart Info

- **Chart**: capsule
- **Repository**: https://projectcapsule.github.io/charts
- **Version**: 0.12.4
