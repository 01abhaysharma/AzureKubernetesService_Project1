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
11. Run the following Docker command to containerize the application. This command would create a Docker image named as mydemoapp with a tag as version1.

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

# Step 3: Create Azure Container Registery and enable admin user

Watch this hands-on video here: https://youtu.be/fLM3xz5I3wM

# Step 4: Push Containerized application to Azure Container Registry

Watch this hands-on video here: https://youtu.be/T9_Ux886LBQ

# Step 5: Azure Container Registry - Repo error - Unauthorized 401 - resolved.

Watch this hands-on video here: https://youtu.be/v7dyscQvjLY

# Step 6: Deploy to AKS Cluster using Kubectl command

1. Create Kubernetes manifest (YAML)
2. Integrate Azure Kubernetes Service with Azure Container Registry
3. Deploy the manifest file to AKS cluster using Kubectl command
   
Watch this hands-on video here: https://youtu.be/UAISDrAHwkE


