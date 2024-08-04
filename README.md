# Create a Kind cluster using terraform 
1- Clone repo \
2- 
```terraform init```

3- Run script - create-cluster.sh \
4- To destroy the cluster:\
```terraform destroy -auto-approve```

## Argocd installation
```
kubectl create namespace argocd 
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
kubectl patch svc argocd-server -n argocd -p '{"spec": {"type": "NodePort"}}'
kubectl patch svc argocd-server -n argocd -p '{"spec": {"ports": [{"port": 443, "nodePort": 32000}]}}'
```
### Get argocd password
```
kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d | awk '{sub(/[-%]+$/, ""); print}'
```
