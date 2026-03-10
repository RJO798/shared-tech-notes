# Helm
Helm is a package manager for Kubernetes that simplifies the deployment and management of **complete applications** in a cluster.  
Instead of manually creating and applying multiple Kubernetes YAML files, Helm allows you to package them into a single reusable unit called a **Chart**.  

A Helm Chart contains:  
- Kubernetes resource templates (Deployments, Services, etc.)
- Default configuration values
- Metadata about the application  

**Helm charts** are commonly shared through **Helm repositories**, which are similar to package repositories such as "apt".  

Using Helm, you can easily:  
- Install applications in a cluster  
- Upgrade or rollback application versions  
- Customize deployments using configuration values  


## Example of use  
List all available Helm repositories:
```bash
helm repo list
```  
Add a repo
```bash
helm repo add bitnami https://charts.bitnami.com/bitnami
```  
Install a package
```bash
helm repo update              # Make sure we get the latest list of charts
helm install happy-panda bitnami/wordpress
```  

List all your Helm apps:  
```bash
helm ls -A
```  

If you want to uninstall it:
```bash
helm uninstall happy-panda
```  

## Installation
Official docu: *https://helm.sh/docs/intro/install/*  

### From script
```bash
curl -fsSL -o get_helm.sh https://raw.githubusercontent.com/helm/helm/main/scripts/get-helm-4
chmod 700 get_helm.sh
./get_helm.sh
```  


### From snap
```bash
sudo snap install helm --classic
```  

### APT-based systems
```bash
sudo apt-get install curl gpg apt-transport-https --yes
curl -fsSL https://packages.buildkite.com/helm-linux/helm-debian/gpgkey | gpg --dearmor | sudo tee /usr/share/keyrings/helm.gpg > /dev/null
echo "deb [signed-by=/usr/share/keyrings/helm.gpg] https://packages.buildkite.com/helm-linux/helm-debian/any/ any main" | sudo tee /etc/apt/sources.list.d/helm-stable-debian.list
sudo apt-get update
sudo apt-get install helm
```  