# Project Title
Preparation, Docker Image Push and Deployment for Containerized Voting Application in Kubernetes Cluster using Docker, Azure Container Registry (ACR) and Azure Kubernetes Service (AKS)

# Project Description

In another project based on a real world scenario, I had to act as a DevOps Engineer, and show a new team member how to deploy an application on a Kubernetes cluster.

This cluster is part of The Cloud Bootcamp project, and I prepared this new team member to deploy the voting application that was developed for the MultiCloud Experience, an online event where participants had the opportunity to learn about Cloud technologies.

I deployed it to Microsoft Azure cloud, where I first pushed the application's Docker image to Azure Container Registry (ACR) and then used the Azure Kubernetes Service (AKS) service to deploy a cluster managed by Microsoft Azure.

# Video Demonstration: 
[![Watch this YouTube video](https://img.youtube.com/vi/9HficAUqVBM/0.jpg)](https://www.youtube.com/watch?v=9HficAUqVBM)

Steps to Deploy the Application
1. Open CloudShell

```bash
wget https://tcb-bootcamps.s3.amazonaws.com/bootcamp-microsoft-azure/mod3/tcb-voting-app.zip
unzip tcb-voting-app.zip
```

2. Create Azure Container Registry (ACR)

```bash
az group create --name tcb-vote --location eastus  # Resource Group
az acr create --resource-group tcb-vote --name tcbjay --sku Basic  # Create ACR
```
![Image](https://i.imgur.com/mdalcXQ.png)

3. Login to ACR

```bash
az acr login -n tcbjay --expose-token  # Show token
docker login tcbjay.azurecr.io -u 00000000-0000-0000-0000-000000000000 -p <TOKEN>
```
![I](https://i.imgur.com/6B0oG6S.png)

4. Build and Push Docker Image

```bash
az acr build --image tcb-vote --registry tcbjay --file Dockerfile .  # Push to ACR
```

5. Create Kubernetes Cluster

```bash
az aks create --resource-group tcb-vote --name AKSClusterTCB --node-count 1 --generate-ssh-keys  # Create cluster
az aks update -n AKSClusterTCB -g tcb-vote --attach-acr tcbjay  # Attach ACR to cluster
```

6. Get Credentials and Deploy App

```bash
az aks get-credentials --resource-group tcb-vote --name AKSClusterTCB
kubectl get nodes  # Check nodes
cd ~/tcb-voting-app
vi tcb-vote-plus-redis.yaml  # Update image path
kubectl apply -f tcb-vote-plus-redis.yaml  # Deploy app
kubectl get service --watch  # Get external IP
```
In this case when editing the .yaml file, the image is changed from 'thecloudbootcamp/tcb=vote:latest' to 'tcbjay.azurecr.io/tcb-vote:latest', the login server of the ACR

From: ![I](https://i.imgur.com/G1tMoTH.png) To: ![I](https://i.imgur.com/XrSKr0t.png)

Gathering the IP of the application:
![I](https://i.imgur.com/M6No8BB.png)

7. Access the App

- Check the external IP in your web browser.
- The counter is being stored in Redis within the Kubernetes cluster.

Accessing the App:
![I](https://i.imgur.com/AxNuBuD.png)

What I Learned

- **Containerization**: Building and pushing Docker images to Azure Container Registry (ACR).
- **Kubernetes**: Creating and managing a Kubernetes cluster using Azure Kubernetes Service (AKS).
- **DevOps Practices**: Implementing a real-world scenario to train a new team member on deploying applications in a cloud environment.
- **Azure Cloud Services**: Utilizing various Azure services such as ACR and AKS to deploy and manage containerized applications.

Conclusion

This project was a part of The Cloud Bootcamp and the MultiCloud Experience, designed to provide hands-on experience with cloud technologies. It was an excellent opportunity to deepen my understanding of deploying applications in a cloud-native environment.

Feel free to explore the repository and reach out if you have any questions or need further insights!

---
**Tags**: DevOps, Kubernetes, Docker, Azure, ACR, AKS, Cloud Bootcamp, MultiCloud Experience, Cloud Computing
