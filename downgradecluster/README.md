## This has all the information needed to downgrade the existing cluster.
My k8 cluster had 1 controller node and 2 client nodes.  I need to downgrade the k8 version for some applications to test. I notedown here instructions I used to perform this task successfully. 

*  Existing k8 version: Client Version: version.Info{Major:"1", Minor:"23", GitVersion:"v1.23.5", GitCommit:"c285e781331a3785a7f436042c65c5641ce8a9e9", GitTreeState:"clean", BuildDate:"2022-03-16T15:58:47Z", GoVersion:"go1.17.8", Compiler:"gc", Platform:"linux/amd64"}
Server Version: version.Info{Major:"1", Minor:"23", GitVersion:"v1.23.17", GitCommit:"953be8927218ec8067e1af2641e540238ffd7576", GitTreeState:"clean", BuildDate:"2023-02-22T13:27:46Z", GoVersion:"go1.19.6", Compiler:"gc", Platform:"linux/amd64"}


*  Downgraded version: Client Version: version.Info{Major:"1", Minor:"22", GitVersion:"v1.22.9", GitCommit:"6df4433e288edc9c40c2e344eb336f63fad45cd2", GitTreeState:"clean", BuildDate:"2022-04-13T19:57:43Z", GoVersion:"go1.16.15", Compiler:"gc", Platform:"linux/amd64"}
Server Version: version.Info{Major:"1", Minor:"22", GitVersion:"v1.22.9", GitCommit:"6df4433e288edc9c40c2e344eb336f63fad45cd2", GitTreeState:"clean", BuildDate:"2022-04-13T19:52:02Z", GoVersion:"go1.16.15", Compiler:"gc", Platform:"linux/amd64"}


* State of the kube-system before downgrading:
diego@msia-k8s-control-01:~$ kgp --namespace kube-system 
NAME                                                               READY   STATUS    RESTARTS       AGE
calico-kube-controllers-856cd9d499-jnz5s                           1/1     Running   0              14d
calico-node-6gcdd                                                  1/1     Running   1 (21h ago)    14d
calico-node-9lwdm                                                  1/1     Running   0              14d
calico-node-rpwjx                                                  0/1     Running   0              14d
coredns-64897985d-f962w                                            1/1     Running   0              14d
coredns-64897985d-r5m8x                                            1/1     Running   0              14d
etcd-msia-k8s-control-01.ads.northwestern.edu                      1/1     Running   7              14d
kube-apiserver-msia-k8s-control-01.ads.northwestern.edu            1/1     Running   0              14d
kube-controller-manager-msia-k8s-control-01.ads.northwestern.edu   1/1     Running   36 (14h ago)   14d
kube-proxy-dk4jz                                                   1/1     Running   0              14d
kube-proxy-khmz9                                                   1/1     Running   0              14d
kube-proxy-tqp9l                                                   1/1     Running   0              14d
kube-scheduler-msia-k8s-control-01.ads.northwestern.edu            1/1     Running   30 (14h ago)   14d




