Add this to your `README.md`.

### Install CloudNativePG Operator

```bash
kubectl create namespace cnpg-system

kubectl apply --server-side -f \
https://raw.githubusercontent.com/cloudnative-pg/cloudnative-pg/release-1.29/releases/cnpg-1.29.1.yaml

kubectl rollout status deployment/cnpg-controller-manager -n cnpg-system

kubectl get pods -n cnpg-system
```

---

### Install ECK Operator

```bash
kubectl create namespace elastic-system

kubectl apply -f \
https://download.elastic.co/downloads/eck/3.1.0/crds.yaml

kubectl apply -f \
https://download.elastic.co/downloads/eck/3.1.0/operator.yaml

kubectl rollout status statefulset/elastic-operator -n elastic-system

kubectl get pods -n elastic-system
```

---

### Verify CRDs

```bash
kubectl get crd | grep cnpg

kubectl get crd | grep elastic
```

Expected:

```text
clusters.postgresql.cnpg.io
backups.postgresql.cnpg.io
elasticsearches.elasticsearch.k8s.elastic.co
kibanas.kibana.k8s.elastic.co
```

---

### Architecture

```text
Kind/Minikube
    |
    +-- ArgoCD
    |
    +-- CloudNativePG Operator
    |      |
    |      +-- PostgreSQL Cluster
    |
    +-- ECK Operator
           |
           +-- Elasticsearch Cluster
           +-- Kibana
```

For POC16, this is enough in the README. The operators can be installed manually before ArgoCD deploys the database resources.
