
### REvision 
<img src="rev.png">

### Application access using Ingress controller 

<img src="ing.png">

### Verify last deployment of mysql db 

```
[ashu@ip-172-31-29-23 ashu-work]$ kubectl  get  deploy
NAME           READY   UP-TO-DATE   AVAILABLE   AGE
ashu-mysqldb   1/1     1            1           17h
[ashu@ip-172-31-29-23 ashu-work]$ kubectl  get po 
NAME                            READY   STATUS    RESTARTS   AGE
ashu-mysqldb-769b844464-vpgzd   1/1     Running   0          16h
[ashu@ip-172-31-29-23 ashu-work]$ kubectl  get secret
NAME            TYPE                             DATA   AGE
ashu-app-db     Opaque                           3      17h
ashu-cred-reg   kubernetes.io/dockerconfigjson   1      18h
ashu-dbpass     Opaque                           1      18h
[ashu@ip-172-31-29-23 ashu-work]$ kube get  svc
NAME    TYPE        CLUSTER-IP     EXTERNAL-IP   PORT(S)    AGE
db-lb   ClusterIP   10.0.111.196   <none>        3306/TCP   17h
[ashu@ip-172-31-29-23 ashu-work]$ 

```

### Creating web app 

```
kubectl  create deployment ashu-web --image=wordpress:6.2.1-apache --port 80 --dry-run=client -o yaml >web_deploy.yaml
```

### updating manifest file to connect backend database 

```
apiVersion: apps/v1
kind: Deployment
metadata:
  creationTimestamp: null
  labels:
    app: ashu-web
  name: ashu-web
spec:
  replicas: 1
  selector:
    matchLabels:
      app: ashu-web
  strategy: {}
  template:
    metadata:
      creationTimestamp: null
      labels:
        app: ashu-web
    spec:
      containers:
      - image: wordpress:6.2.1-apache
        name: wordpress
        ports:
        - containerPort: 80
        env: # using ENV to connect Db 
        - name: WORDPRESS_DB_HOST 
          value: db-lb # service name of DB 
        - name: WORDPRESS_DB_USER
          valueFrom:
            secretKeyRef:
              name: ashu-app-db
              key: MYSQL_USER
        - name: WORDPRESS_DB_PASSWORD 
          valueFrom:
            secretKeyRef:
              name: ashu-app-db
              key:  MYSQL_PASSWORD
        resources: {}
status: {}

```

### Creating configmap to store db host detail

```
kubectl create configmap ashu-app-connect  --from-literal dbconn=db-lb  --dry-run=client -o yaml >appcm.yaml 
[ashu@ip-172-31-29-23 twotapp]$ kubectl apply -f appcm.yaml 
configmap/ashu-app-connect created
[ashu@ip-172-31-29-23 twotapp]$ kubectl  get  cm
NAME               DATA   AGE
ashu-app-connect   1      3s
kube-root-ca.crt   1      24h
```

### updating manifest

```
apiVersion: apps/v1
kind: Deployment
metadata:
  creationTimestamp: null
  labels:
    app: ashu-web
  name: ashu-web
spec:
  replicas: 1
  selector:
    matchLabels:
      app: ashu-web
  strategy: {}
  template:
    metadata:
      creationTimestamp: null
      labels:
        app: ashu-web
    spec:
      containers:
      - image: wordpress:6.2.1-apache
        name: wordpress
        ports:
        - containerPort: 80
        env: # using ENV to connect Db 
        - name: WORDPRESS_DB_HOST 
          valueFrom: # reading value of DB host from ConfigMAP 
            configMapKeyRef:
              name: ashu-app-connect
              key: dbconn
        - name: WORDPRESS_DB_USER
          valueFrom:
            secretKeyRef:
              name: ashu-app-db
              key: MYSQL_USER
        - name: WORDPRESS_DB_PASSWORD 
          valueFrom:
            secretKeyRef:
              name: ashu-app-db
              key:  MYSQL_PASSWORD
        resources: {}
status: {}

```

### deploy web app also 

```
[ashu@ip-172-31-29-23 twotapp]$ ls
appcm.yaml  dbdeploy.yaml  dbsecret.yaml  dbsvc.yaml  web_deploy.yaml
[ashu@ip-172-31-29-23 twotapp]$ kubectl apply -f   . 
configmap/ashu-app-connect configured
deployment.apps/ashu-mysqldb configured
secret/ashu-app-db configured
service/db-lb configured
deployment.apps/ashu-web created
[ashu@ip-172-31-29-23 twotapp]$ kubectl  get  deploy 
NAME           READY   UP-TO-DATE   AVAILABLE   AGE
ashu-mysqldb   1/1     1            1           17h
ashu-web       1/1     1            1           14s
[ashu@ip-172-31-29-23 twotapp]$ kubectl  get  po 
NAME                            READY   STATUS    RESTARTS   AGE
ashu-mysqldb-769b844464-vpgzd   1/1     Running   0          16h
ashu-web-6d69b75dd8-tvpgn       1/1     Running   0          19s
[ashu@ip-172-31-29-23 twotapp]$ 
```

