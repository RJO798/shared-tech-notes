# kube-vip
*Official docu: https://kube-vip.io/docs/*  

kube-vip will intercept LoadBalancer type requests and will publish our VIP in the local network using ARP protocol. It is necessary to access the cluster from the outside (targeting the VIP) in HA cases.  
kube-vip ensures the target IP (the VIP) will always be the same.   


## kube-vip installation
### Create the RBAC settings
kube-vip as a DaemonSet needs the correct access to be able to watch Kubernetes Services and other objects. In order to do this, RBAC resources must be created.  
```bash
kubectl apply -f https://kube-vip.io/manifests/rbac.yaml
```  

### Creating the DaemonSet
Run the next commands in the **controlplane** to generate the DaemonSet.  

Set your desired VIP.
> [!IMPORTANT]  
> This VIP must be an available/unused IP in your network. And this network must be the external one. Do NOT use the primary IP of any of your nodes.  

```bash
export VIP=10.3.4.100
```  

Set the network interface name on your controlplane which will announce the VIP:  
```bash
export INTERFACE=eth0
```  

Get the latest version of the kube-vip release:  
```bash
KVVERSION=$(curl -sL https://api.github.com/repos/kube-vip/kube-vip/releases | jq -r ".[0].name")
```  

Pull the image using containerd:  
```bash
alias kube-vip="sudo ctr image pull ghcr.io/kube-vip/kube-vip:$KVVERSION; sudo ctr run --rm --net-host ghcr.io/kube-vip/kube-vip:$KVVERSION vip /kube-vip"
```  

Create the kube-vip installation manifest as a DaemonSet and redirect it to a YAML definition file:  
```bash
kube-vip manifest daemonset \
    --interface $INTERFACE \
    --address $VIP \
    --inCluster \
    --taint \
    --controlplane \
    --services \
    --arp \
    --leaderElection | tee kube-vip.yaml
# By using the --controlplane flag, this DaemonSet will only schedule pods on the controlplane nodes.
```  
> [!NOTE]  
> This architecture ensures kube-vip pods will ONLY be deployed in the controlplane nodes, as they are the only ones that have an attached network interface that can be seen from outside the cluster.  

Then, you can deploy the DaemonSet:  
```bash
kubectl apply -f kube-vip.yaml
```  

Check if your kube-vip pod is in Running state:  
```bash
k get pods -n kube-system
```  