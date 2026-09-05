# s12-s11-argocd

## Get argocd default password
```sh
kubectl -n argocd get secret argocd-initial-admin-secret \
  -o jsonpath="{.data.password}" | base64 -d && echo
```

admin
8THFlGOmBmdleQ43
argocd login <ARGOCD_SERVER>
argocd login 134.199.179.127


OmAAvKpelGogYyIk
argocd login https://argocd.dev.webforxtech.com

# List applications
kubectl get applications -n argocd

# Short form
kubectl get app -n argocd

# Get more information
kubectl get app -n argocd -o wide

# Describe a specific application
kubectl describe app <application-name> -n argocd

# Get the complete Application YAML
kubectl get app <application-name> -n argocd -o yaml

## Encrypt the password
argocd account bcrypt --password <YOUR-PASSWORD-HERE>
argocd account bcrypt --password DevOpsEasyLearning2026@
$2a$10$gahcEL/FBhKbj4lCg3/l8upN5GuaoUX0euwP1LdrtQJ.TveOq/Jka

# bcrypt(password)=$2a$10$gahcEL/FBhKbj4lCg3/l8upN5GuaoUX0euwP1LdrtQJ.TveOq/Jka
kubectl -n argocd patch secret argocd-secret \
  -p '{"stringData": {
    "admin.password": "$2a$10$gahcEL/FBhKbj4lCg3/l8upN5GuaoUX0euwP1LdrtQJ.TveOq/Jka",
    "admin.passwordMtime": "'$(date +%FT%T%Z)'"
  }}'

# Argo CD Local User Management

This guide shows how to create:

1. A **Read-Only Argo CD user**
2. An **Admin Argo CD user**

---

# 1. Create a Read-Only User

Example username:

```text
readonly-user
```

## Step 1 — Create the User

Edit the Argo CD ConfigMap:

```bash
kubectl edit configmap argocd-cm -n argocd
```

Add under `data:`:

```yaml
accounts.readonly-user: login
```

---

## Step 2 — Assign Read-Only Access

Edit the RBAC ConfigMap:

```bash
kubectl edit configmap argocd-rbac-cm -n argocd
```

Add to `policy.csv`:

```text
g, readonly-user, role:readonly
```

Example:

```yaml
data:
  policy.csv: |
    g, readonly-user, role:readonly
```

> If `policy.csv` already contains other rules, keep them and simply add the new rule.

---

## Step 3 — Set the Password

Login as the existing Argo CD admin:

```bash
argocd login <ARGOCD-SERVER> --username admin
```

Set the password:

```bash
argocd account update-password \
  --account readonly-user
```

Follow the prompts to set the password.

---

## Step 4 — Verify the User

```bash
argocd account list
```

Login with the new user:

```bash
argocd login <ARGOCD-SERVER> \
  --username readonly-user

argocd login 134.199.179.127 \
  --username readonly-user
```

Test access:

```bash
argocd app list
```

```bash
argocd proj list
```

The user can view Argo CD resources but should not be able to perform actions such as:

```bash
argocd app sync <APP-NAME>
```

---

# 2. Create an Admin User

Example username:

```text
argocd-admin
```

## Step 1 — Create the Admin User

Edit:

```bash
kubectl edit configmap argocd-cm -n argocd
```

Add:

```yaml
accounts.argocd-admin: login
```

---

## Step 2 — Assign Admin Access

Edit:

```bash
kubectl edit configmap argocd-rbac-cm -n argocd
```

Add:

```text
g, argocd-admin, role:admin
```

Example:

```yaml
data:
  policy.csv: |
    g, readonly-user, role:readonly
    g, argocd-admin, role:admin
```

---

## Step 3 — Set the Admin User Password

While logged in as an existing administrator:

```bash
argocd account update-password \
  --account argocd-admin
```

Follow the prompts to set the password.

---

## Step 4 — Verify

```bash
argocd account list
```

Login:

```bash
argocd login <ARGOCD-SERVER> \
  --username argocd-admin
```

Verify:

```bash
argocd account get-user-info
```

---

# Final Configuration

## argocd-cm

```yaml
data:
  accounts.readonly-user: login
  accounts.argocd-admin: login
```

## argocd-rbac-cm

```yaml
data:
  policy.csv: |
    g, readonly-user, role:readonly
    g, argocd-admin, role:admin
```

## Access Summary

| User | Role | Access |
|---|---|---|
| `readonly-user` | `role:readonly` | View only |
| `argocd-admin` | `role:admin` | Full administrative access |


## Argocd login
```sh
http://134.199.179.127/

# Admin account
Username: admin
Password: DevOpsEasyLearning2026@

# Readonly account
Username: readonly-user
Password: DevOpsEasyLearning2026@
```