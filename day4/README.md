### revision 

<img src="rev.png">

### Understanding docker image build cpu depedencies 

<img src="cpu.png">

### Creating ns and configure it 

```
[ashu@ip-172-31-29-23 ashu-work]$ kubectl  create  ns  ashu-apps
namespace/ashu-apps created
[ashu@ip-172-31-29-23 ashu-work]$ kubectl config set-context --current --namespace  ashu-apps
Context "oracle_training" modified.
[ashu@ip-172-31-29-23 ashu-work]$ 
[ashu@ip-172-31-29-23 ashu-work]$ 
[ashu@ip-172-31-29-23 ashu-work]$ kubectl config get-contexts 
CURRENT   NAME              CLUSTER           AUTHINFO                            NAMESPACE
*         oracle_training   oracle_training   clusterUser_k8sty_oracle_training   ashu-apps
[ashu@ip-172-31-29-23 ashu-work]$ 
[ashu@ip-172-31-29-23 ashu-work]$ kubectl  get pods
No resources found in ashu-apps namespace.
[ashu@ip-172-31-29-23 ashu-work]$ 

```
