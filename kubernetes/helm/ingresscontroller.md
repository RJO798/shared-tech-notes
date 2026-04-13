# Ingress Controller
The Ingress Controller is in charge of distributing the external traffic to its destiny inside the cluster.  

## Installing an Ingress Controller with helm
Add the Ingress-NGINX repo and update it:  
```bash
helm repo add ingress-nginx https://kubernetes.github.io/ingress-nginx
helm repo update
```  

Set the IP you will use to access the cluster from an external network (If you didn't set it before):
```bash
export VIP='<YOUR_EXTERNAL_IP>'
```

Install the Ingress-NGINX chart (optimized for bare-metal):  
```bash
helm install ingress-nginx ingress-nginx/ingress-nginx \
  --namespace ingress-nginx \
  --create-namespace \
  --set controller.service.externalTrafficPolicy=Cluster \
  --set controller.service.type=LoadBalancer \
  --set controller.service.annotations."kube-vip\.io/loadbalancerIPs"=$VIP \
  --set controller.ingressClassResource.default=true \
  --set controller.replicaCount=1
# For Production HA: Set replicaCount to match your number of worker nodes that you want to run the IngressController.
# It is recommended to set it to 2, at least.
```  
> [!NOTE]  
> This configuration will work if you have deployed a LoadBalancer provider such as **kube-vip**.  