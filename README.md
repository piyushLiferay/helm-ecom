```
apt install nginx 

helm repo add bitnami https://charts.bitnami.com/bitnami
helm search repo bitnami | grep nginx

#Instance of a release
helm install nginxv1 bitnami/nginx \
--set pdb.create=false

kubectl get secrets -n aurmlyurzmebyygcle | grep nginxv1


helm repo add eks https://aws.github.io/eks-charts
helm repo update eks
helm search repo eks
helm search repo eks | grep load

helm uninstall nginxv1
```

# Creating Helm Repositories for Payments and Shipping Services
```
mkdir -p helm-repo/{payments,shipping}
cd helm-repo
```
```
helm package payment
helm package shipping
```

# Deployment Template
```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: {{ .Release.Name }}-{{ .Chart.Name }}
  labels:
    app: {{ .Chart.Name }}
spec:
  replicas: 1
  selector:
    matchLabels:
      app: {{ .Chart.Name }}
  template:
    metadata:
      labels:
        app: {{ .Chart.Name }}
    spec:
      containers:
        - name: {{ .Chart.Name }}
          image: {{ .Values.image.repository }}:{{ .Values.image.tag }}
          command: ['sh', '-c', 'echo {{ .Values.appMessage }}; sleep 3600']
          imagePullPolicy: {{ .Values.image.pullPolicy }}
```

# Payments Chart's value.yaml
```yaml
image:
  repository: busybox
  tag: latest
  pullPolicy: IfNotPresent
appMessage: "Payments Service"
```


# Package the Charts
```
helm package payments
helm package shipping
helm repo index .
```

# Using the helm-ecom Repo
```
helm repo add helmecomRepo https://piyushghosh017.github.io/helm-ecom
helm repo update
helm search repo helmecomRepo

helm install payments-service myrepo/payment
```



# helm status
```
kubectl get secrets -n aurmlyurzmebyygcle | grep helm
helm status nginxv1 -n aurmlyurzmebyygcle
```

# UPGRADE
```
helm upgrade nginxv1 bitnami/nginx \
--set replicaCount=2 \
--set pdb.create=false
```

# clean up resource
```
 kubectl delete deployment nginxv1
kubectl delete svc nginxv1
kubectl delete secret nginxv1-tls
```




