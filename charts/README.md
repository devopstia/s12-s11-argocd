
helm repo add jenkins https://charts.jenkins.io
helm repo update
helm search repo jenkins/jenkins
helm pull jenkins/jenkins --untar

helm template jenkins ./jenkins \
  --namespace jenkins \
  > jenkins-rendered.yaml

helm template jenkins ./jenkins \
  --namespace jenkins \
  > jenkins-rendered.yaml

helm install jenkins ./jenkins \
  --namespace jenkins \
  --create-namespace \
  --dry-run

helm upgrade --install jenkins ./jenkins \
  --namespace jenkins \
  --create-namespace


kubectl scale deployment nginx-dev \
    -n nginx-dev \
    --replicas=1