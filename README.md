# aks-ingress-controller
How to deploy nginx-ingress controller in a private Azure Kubernetes & Private ACR.  

# Pre-requisites
1. VNET with 2 subnets (Enable Service Endpoint)
2. Private Azure Container Registry (networking blade should be on Disabled)
3. Private AKS with RBAC enabled (Attach AKS cluster to registry)
4. Linux Jumpbox with Azure CLI, Docker, Helm (v 3.10.0), Kubernetes CLI installed


# Installing Ingress Controller (Step-by-Step)
1. From Linux VM, authenticate with your registry and run az acr import to import all necessary images/repositories into the container registry (make sure you have Azure CLI, Docker, Helm (v 3.10.0), Kubernetes CLI installed )
2. Download desired package for ingress-nginx (Im using version 3.36.0)
3. Extract .tgz file and run helm push to acr (refer to manual.sh steps for commands)
4. Authenticate with your ACR and make sure helm pull runs successfully before running helm upgrade command
5. Create AKS namespace for ingress controller
6. Create internal-ingress.yaml file to create ingress controller and specify an ip that is not been used inside your vnet (use the internal-ingress.yaml inside ingress folder on this repo for reference)
7. Run helm upgrade (Refer to manual.sh for commands)
8. Run demo applications (aks-helloworld.yaml and ingress-demo.yaml) by running kubectl apply from linux vm
9. Run kubectl get validatingwebhookconfigurations and Delete it (kubectl delete validatingwebhookconfigurations nameoftheWebhook)
10. Create an ingress route (deploy hello-world-ingress.yaml) 
11. Validate and test ingress controller by running the following: 
    <br>a. kubectl run -it --rm aks-ingress-test --image=mcr.microsoft.com/aks/fundamental/base-ubuntu:v0.0.11 -n yournamespace <br>
    b. apt-get update && apt-get install -y curl<br>
    c. curl -L http://youripgivenfortheinternalloadbalancer and curl -L -k http://yourip/hello-world-two <br>
    d. You can also open a browser in a vm with access to this vnet and you should see your ingress working <br>
    
    
# Links
1. https://learn.microsoft.com/en-us/azure/aks/ingress-basic?tabs=azure-cli
2. https://devopscube.com/setup-ingress-kubernetes-nginx-controller/
3. https://www.youtube.com/watch?v=72zYxSxifpM&list=PLHq1uqvAteVvUEdqaBeMK2awVThNujwMd&index=21&t=1147s
