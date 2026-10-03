## Argocd installation
https://argo-cd.readthedocs.io/en/stable/getting_started/

```
kubectl create namespace argocd
kubectl apply -n argocd --server-side --force-conflicts -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
kubectl patch svc argocd-server -n argocd -p '{"spec": {"type": "LoadBalancer"}}'

argocd admin initial-password -n argocd
username: admin
passwor: S12-11-K8S/argocd-cd S12-11-K8S/command.md

argocd login <ARGOCD_SERVER>
argocd login 165.227.254.154
```

https://165.227.254.154/settings/repos


NS=argocd
kubectl get namespace "$NS" -o json \
  | tr -d "\n" | sed "s/\"finalizers\": \[[^]]\+\]/\"finalizers\": []/" \
  | kubectl replace --raw "/api/v1/namespaces/$NS/finalize" -f -