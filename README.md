# preview-environments-example
A simple application with an Argo CD ApplicationSet that dynamically creates preview environments

# Create the base application with the Argo CD CLI
```
argocd app create base-app \
    --project default \
    --sync-policy automatic \
    --auto-prune --self-heal \
    --repo "https://github.com/IonOlaruAnant/preview-environments-example.git" \
    --revision HEAD \
    --path helm \
    --dest-name in-cluster \
    --dest-namespace apps-staging
```

# Create the ApplicationSet with the Argo CD CLI
```
argocd app create appset \
    --project default \
    --sync-policy automatic \
    --auto-prune --self-heal \
    --repo "https://github.com/IonOlaruAnant/preview-environments-example.git" \
    --revision HEAD \
    --path appset \
    --dest-name in-cluster \
    --dest-namespace argocd
```