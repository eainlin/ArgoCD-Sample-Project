# Setup argocd to deploy k8s manifests


## Get the argocd CLI
```bash
$ wget https://github.com/argoproj/argo-cd/releases/download/v2.6.1/argocd-linux-amd64
$ chmod +x argocd-linux-amd64
$ mv argocd-linux-amd64 /usr/bin/argocd
```

Check you can now run `argocd` commands:
```bash
$ argocd version
argocd: v2.3.3+07ac038
  BuildDate: 2022-03-30T01:46:59Z
```

## Install Argo In Your Cluster
```bash
$ kubectl create namespace argocd
namespace/argocd created
```

```bash
$ kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
```

```bash
$ kubectl get deployments -n argocd
NAME                               READY   UP-TO-DATE   AVAILABLE   AGE
argocd-applicationset-controller   1/1     1            1           67s
argocd-dex-server                  1/1     1            1           67s
argocd-notifications-controller    1/1     1            1           67s
argocd-redis                       1/1     1            1           67s
argocd-repo-server                 1/1     1            1           67s
argocd-server                      1/1     1            1           67s

```

## Connecting to Argo
```bash
$ kubectl port-forward svc/argocd-server -n argocd 8080:443
```

### Get Password
```bash
$ argocd admin initial-password -n argocd
xxxxxxxxxx
```
delete the Kubernetes secret that contains the original password for the admin ac
ount:

```bash
$ kubectl delete secret argocd-initial-admin-secret -n argocd
secret "argocd-initial-admin-secret" deleted
```

## Login to the CLI

```bash
$ argocd login localhost:8080
```
Similarly to the browser warning encountered above, you’ll be prompted to accept Argo’s built-in self-signed certificate if you haven’t configured your own TLS:
```
WARNING: server certificate had error: x509: certificate signed by unknown authority. Proceed insecurely (y/n)?
```
Accept the prompt by typing y and pressing return.
```
'admin:login' logged in successfully
Context 'localhost:8080' updated
```
