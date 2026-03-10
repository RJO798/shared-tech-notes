# Installing StorageClass with helm
Add OpenEBS repo and update it:  
```bash
helm repo add openebs https://openebs.github.io/charts
helm repo update
```  
Install OpenEBS app:  
```bash
helm install openebs openebs/openebs --namespace openebs --create-namespace
``` 

Set OpenEBS as the default disk of the cluster:  
```bash
kubectl patch storageclass openebs-hostpath -p '{"metadata": {"annotations":{"storageclass.kubernetes.io/is-default-class":"true"}}}'
```  