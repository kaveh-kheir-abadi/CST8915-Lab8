# CST8915 Lab 8 – Algonquin Pet Store on AKS

**Student:** Kaveh Kheir Abadi  
**Student ID:** 041297932  
**Course:** CST8915 – Full-stack Cloud-native Development  
**Semester:** Fall 2026

## Video Demonstration

[Watch Lab 8 Demonstration on YouTube](https://youtu.be/Y4yxj_9k_iI)

## Lab Overview

In this lab, I deployed the Algonquin Pet Store application on Azure Kubernetes Service (AKS). The application includes multiple microservices, MongoDB, RabbitMQ, virtual customers and workers, and AI-powered product description and image generation using OpenAI.

## Task 1 – Build and Deploy Custom Docker Images

I forked all eight repositories provided by the instructor, cloned them to my local machine, built Docker images, and pushed them to my Docker Hub account.

### GitHub Repositories and Docker Images

| GitHub Repository | Docker Image |
|---|---|
| [store-front-L8](https://github.com/kaveh-kheir-abadi/store-front-L8) | kavehkhe/store-front-l8:v1 |
| [store-admin-L8](https://github.com/kaveh-kheir-abadi/store-admin-L8) | kavehkhe/store-admin-l8:v1 |
| [order-service-L8](https://github.com/kaveh-kheir-abadi/order-service-L8) | kavehkhe/order-service-l8:v1 |
| [product-service-L8](https://github.com/kaveh-kheir-abadi/product-service-L8) | kavehkhe/product-service-l8:v1 |
| [makeline-service-L8](https://github.com/kaveh-kheir-abadi/makeline-service-L8) | kavehkhe/makeline-service-l8:v1 |
| [ai-service-L8](https://github.com/kaveh-kheir-abadi/ai-service-L8) | kavehkhe/ai-service-l8:v1 |
| [virtual-customer-L8](https://github.com/kaveh-kheir-abadi/virtual-customer-L8) | kavehkhe/virtual-customer-l8:v1 |
| [virtual-worker-L8](https://github.com/kaveh-kheir-abadi/virtual-worker-L8) | kavehkhe/virtual-worker-l8:v1 |

**Docker Hub:** [kavehkhe](https://hub.docker.com/u/kavehkhe)

### Updated Kubernetes Deployment Files

I updated the following Kubernetes YAML files to use my own Docker images:

- [aps-all-in-one-Task1.yaml](./aps-all-in-one-Task1.yaml)
- [admin-tasks.yaml](./admin-tasks.yaml)

MongoDB and RabbitMQ continued to use their original container images.

### Deployment Commands

```bash
kubectl apply -f aps-all-in-one-Task1.yaml
kubectl apply -f admin-tasks.yaml
```

### Deployment Verification

```bash
kubectl get nodes
kubectl get pods
kubectl get services
```

```bash
kubectl get deployments -o custom-columns='NAME:.metadata.name,IMAGE:.spec.template.spec.containers[*].image,READY:.status.readyReplicas'
```

### Testing Results

The following components were successfully tested:

- Both AKS worker nodes were ready.
- All ten pods were running successfully.
- All eight application deployments used my own Docker images.
- Store Front and Store Admin were accessible through public IP addresses.
- Customer orders were successfully submitted.
- Virtual Customer generated orders automatically.
- Virtual Worker successfully processed orders.
- AI-generated product descriptions and images worked successfully.
- New products were successfully added to the store.
- RabbitMQ and MongoDB were explored during the initial deployment.
- Order Service was successfully scaled to three replicas during the initial deployment.

### Challenges

I initially attempted to use Azure OpenAI. However, subscription restrictions and insufficient image-generation model quota prevented me from completing the Azure OpenAI configuration.

I used the direct OpenAI integration provided by the instructor instead, which allowed both text and image generation to work successfully.

## Task 2 – Azure Managed Services

### 1. Azure Cosmos DB for MongoDB

**Purpose:**

Azure Cosmos DB for MongoDB is a fully managed NoSQL database service that supports MongoDB-compatible applications.

**Why is it a suitable replacement?**

In this lab, MongoDB runs inside the AKS cluster and stores order information. Azure Cosmos DB for MongoDB can provide similar document-based storage without requiring us to manage a database inside Kubernetes.

**Benefits:**

- Automated backups and database maintenance.
- Automatic scaling options.
- High availability and global distribution.
- Reduced infrastructure management.
- Compatibility with MongoDB drivers.

The application would require an updated database connection string and compatibility testing.

### 2. Azure Service Bus

**Purpose:**

Azure Service Bus is a fully managed message broker that supports asynchronous communication through message queues and publish-subscribe topics.

**Why is it a suitable replacement?**

In this lab, RabbitMQ handles order messages between microservices. Azure Service Bus can provide similar asynchronous communication without requiring us to deploy and maintain a message broker inside AKS.

**Benefits:**

- Reliable message delivery.
- Reduced infrastructure maintenance.
- Improved scalability and availability.
- Dead-letter queues for failed messages.
- Duplicate message detection.
- Integration with Azure security services.

Azure Service Bus is not a direct replacement for RabbitMQ. Changes to the messaging code and application configuration would be required.

### Conclusion

Using managed Azure services would reduce infrastructure maintenance, improve reliability and scalability, and allow developers to focus more on application development.

These services were researched as potential replacements and were not deployed as part of this lab.

### References

- [Microsoft Learn – Azure Cosmos DB for MongoDB](https://learn.microsoft.com/en-us/azure/cosmos-db/mongodb-introduction)
- [Microsoft Learn – Azure Service Bus](https://learn.microsoft.com/en-us/azure/service-bus-messaging/service-bus-messaging-overview)

## Resource Cleanup

After completing the lab, testing the application, and recording the demonstration video, I deleted the Azure resources to avoid unnecessary charges.

API keys and secret files are not included in this repository for security reasons.

## Lab Status

**Completed Successfully.**
