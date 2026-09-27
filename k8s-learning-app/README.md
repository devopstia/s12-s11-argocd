# Kubernetes Study

A full-stack web app that teaches Kubernetes — **and is itself a Kubernetes
lesson.** The website explains every API object and gives you a searchable
kubectl cheat sheet; the repository around it shows how a real three-tier
application is packaged, deployed, secured and observed in a cluster.

---

## What's in the box

**The website** — sign in, then:

- **57 Kubernetes objects**, each with a plain-English explanation of what it
  solves and how it works mechanically, a complete manifest you can `kubectl
  apply`, the fields worth knowing, common mistakes, and related objects.
- **216 kubectl commands**, grouped into 13 categories, searchable, with the
  non-obvious notes an experienced operator would tell you. Destructive commands
  are flagged.
- **Progress tracking** — mark things learned; the dashboard shows how far
  through you are.
- **Personal notes** on any object, autosaved.
- Full-text search across everything, dark mode, copy-to-clipboard everywhere.

**The infrastructure** — a three-tier deployment with 20 Kubernetes objects:
Deployments, a StatefulSet with persistent storage, Services, an Ingress,
NetworkPolicies, an HPA, PodDisruptionBudgets, a ResourceQuota, a LimitRange,
ConfigMap/Secret, a ServiceAccount, and an Argo CD Application.

Every manifest is commented to explain *why*, not just what.

---

## Stack

| Layer | Choice | Why |
|---|---|---|
| Frontend | React 18 + TypeScript + Vite, served by nginx | Static files; ~50MB image |
| Backend | Node 20 + TypeScript + Express | One language across the stack |
| Database | PostgreSQL 16 as a StatefulSet | Teaches PVCs, ordered rollout, stable identity |
| Auth | scrypt + JWT access tokens + rotating refresh tokens | No native dependencies |
| Logging | pino, structured JSON to stdout | What Kubernetes actually expects |
| Metrics | prom-client on `/metrics` | Point a ServiceMonitor at it |

---

## Run it locally (5 minutes, no cluster)

```bash
docker compose up --build
```

Compose starts Postgres, runs migrations, seeds the learning content, creates a
local admin user, then starts the API and web containers.

Open **http://localhost:8080** and sign in with the local admin account:

```text
Email: admin@example.com
Password: admin-password
```

You can also create a normal student account from the registration page.

Or run the pieces directly for hot reload:

```bash
docker compose up postgres -d          # just the database
cd backend  && cp ../.env.example .env && npm install && npm run dev
cd frontend && npm install && npm run dev     # http://localhost:5173
```

---

## Run it on Kubernetes

Works on kind, minikube, k3s, Docker Desktop or a real cluster.

```bash
# 1. Build and push images (or load them into kind/minikube directly)
export REGISTRY=ghcr.io/your-username
make push

# 2. Point the manifests at your registry
#    Edit k8s/kustomization.yaml and replace REPLACE_ME

# 3. Generate a real JWT secret and put it in k8s/01-config.yaml
openssl rand -base64 48

# 4. Deploy
make deploy
make status
```

Reach it without an Ingress:

```bash
make port-forward     # http://localhost:8080
```

With an Ingress, add `k8s-learn.local` to `/etc/hosts` pointing at your
ingress controller's IP.

### With Argo CD

```bash
# push this repo to git, set repoURL in argocd/application.yaml, then:
kubectl apply -f argocd/application.yaml
```

`selfHeal: true` means Argo reverts manual `kubectl edit` changes — try it and
watch it happen. It is the fastest way to understand what GitOps actually means.

---

## Docs

| File | What it covers |
|---|---|
| [`docs/ARCHITECTURE.md`](docs/ARCHITECTURE.md) | How the pieces fit, request flow, auth design, data model |
| [`docs/KUBERNETES-WALKTHROUGH.md`](docs/KUBERNETES-WALKTHROUGH.md) | **Start here.** A guided tour of every manifest, with exercises that break things on purpose |
| [`docs/API.md`](docs/API.md) | Every endpoint, with curl examples |

---

## Layout

```
backend/          Express + TypeScript API
  src/db/         migrations, pool, seeder
  src/routes/     auth, content, progress, health
  src/services/   password (scrypt), tokens, users
  content/        objects.json (57), commands.json (216)
frontend/         React + TypeScript + Vite
k8s/              20 Kubernetes manifests, heavily commented
argocd/           Argo CD Application
docs/             architecture, walkthrough, API reference
```

---

## Status

Verified working: the backend compiles and passes an end-to-end API test
(register, login, refresh, session, search, progress, metrics); the frontend
builds and was driven through every screen with a real browser; all 20 manifests
pass a cross-reference lint (selectors match templates, probes reference declared
ports, Ingress backends exist, the StatefulSet's Service is headless).

Not yet done: no automated test suite, no CI pipeline, and the images have never
been built by Docker in anger — see `docs/KUBERNETES-WALKTHROUGH.md` for what to
check first.
