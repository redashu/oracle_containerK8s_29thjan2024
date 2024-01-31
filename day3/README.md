# oracle_containerK8s_29thjan2024

### Revision 

<img src="rev.png">

### Understanding rancher desktop offering 

<img src="rancher.png">

### Problems with Container runtime engine 

<img src="prob1.png">

### Introduction to container orchestration engine 

<img src="corch.png">

### More info aout k8s 

<img src="k8s.png">

### k8s arch 1 

<img src="k8s1.png">

### k8s arch 2 

<img src="k8s2.png">

### k8s arch 3

<img src="k8s3.png">

### checking kubectl connection in case of rancher desktop 

```
PS C:\Users\hp> kubectl version  -o json
{
  "clientVersion": {
    "major": "1",
    "minor": "27",
    "gitVersion": "v1.27.1",
    "gitCommit": "4c9411232e10168d7b050c49a1b59f6df9d7ea4b",
    "gitTreeState": "clean",
    "buildDate": "2023-04-14T13:21:19Z",
    "goVersion": "go1.20.3",
    "compiler": "gc",
    "platform": "windows/amd64"
  },
  "kustomizeVersion": "v5.0.1",
  "serverVersion": {
    "major": "1",
    "minor": "26",
    "gitVersion": "v1.26.3+k3s1",
    "gitCommit": "01ea3ff27be0b04f945179171cec5a8e11a14f7b",
    "gitTreeState": "clean",
    "buildDate": "2023-03-27T22:23:17Z",
    "goVersion": "go1.19.7",
    "compiler": "gc",
    "platform": "linux/amd64"
  }
}
PS C:\Users\hp> kubectl version  -o yaml


```

### checking number of nodes 

```
PS C:\Users\hp> kubectl  get  nodes
NAME              STATUS   ROLES                  AGE    VERSION
desktop-52f5653   Ready    control-plane,master   287d   v1.26.3+k3s1
PS C:\Users\hp>

```

### api_server is getting info from ETcd 

<img src="etcd.png">


