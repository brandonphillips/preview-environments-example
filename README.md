# preview-environments-example
A simple application with an Argo CD ApplicationSet that dynamically creates preview environments

# ArgocCD login
```
argocd login $(kubectl get service argocd-server -n argocd --output=jsonpath='{.status.loadBalancer.ingress[0].hostname}') \
    --username admin \
    --password $(kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d; echo) \
    --insecure
```

# Create namespace
```
kubectl create ns apps-preview
```

# Create secrets
Github code and dockerhub images
```
kubectl create -f ./k8s/secret.yaml
```

# Create the base application with the Argo CD CLI
```
argocd app create go-webserver-base \
    --project default \
    --sync-policy automatic \
    --auto-prune --self-heal \
    --repo "https://github.com/IonOlaruAnant/preview-environments-example.git" \
    --revision HEAD \
    --path helm \
    --dest-name in-cluster \
    --dest-namespace apps-preview
```

# Create the ApplicationSet with the Argo CD CLI
```
argocd app create go-web-server-appset \
    --project default \
    --sync-policy automatic \
    --auto-prune --self-heal \
    --repo "https://github.com/IonOlaruAnant/preview-environments-example.git" \
    --revision HEAD \
    --path appset \
    --dest-name in-cluster \
    --dest-namespace argocd
```