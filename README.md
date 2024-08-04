# Create a Kind cluster using terraform 
1- Clone repo \
2- 
```terraform init```

3- Run script - create-cluster.sh \
4- To destroy the cluster:\
```terraform destroy -auto-approve```

# Metallb installation

```
kubectl apply -f https://raw.githubusercontent.com/metallb/metallb/v0.14.8/config/manifests/metallb-native.yaml
```

**Configuration**\
1- Get docker network CIDR, run:
```
docker network inspect -f '{{.IPAM.Config}}' kind
```
2- Create the metallb-configmap.yaml
```
cat <<EOF > metallb-configmap.yaml
# Create a MetalLB config map (modify with your IP address range)
apiVersion: v1
kind: ConfigMap
metadata:
  namespace: metallb-system
  name: config
data:
  config: |
    address-pools:
    - name: default
      protocol: layer2
      addresses:
      - 172.26.255.200-172.26.255.250
EOF
```

3- Apply configuration:
```
kaf metallb-configmap.yaml
```
# Argocd installation
```
kubectl create namespace argocd 
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
kubectl patch svc argocd-server -n argocd -p '{"spec": {"type": "NodePort"}}'
kubectl patch svc argocd-server -n argocd -p '{"spec": {"ports": [{"port": 443, "nodePort": 32000}]}}'
```
# Get argocd password
```
kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d | awk '{sub(/[-%]+$/, ""); print}'
```
