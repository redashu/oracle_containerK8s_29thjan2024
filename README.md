### Plan 

<img src="plan.png">

## tech stack 

<img src="tech.png">

### isntalling docker compose with aarch64 process

```
curl -L https://github.com/docker/compose/releases/download/v2.24.3/docker-compose-linux-aarch64 -o /usr/bin/docker-compose

chmod +x /usr/bin/docker-compose
docker-compose  version 
Docker Compose version v2.24.3
[root@docker-server yum.repos.d]# 

```

### HPA in k8s 

<img src="hpa1.png">

### HPA need 

<img src="hpa2.png">

### implementing HPA 

```
ashu@ip-172-31-29-23 k8s-manifest]$ kubectl  get  deploy
NAME          READY   UP-TO-DATE   AVAILABLE   AGE
ashu-hpaweb   1/1     1            1           2m9s
[ashu@ip-172-31-29-23 k8s-manifest]$ 
[ashu@ip-172-31-29-23 k8s-manifest]$ kubectl autoscale deployment ashu-hpaweb --cpu-percent 70 --min 2 --max 15  --dry-run=client -o yaml >hparule.yaml 
[ashu@ip-172-31-29-23 k8s-manifest]$ kubectl  get po 
NAME                          READY   STATUS    RESTARTS   AGE
ashu-hpaweb-569d754b5-4cg9f   1/1     Running   0          3m58s
[ashu@ip-172-31-29-23 k8s-manifest]$ 
[ashu@ip-172-31-29-23 k8s-manifest]$ 
[ashu@ip-172-31-29-23 k8s-manifest]$ kubectl create -f hparule.yaml 
horizontalpodautoscaler.autoscaling/ashu-hpaweb created
[ashu@ip-172-31-29-23 k8s-manifest]$ 
[ashu@ip-172-31-29-23 k8s-manifest]$ kubectl get hpa
NAME          REFERENCE                TARGETS         MINPODS   MAXPODS   REPLICAS   AGE
ashu-hpaweb   Deployment/ashu-hpaweb   <unknown>/70%   2         15        0          4s
[ashu@ip-172-31-29-23 k8s-manifest]$ kubectl  get  po 
NAME                          READY   STATUS    RESTARTS   AGE
ashu-hpaweb-569d754b5-4cg9f   1/1     Running   0          4m17s
```
