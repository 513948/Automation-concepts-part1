# Automation Concepts - Part 1
## Overview
**Students**: Geordi and Jent

This project demonstrates the deployment of a highly available and scalable .NET solution, **CloudShirt**, using AWS CloudFormation. The solution is designed to meet specific requirements for high availability, scalability, monitoring, and logging. The infrastructure is provisioned using multiple AWS services, including VPC, RDS, Auto Scaling Groups, Elastic Load Balancers, and Elastic File System (EFS).

---

## Stacks
The following CloudFormation stacks are used to deploy the environment:

| **Stack Name**   | **Description**                                      |
|-------------------|------------------------------------------------------|
| **AZStack**       | Deploys the network resources (VPC, subnets, etc.). |
| **DatabaseStack** | Deploys the database resources (Amazon RDS).        |
| **LBStack**       | Deploys the load balancer resources.                |
| **EFSStack**      | Deploys the Elastic File System (EFS) for storing web server logs. |
| **ASGStack**      | Deploys the auto scaling group resources.           |
| **ELKStack**      | Deploys the Elastic Stack (ELK) for monitoring and logging. |
| **S3Stack**       | Deploys the S3 bucket for storing exported RDS data. |

> [!IMPORTANT] 
> Ensure these stack names are used when launching the environment,
> when not using the exact same names it wil causes errors creating 
> the environment.

## Components
The following components are included in the solution:

- **Auto Scaling Group (ASG)**: Ensures the application scales dynamically based on traffic.
- **Two Availability Zones (AZs)**: Provides high availability and fault tolerance.
- **.NET Application**: The CloudShirt application is deployed and runs on EC2 instances.
- **Database**: Amazon RDS is used for the data tier. 
- **Load Balancer**: An Application Load Balancer (ALB) distributes traffic across instances.
- **Elastic File System (EFS)**: Stores web server log files.
- **Elastic Stack (ELK)**: Logs are made visible using FileBeat for monitoring and analysis.
- **S3 bucket (S3)**: RDS is getting exported by a script in the bucket.

## Deployment 
The Setup.sh script can be used to easily deploy the envirement through AWS CLI, for this AWS CLI needs to be installed and 
a token needs to be configured in the /user/"username"/.aws/credentials file. 
At the top of the script are two variables for the database username and password.
> [!IMPORTANT] 
> Ensure when creating the **DatabaseStack** that the masterpassword has
> more than 8 vars in the string, otherwise the stack will fail.

### Configuration Elastic search
When the **ELKStack** is deployed you have to wait approx. 5 min when the ELK-instance is fully configured and the Elastisch search, filebeat and kibana
services are up. 

After the 5 min you can connect to the elk-instance by typing in 'http://{PUBLIC_IP}:5601' in the web-browser, after that follow the steps as shown in the images
to add the cloudshirt data-view to monitor it.

![stack_management](https://github.com/513948/Automation-concepts-part1/blob/main/images/stack_management.png)

![data_views](https://github.com/513948/Automation-concepts-part1/blob/main/images/data_views.png)

![create_data_view](https://github.com/513948/Automation-concepts-part1/blob/main/images/create_data_view.png)

![overview](https://github.com/513948/Automation-concepts-part1/blob/main/images/overview.png)

## Requirements
The solution is designed to meet the following requirements:

- [x] **REQ-01**: The CloudShirt .NET solution is highly available across multiple AZs using a single URL.
- [x] **REQ-02**: The CloudShirt .NET solution can scale out during spike traffic hours between 6 PM and 8 PM Eastern Time.
- [x] **REQ-03**: Elastic File System (EFS) is used to store public webserver log files on a daily basis.
- [x] **REQ-04**: The data tier of the CloudShirt .NET solution is based on Amazon RDS.
- [x] **REQ-05**: Elastic (ELK) Stack v8.x is provisioned as a monitoring solution.
- [x] **REQ-06**: Logs are made visible on the Elastic Stack using FileBeat.
- [ ] **REQ-07**: RDS exports the order table to an S3 bucket using an AWS CLI script and the `bcp` utility, enabling external Athena partners to analyze it.
