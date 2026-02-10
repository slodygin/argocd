# Usefull commands
```
k get applications -n argocd
argocd app list
argocd app get APPNAME --refresh
argocd app get APPNAME --hard-refresh
kubectl annotate app/APPNAME argocd.argoproj.io/refresh="hard"

```
# How to create argocd user for gitlab
<a name="argocd319"></a>
```
#1. add to configmap
k edit cm argocd-cm -n argocd
data:
  accounts.gitlab: login
  accounts.gitlab.enabled: "true"
#2. set password
argocd account update-password --account gitlab --new-password  NEWPASS
#3. add permissions
k edit configmap argocd-rbac-cm -n argocd
data:
  policy.csv: |
    p,gitlab,applications, get, */*,allow
    p,gitlab,applications, update, */*,allow
#4. try to login
argocd login ARGO_URL
argocd app list
argocd app set APPNAME -p image.tag=dev-latest
```

# How to limit k8s permissions for argocd user
https://github.com/argoproj/argo-cd/issues/5389#issuecomment-808977225
