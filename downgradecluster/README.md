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

* Loign to the client node using: ssh user@IP
* sudo kubeadm reset
* sudo apt-get purge kubeadm kubelet kubectl
* sudo apt-get update
* If there are errors related to <img width="1379" alt="image" src="https://github.com/ramankhurana/kubernetes-scripts/assets/4996609/b7b332e2-bfbc-4103-937b-c7450b78f6b3">
  * sudo emacs -nw  /etc/apt/sources.list # search for "deb [arch=amd64] https://download.docker.com/linux/ubuntu focal stable" and comment it as it is present in docker.list
  * sudo apt-get update

* If the errors are related to: <img width="1510" alt="image" src="https://github.com/ramankhurana/kubernetes-scripts/assets/4996609/9a7b30e8-9540-4a81-9d5c-2ce7fa4bb052">
  * sudo apt clean; sudo rm /etc/apt/sources.list.d/kubernetes.list; curl -s https://packages.cloud.google.com/apt/doc/apt-key.gpg | sudo apt-key add -; echo "deb https://apt.kubernetes.io/ kubernetes-xenial main" | sudo tee -a /etc/apt/sources.list.d/kubernetes.list; sudo apt-get update

 
* sudo apt-get install -y kubelet=1.22.9-00 kubeadm=1.22.9-00 kubectl=1.22.9-00
* On the clinet node execute the join command:
   * You will get the command by executing the sudo kubeadm init on the controller
   * It will be of the form sudo kubeadm join 10.110.1.17:6443 --token w4r39i.5e2v2f5lx1i1v75d \
        --discovery-token-ca-cert-hash sha256:1e6b78e9ba38624b36b6ad55d1c7f995ef80094b8f0c7802cc5fcc1bc2cdf4cb
   

* If Calico is not working, delete the pod --> not working
   * check if it is uncordon, possible taints etc?
   * kubectl taint nodes my-node node-role.kubernetes.io/master:NoSchedule-  (--> not working)
       *  Restart daemonset for calico
       *  kubectl delete daemonset calico-node -n kube-system
       *  kubectl cordon msia-k8s-control-01.ads.northwestern.edu ## this might not be needed always 
       *  kubectl apply -f https://docs.projectcalico.org/manifests/calico.yaml
    
   * Calico yaml can ge downloaded from
      * wget https://docs.projectcalico.org/v3.17/manifests/calico.yaml
      * kubectl apply -f calico.yaml
    
 * If coredns is not working
    * restart coredns (check taints/no-scheduling on master nodes)


* On controller you might need to setup the config: [for one time]
* export KUBECONFIG=/etc/kubernetes/admin.conf
* sudo chown $(id -u):$(id -g) /etc/kubernetes/admin.conf
   * For permanent solution:
   * sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config   ## overwrite (yes)
   * sudo chown $(id -u):$(id -g) $HOME/.kube/config

## Repeat the steps above on all the clinet nodes 
 






