# argo-workflows Helm chart
```
helm repo add argo https://argoproj.github.io/argo-helm
helm install my-argo-workflows argo/argo-workflows --version 0.40.3
```
https://artifacthub.io/packages/helm/argo/argo-workflows

# Install Argo Workflows
```
kubectl create namespace argo
kubectl apply -n argo -f https://github.com/argoproj/argo-workflows/releases/download/v3.5.2/install.yaml
```
# Patch argo-server authenticatio
```
kubectl patch deployment \
  argo-server \
  --namespace argo \
  --type='json' \
  -p='[{"op": "replace", "path": "/spec/template/spec/containers/0/args", "value": [
  "server",
  "--auth-mode=server"
]}]'
```

# Port-forward the UI
```
kubectl -n argo port-forward deployment/argo-server 2746:2746
```
https://localhost:2746/


# CLI
https://github.com/argoproj/argo-workflows/releases/

- Rename the argo-windows-amd64 to argo.exe
- Place it in a folder (eg. C:\argo-cli)
- Open the environment variables setting and add the folder under System Variables > PATH 
            ```
            setx PATH "%PATH%;C:\argo-cli" 
            ```
-To test, open cmd and type argo version

# Submitting an example workflow¶
## Submit an example workflow (CLI)
```
argo submit -n argo --watch https://raw.githubusercontent.com/argoproj/argo-workflows/main/examples/hello-world.yaml
argo submit -n argo --watch hello-world.yaml
argo list -n argo
argo get -n argo @latest
argo logs -n argo @latest
argo delete --all -n argo
argo submit Parameters.yaml -p message="goodbye world" -n argo
argo submit Parameters.yaml --parameter-file params.yaml -n argo


```