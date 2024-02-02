
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

