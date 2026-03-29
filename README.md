# Kubernetes Security Lab

This repository contains lab files for validating Kubernetes security controls on a 1-control-plane/2-worker cluster.

## Topics tested
- RBAC least privilege
- ServiceAccount token exposure and automount disabling
- Pod Security Admission
- Falco deployment and runtime alert visibility

## Key files
- rbac/rbac-test.yaml
- serviceaccount/sa-test.yaml
- serviceaccount/sa-test-no-token.yaml
- psa/namespace-baseline.yaml
- psa/privileged-pod.yaml
- falco/install-falco.sh
- falco/falco-values.yaml

## Useful commands
kubectl apply -f rbac/rbac-test.yaml
kubectl auth can-i get pods --as=system:serviceaccount:default:paper-sa
kubectl auth can-i get secrets --as=system:serviceaccount:default:paper-sa

kubectl apply -f serviceaccount/sa-test.yaml
kubectl exec -n default sa-demo -- ls -l /var/run/secrets/kubernetes.io/serviceaccount/

kubectl apply -f serviceaccount/sa-test-no-token.yaml
kubectl exec -n default sa-demo-no-token -- ls -l /var/run/secrets/kubernetes.io/serviceaccount/

kubectl apply -f psa/namespace-baseline.yaml
kubectl apply -f psa/privileged-pod.yaml

./falco/install-falco.sh
kubectl logs -n falco -l app.kubernetes.io/name=falco --tail=20
