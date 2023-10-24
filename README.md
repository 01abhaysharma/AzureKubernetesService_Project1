# AzureKubernetesService_Project1

# Deploying .NET core application to Azure Kubernetes Service

# Step 1: Provision Azure Kubernetes Service cluster using Azure portal

1. **Sign in to Azure Portal:**
   Sign in to the [Azure portal](https://portal.azure.com/).
2. **Click on "+ Create a resource":**
   Click the "+ Create a resource" button in the left sidebar.
3. **Search for Kubernetes Service:**
   Type "Kubernetes Service" in the search bar and select "Kubernetes Service" from the results.
4. **Click "Create":**
   Click the "Create" button to start creating a new AKS cluster.
5. **Basics Tab:**
   - **Subscription:** Choose your Azure subscription.
   - **Resource Group:** Create a new resource group or select an existing one.
   - **Kubernetes Cluster Name:** Enter a unique name for your AKS cluster.
   - **Region:** Select the region for your cluster.
   - Click "Next: Node Pools" to proceed.
6. **Node Pools Tab:**
   - **Virtual Machine Size:** Choose the size of the virtual machines for your nodes.
   - **Node Count:** Specify the number of nodes in your node pool.
   - For this handson, You see a default system node pool is already selected for creation. You may leave it as default. Click "Next: Networking" to proceed.
7. **Networking Tab:**
   - **Network Configuration:** Configure the virtual network and subnet for your AKS cluster.
   - For this handson, You may leave it as default as Azure managed. Click "Next: Integrations" to proceed.
8. **Integrations Tab:**
   - **Monitoring:** Choose whether to enable Azure Monitor for containers.
   - **Enable Azure Policy:** Choose whether to enable Azure Policy integration.
   - For this handson, You may leave it as default. Click "Next: Authentication" to proceed.
9. **Authentication Tab:**
   - **Authentication Method:** Choose between System-assigned managed identity and Service principal.
   - For this handson, You may leave it as default. Click "Next: Tags" to proceed.
10. **Tags Tab:**
    - **Add Tags (Optional):** Add any tags if needed.
    - Click "Next: Review + create" to proceed.
11. **Review + Create Tab:**
    - Review all your settings.
    - Click "Create" to create the AKS cluster.
12. **Deployment and Provisioning:**
    Azure will now deploy your AKS cluster. You can monitor the progress in the Azure portal. It takes around 15-20 mins to create cluster.
13. **Access and Manage Your AKS Cluster:**
    Once the deployment is complete, you can access and manage your AKS cluster through the Azure portal. Click on your AKS cluster resource to view details, configure settings, and connect your applications.

That's it! AKS cluster is now created.

Watch this hands-on video here: https://youtu.be/rAjS0wGuDwc

# Step 2: Create and Containerize .NET Core application

1. Launch Microsoft Visual Studio. Click on Create a new project.
2. Select the template ASP.NET Core Web Application. Click on Next.
3. Provide the name of your Project and the Location of the Project. Click on Create.
4. Select Web Application (Model-View-Controller). Check Enable Docker Support to generate a Docker File by default for the application. We can use this 
5. Docker file to build our Docker image. Click on Create.
6. The project gets created. You can verify that a Docker file gets generated as a project file.
7. Use this Docker file to containerize the application. Right-click on your project in the Solution Explorer. Click on Open Folder in File Explorer.
8. The project folder opens up in the Windows explorer. You can see the Docker file in this folder.
9. Copy this Docker file, move one folder level up to where the project solution file (.sln) is present, and paste the Docker file in that folder.
10. Now launch the PowerShell window in Administrator mode, navigate to the folder where you have the Dockerfile, and the solution (.sln) file.
11. Run the following Docker command to containerize the application. This command would create a Docker image named as akswebapp with a tag as version1.

    docker build -t akswebapp:version1 .

12. Verify the Docker image generated using the following command.

    docker image ls

13. Now run the image using the following command. In this command, all the incoming requests on the host’s port 8080 get forwarded to the Container port 80. By using the switch --detach, we run the Container in the background. We name the Container as aksapp.

    docker run --publish 8080:80 --detach --name aksapp akswebapp:version1 

14. Open the browser and navigate the following URL http://localhost:8080. You can browse the default application that is running inside the Container.
15. You cam remove the running Container using the following command. 

   docker rm --force aksapp

That's it! .Net core application has been created and containerized.

Watch this hands-on video here: https://youtu.be/yufI4K4nk34

# Step 3: Create Azure Container Registry and enable admin user

1. Open the Azure Portal and create an Azure Container Registry. Click on Create a resource.
2. Search Azure Container Registry, click on Container Registry and create.
3. Provide Resource group name. Either create new Resource group or use existing one.
4. Provide the name, Location, and SKU for the Azure Container Registry. Click on Review + Create.
5. Validate the details for the Azure Container Registry and click on Create to create the Azure Container Registry.
6. Navigate to the Azure Container Registry once it gets created. Click on the Access Keys.
7. Enable the Admin User for the Azure Container Registry so that we can push the container images to the Azure Container Registry using the Admin user and password. Click on Enable.

That's it! Azure container registry has been created and admin user has been enabled now.

Watch this hands-on video here: https://youtu.be/fLM3xz5I3wM

# Step 4: Push Containerized application to Azure Container Registry

1. Launch the PowerShell window in Administrator mode, navigate to the folder where you have the Dockerfile, and the solution (.sln) file.
2. Run the following Docker command to authenticate with the Azure Container Registry you have created to get successfully authenticated so that we can push the container image to the Azure Container Registry

docker login [Registry name].azurecr.io

3. Once you have successfully authenticated with the Azure Container Registry, you need to tag the container image with the Azure Container Registry. 

docker tag akswebapp:version1 [Registry name].azurecr.io/akswebapp:version1

4. Push the image to the Azure Container Registry.

docker push [Registry name].azurecr.io/akswebapp:version1

5. Once the push command completes successfully, go to the Azure Container Registry in the Azure Portal and verify if the container image is present. 
6. Navigate to the Azure Container Registry in the Azure Portal and click on Repositories.
7. You will see that an image Repository named as akswebapp exists.

That's it! containerized application has been pushed to Azure container regostry.

Watch this hands-on video here: https://youtu.be/T9_Ux886LBQ

# Step 5: Azure Container Registry - Repo error - Unauthorized 401 - resolved

Once You push the application image to Azure container registry, You may notice "Unauthorized error 401" in Repositories section. To resolve this issue get yourself assigned with Reader role under IAM section of Azure container registry.

Watch this hands-on video here: https://youtu.be/v7dyscQvjLY

# Step 6: Deploy to AKS Cluster using Kubectl command

**1. Create Kubernetes manifest (YAML)**

Define the desired state of the Kubernetes Cluster using a Manifest YAML file. The Manifest file defines the ReplicaSets, the number of replicas in the ReplicaSets, Service definitions, where to get the images from, and all other 
necessary information for the Cluster.

Kubernetes manifest (YAML) file with name deployment.yaml is created in the repo below: https://github.com/01abhaysharma/AzureKubernetesService_Project1/blob/main/deployment.yaml


**2. Integrate Azure Kubernetes Service with Azure Container Registry**

a. You need to be an Azure Account Administrator or Owner to be able to integrate the Azure Container Registry with Azure Kubernetes Service. 

b. Launch the PowerShell window in Administrator mode, navigate to the folder where you have the Dockerfile, and the solution (.sln) file.

c. select the subscription where you have created your Azure Kubernetes Service. Execute the following 
Azure CLI command to select your subscription.

Login: az login.
Select subscription: az account set -s "Your Azure Subscription Name" 
Validate: az account show

d. Run the following command to integrate the Azure Container Registry with the Azure Kubernetes Service. 

az aks update -n "Azure Kubernetes Service Name" -g "Azure Kubernetes Service Resource Group Name" --attach-acr "Azure Container Registry Name"

e. Your Azure Kubernetes Service gets integrated with the Azure Container Registry.


**3. Deploy the manifest file to AKS cluster using Kubectl command**

a. Execute the following command to get the Azure Kubernetes Service credentials. Getting the Azure Kubernetes Service 
credentials would help you interact with the Cluster using kubectl commands.

az aks get-credentials --resource-group "Azure Kubernetes Service Resource Group Name" --name "Azure Kubernetes Service Name"

b. Execute the following command to apply the manifest file to the Kubernetes Cluster.

kubectl apply -f deployment.yaml

c. The containerized application gets deployed to the Kubernetes Cluster.

d. Now test the containerized application that we deployed to the Azure Kubernetes Service Cluster. Execute the following command to check if the Pods are running. This command would list out all the Pods running in the Cluster.

kubectl get pods

You will see three Pods running in the Cluster as we had specified 3 Replicas in the Kubernetes manifest file. 

e. Execute the following command to get all the services running in the Kubernetes Cluster.

kubectl get services

You will see the Service named akswebapp as specified in the Kubernetes manifest file. 

f. Copy the value of the EXTERNAL-IP for the Service akswebapp.

g. Now browse the Service EXTERNAL-IP that you have copied. You will see that the application is up and running in the Azure Kubernetes Service Cluster.

That's it! Your application is now deployed to Azure Kubernetes Service Cluster.

Watch this hands-on video here: https://youtu.be/UAISDrAHwkE
