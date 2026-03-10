# Installing Ingress Controller with helm
Add Ingress-Nginx repo and update it:  
```bash
helm repo add ingress-nginx https://kubernetes.github.io/ingress-nginx
helm repo update
```  

Install Ingress-Nginx app:  
```bash
helm install ingress-nginx ingress-nginx/ingress-nginx \
  --namespace ingress-nginx \
  --create-namespace \
  --set controller.hostNetwork=true \
  --set controller.reportNodeIP=true
```  

Set this ingress controller as the default router of the Cluster:
```bash
kubectl patch ingressclass nginx -p '{"metadata": {"annotations":{"ingressclass.kubernetes.io/is-default-class":"true"}}}'
```  