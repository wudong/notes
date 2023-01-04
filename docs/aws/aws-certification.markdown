---
title:  "AWS Solution Architect Certificate Exam"
date:   2023-01-03 22:35:46 +0000
tags: aws certificate
parent: aws
---


# Compute 

## EC2
### AMI: 
- An AMI is an **image baseline** with an operating system and applications along with any **custom configuration**
- AWS Marketplace: online store that allows you to purchase AMI from trusted vendros.
- Cumminit AMIs: created and shared by other AWS members.

### EC2 Instance Types
- ECUs: EC2 Compute Units for the instance
- AES-NI: Advenced Encryption Standard - New Instructions, needed for enhanced data protection.
- AVX: for application focus on a/v, scientific calc, 3D modeling, etc.
- Mostly we focus on
  - vCPUs
  - Memory
  - Instance Storage (EBS)
  - Network Performance

### Instance Purchasing Options
- On-Demand: flat rate determined on the instance type, charged by second, typically used for *short-term* uses, 
  workload cannot be interrupted.
- Reserved: saving as much as 75%, 1 or 3 years.
  - All upfront: complete payment for 1 or 3 years
  - Partial Upfront
  - No Upfront
- Scheduled: pay for the reservations on a recurring schedule, either daily, weekly, or monthly.
  - changed even if it is not used.
- Spot: bid for an unused EC2 compute resources, can be interrupted. 
  - When the big price is lower than the current spot price, the instance is terminated with a 2-minute grace period.
  - Suitable for batch process or background processing of data. 
- On-Demand Capacity Reservations: 
  - Reserved capacity for a specific instance type in a specific Availability Zone for any period of time.
  - Ensure you have the capacity you need immediately.
  - Can be shared with reserved instances discount.

### EC2 Tenancy
What underlying host you EC2 instance will reside on:
- shared tenancy
- dedicated instances: 
  - hosted on hardware that no other customer can access, with additional charges.
  - hardware can be shared by other resources in your own account.
- dedicated hosts: 
  - same as dedicated instances, with additional visilbity and control on the physical host.
  - use with other licenses that require a physical server.
  - same host can be used for multiple instances.

### User Data
User data allows you to enter commands that will run during the first boot cycle of that instance.

### Storage Options
- Persistent Storage: EBS, not physically attached, but like NAS. EBS is replicated within AZ. 
- Ephemeral Storage: 
  - physically attached, all data on disk is lost when the instance is terminated.
  - reboot however remain the data. 

### Security 
- Security group is an instance level firewall that controls the traffic in and out.
- Key pair is used for encrypting the login credentials and remote SSH.
- It is your responsibility to maintain and install the latest OS and security patches.

##  EC2 Container Service (ECS)
- Run docker-enabled applications as containers across a *cluster of EC2 instances*.
- The burden of managing your own cluster is with AWS, through the use of AWS Fargate.
- **AWS Fargate** is an engine used to enable ECS to run containers without having to 
  manage and provision the underlying EC2 instances.
- Two ways of launching a ECS cluster:
  - Fargate launch: less configuration.
  - EC2 launch: more control over the underlying EC2 instances.
- Monitoring is done through Amazon CloudWatch.
- EC2 instances within the cluster still operate in much the same way os a single EC2 instance.
  - Access
  - Security group, Elastic Load Balancer, etc. 
- ECS is a regional service, it can scale across multiple AZs within a region, but not cross region.
- Instances within the cluster also have a docker daemon and an ECS agent installed.

## Elastic Container Registry (ECR)
- Fully managed Docker container registry.
- Access is controlled via IAM policies in addition to repository policies.
- Before your docker client can access your registry, it needs to be authenticated as an AWS user via 
  an Authorization token.
  ```shell
    aws ecr get-login-password --region us-east-1 --no-include-email \ 
        | docker login --username AWS --password-stdin 123456789012.dkr.ecr.us-east-1.amazonaws.com    
  ```
  Token can be used for 12 hours.
- Repository are objects within your registry that allow you to group together and secure different docker images.

## Elastic Container Service for Kubernetes (EKS)
- AWS maintains the k8s control plane, you manage the worker nodes.
- Node runs with a specific AMI, to ensures Docker and kubelet is installed.
- Once the worker nodes are up and running, they can connect to EKS using an endpoint.

## Elastic Beanstalk
- An AWS managed service that take code and automatically provisions and deploy the required
  resources within AWS.
- You can continue to support and maintain the environment as you would with a custom-built environment.
- You can perform maintenance tasks from the Elastic Beanstalk dashboard.
- Elastic Beanstalk is free to use, just need to pay for the resources it provisioned. 

