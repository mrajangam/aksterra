https://developer.hashicorp.com/terraform/tutorials/kubernetes/aks
Terraform file kept in : C:\Users\prave\Documents\azure\aksexercise\learn-terraform-provision-aks-cluster"
Manifest files kept in : C:\Users\prave\Documents\azure\aksexercise\

Stage I : Build AKS Cluster

Step 1: login to azure 
  " az login"

Step 2: Change to the directory where the "Terraform form (Hashi Corp) file kept. In our case it is palced in "C:\Users\prave\Documents\azure\aksexercise\learn-terraform-provision-aks-cluster"

Step 3: Create service principal
 " az ad sp create-for-rbac --skip-assignment"

Step 4: Copy the output from Step 3 (similar to below showed) 

  "appId": "Tobe filled",
  "displayName": "Tobe filled",
  "password": "Tobe filled",
  "tenant": "Tobe filled"

Step 5 : Update "terraform.tfvars" (kept in the learn-terraform-provision-ask-cluster) with appId and Password 

Step 6 : Initialize Terraform
   "terraform init"

Step 7 : Provision the AKS Cluter
   " terraform apply"

Step 8 :
kubernetes_cluster_name = "integral-polliwog-aks"
resource_group_name = "integral-polliwog-rg"
-----------------------------------------------------------------------------------------------------------
Stage II Associate "Azure Container Registry with AKS Cluster (Just build)
  - Pre-requisite "Application Image in Azure registry". I have created a nginx image (nginx:v1) and kept it in my Azure registry (acr2024demo).

Login the Azure portal and validate the AKS Cluster status. Also take down the 
 - AKS Kubernetes service Name" and Azure Kubernetes Service Resource Group Name"

In our case it is :
  kubernetes_cluster_name = "integral-polliwog-aks"
  resource_group_name = "integral-polliwog-rg"

To associate the ACR with AKS run below command

  - az aks update -n "Azure Kubernetes Service Name" -g "Azure Kubernetes Service Resource Group Named" --attach-acr "Azure Container Registry Name"

   "az aks update -n "integral-polliwog-aks" -g "integral-polliwog-rg" --attach-acr "acr2024demo"

---------------------------------------------------------------------------------------------------------

Stage III : Retrieve the AKS Credential using the below command & Validate the AKS using "kubectl" commands.

az aks get-credentials --resource-group "integral-polliwog-rg" --name "integral-polliwog-aks"

2. kubectl get nodes

----------------------------------------------------------------------------------------------------------

Stage IV : Run containerized (nginx) application in this cluster
Note : The "Manifest files such as "deployment" , "Services" kept in the folder "C:\Users\prave\Documents\azure\aksexercise"

Step 1 : Change to the folder "deployment.yaml" & "service.yaml"and use kubectl command to apply

 - kubectl apply -f deployment.yaml
 - kubectl apply -f service.yaml

Validate using below listed command:
 - kubectl get pods  
  output similar to below will be showed"

"C:\Users\prave\Documents\azure\aksexercise>kubectl get pods
NAME                         READY   STATUS    RESTARTS   AGE
mydemoapp-6f77665f4f-qpxr7   1/1     Running   0          43s
mydemoapp-6f77665f4f-skd57   1/1     Running   0          43s
mydemoapp-6f77665f4f-smdd4   1/1     Running   0          43s


 - kubectl get svc

 output similar to below will be showed. You may need to wait for a while to get the svc external IP. In this example it is "51.8.27.35:8080". Use this ip and validate in the edge browser. nginix defult screen will be there.
