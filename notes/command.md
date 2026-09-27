# Kubernetes Resource Shortcuts

Kubernetes provides built-in **short names** for many resources. These shortcuts can make `kubectl` commands faster and easier to type.

For example:

```bash
# Full command
kubectl get pods

# Using the resource shortcut
kubectl get po
```

## Kubernetes Resource Shortcuts

| Full Resource              | Shortcut      | Full Command                             | Shortcut Command     |
| -------------------------- | ------------- | ---------------------------------------- | -------------------- |
| Pods                       | `po`          | `kubectl get pods`                       | `kubectl get po`     |
| Services                   | `svc`         | `kubectl get services`                   | `kubectl get svc`    |
| Deployments                | `deploy`      | `kubectl get deployments`                | `kubectl get deploy` |
| ReplicaSets                | `rs`          | `kubectl get replicasets`                | `kubectl get rs`     |
| StatefulSets               | `sts`         | `kubectl get statefulsets`               | `kubectl get sts`    |
| DaemonSets                 | `ds`          | `kubectl get daemonsets`                 | `kubectl get ds`     |
| Namespaces                 | `ns`          | `kubectl get namespaces`                 | `kubectl get ns`     |
| ConfigMaps                 | `cm`          | `kubectl get configmaps`                 | `kubectl get cm`     |
| Secrets                    | —             | `kubectl get secrets`                    | `kubectl get secret` |
| ServiceAccounts            | `sa`          | `kubectl get serviceaccounts`            | `kubectl get sa`     |
| Nodes                      | `no`          | `kubectl get nodes`                      | `kubectl get no`     |
| PersistentVolumes          | `pv`          | `kubectl get persistentvolumes`          | `kubectl get pv`     |
| PersistentVolumeClaims     | `pvc`         | `kubectl get persistentvolumeclaims`     | `kubectl get pvc`    |
| StorageClasses             | `sc`          | `kubectl get storageclasses`             | `kubectl get sc`     |
| HorizontalPodAutoscalers   | `hpa`         | `kubectl get horizontalpodautoscalers`   | `kubectl get hpa`    |
| CronJobs                   | `cj`          | `kubectl get cronjobs`                   | `kubectl get cj`     |
| Jobs                       | —             | `kubectl get jobs`                       | `kubectl get job`    |
| Ingresses                  | `ing`         | `kubectl get ingresses`                  | `kubectl get ing`    |
| NetworkPolicies            | `netpol`      | `kubectl get networkpolicies`            | `kubectl get netpol` |
| Endpoints                  | `ep`          | `kubectl get endpoints`                  | `kubectl get ep`     |
| EndpointSlices             | `eps`         | `kubectl get endpointslices`             | `kubectl get eps`    |
| Events                     | `ev`          | `kubectl get events`                     | `kubectl get ev`     |
| CustomResourceDefinitions  | `crd`, `crds` | `kubectl get customresourcedefinitions`  | `kubectl get crd`    |
| PriorityClasses            | `pc`          | `kubectl get priorityclasses`            | `kubectl get pc`     |
| RuntimeClasses             | `rc`          | `kubectl get runtimeclasses`             | `kubectl get rc`     |
| ResourceQuotas             | `quota`       | `kubectl get resourcequotas`             | `kubectl get quota`  |
| LimitRanges                | `limits`      | `kubectl get limitranges`                | `kubectl get limits` |
| PodDisruptionBudgets       | `pdb`         | `kubectl get poddisruptionbudgets`       | `kubectl get pdb`    |
| CertificateSigningRequests | `csr`         | `kubectl get certificatesigningrequests` | `kubectl get csr`    |
| ClusterRoles               | `cr`          | `kubectl get clusterroles`               | `kubectl get cr`     |
| ClusterRoleBindings        | `crb`         | `kubectl get clusterrolebindings`        | `kubectl get crb`    |
| Roles                      | —             | `kubectl get roles`                      | `kubectl get role`   |
| RoleBindings               | `rb`          | `kubectl get rolebindings`               | `kubectl get rb`     |

---

# Usage Notes

## Pods

Full command:

```bash
kubectl get pods
```

Shortcut:

```bash
kubectl get po
```

## ConfigMaps

Full command:

```bash
kubectl get configmaps
```

Shortcut:

```bash
kubectl get cm
```

## Namespaces

Full command:

```bash
kubectl get namespaces
```

Shortcut:

```bash
kubectl get ns
```

## Deployments

Full command:

```bash
kubectl get deployments
```

Shortcut:

```bash
kubectl get deploy
```

---

# Short Names Work With Other kubectl Commands

These short names are **resource short names**, not shortcuts for only `kubectl get`.

You can also use them with commands such as `describe` and `delete`.

## Describe a Pod

Full:

```bash
kubectl describe pods nginx
```

Shortcut:

```bash
kubectl describe po nginx
```

## Delete a Pod

Full:

```bash
kubectl delete pods nginx
```

Shortcut:

```bash
kubectl delete po nginx
```

## Get Pods From All Namespaces

Full:

```bash
kubectl get pods --all-namespaces
```

Shortcut:

```bash
kubectl get po -A
```

## Get Pods From a Specific Namespace

Full:

```bash
kubectl get pods -n development
```

Shortcut:

```bash
kubectl get po -n development
```

---

# Discover Kubernetes Resource Shortcuts

You do not have to memorize every Kubernetes resource shortcut.

Run:

```bash
kubectl api-resources
```

This displays Kubernetes resources and their available short names.

For more information:

```bash
kubectl api-resources -o wide
```

Look at the **SHORTNAMES** column.

Example:

```text
NAME                     SHORTNAMES
configmaps               cm
namespaces               ns
nodes                    no
pods                     po
services                 svc
daemonsets               ds
deployments              deploy
replicasets               rs
statefulsets             sts
horizontalpodautoscalers hpa
```

This is especially useful when working with **Custom Resource Definitions (CRDs)** because operators may introduce additional resource types and short names.

---

# Make kubectl Even Shorter

If you use Kubernetes frequently, you can create a shell alias for `kubectl`.

```bash
alias k='kubectl'
```

Instead of:

```bash
kubectl get pods
```

you can then use:

```bash
k get po
```

Other examples:

```bash
k get po
k get po -A
k get po -n development
k get svc
k get deploy
k get sts
k get ds
k get no
k get ns
k get cm
k get secret
k get pvc
k get sc
k get ing
k get crd
```

For example:

```bash
kubectl get pods --all-namespaces
```

becomes:

```bash
k get po -A
```

---

## Important Tip

Always remember that not every Kubernetes resource has a short name.

If you are unsure, use:

```bash
kubectl api-resources
```

This is the best way to confirm which resource names and shortcuts are available on the Kubernetes cluster you are currently connected to.
