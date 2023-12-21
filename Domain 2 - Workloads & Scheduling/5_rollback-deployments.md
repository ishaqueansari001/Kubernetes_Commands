#### View previous rollout revisions history
```sh
kubectl rollout history deployment/kplabs-deployment
```
#### View specific previous rollout revision history
```sh
kubectl rollout history deployment/kplabs-deployment --revision=1
```

#### Rollback to previous Revision
```sh
kubectl rollout undo deployment/kplabs-deployment --to-revision=1

kubectl get rs

kubectl describe deployment kplabs-deployment

kubectl rollout history deployment/kplabs-deployment
```
