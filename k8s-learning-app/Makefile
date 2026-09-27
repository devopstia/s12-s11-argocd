# Convenience targets. Every one of these is a command you could type by hand —
# read them rather than treating them as magic.
REGISTRY ?= ghcr.io/REPLACE_ME
TAG      ?= latest
NS       ?= k8s-learn

.PHONY: help dev build push deploy undeploy logs status port-forward psql reset-db

help:
	@grep -E '^[a-z-]+:.*?##' $(MAKEFILE_LIST) | sed 's/:.*##/\t/'

dev:            ## run everything locally with docker compose
	docker compose up --build

build:          ## build both images
	docker build -t $(REGISTRY)/k8s-learn-api:$(TAG) ./backend
	docker build -t $(REGISTRY)/k8s-learn-web:$(TAG) ./frontend

push: build     ## push both images
	docker push $(REGISTRY)/k8s-learn-api:$(TAG)
	docker push $(REGISTRY)/k8s-learn-web:$(TAG)

deploy:         ## apply the manifests
	kubectl apply -k k8s/
	kubectl -n $(NS) rollout status deploy/api --timeout=180s
	kubectl -n $(NS) rollout status deploy/web --timeout=120s

undeploy:       ## remove everything EXCEPT the PVC (your data survives)
	kubectl delete -k k8s/ --ignore-not-found

logs:           ## tail API logs across all replicas
	kubectl -n $(NS) logs -l app.kubernetes.io/name=api --all-containers -f --tail=50

status:         ## what is actually running
	kubectl -n $(NS) get pods,svc,ingress,pvc,hpa -o wide

port-forward:   ## reach the app without an Ingress
	@echo "web -> http://localhost:8080"
	kubectl -n $(NS) port-forward svc/web 8080:80

psql:           ## open a shell on the database
	kubectl -n $(NS) exec -it postgres-0 -- psql -U k8slearn -d k8slearn

reset-db:       
	kubectl -n $(NS) delete statefulset postgres --ignore-not-found
	kubectl -n $(NS) delete pvc data-postgres-0 --ignore-not-found
