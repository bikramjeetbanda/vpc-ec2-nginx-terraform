# AWS VPC + EC2 + NGINX using Terraform

This project provisions a fully functional AWS infrastructure using **Terraform**, including:

* A custom VPC with **public and private subnets** across multiple availability zones
* Proper routing via **Internet Gateway** and route tables
* An EC2 instance running **NGINX** in a public subnet
* Modular, reusable Terraform code with clear input variables
* Public HTTP access to the instance on **port 80**
* Zero use of third-party/community modules

---

## Assignment Requirements & Implementation

| Requirement                                           | Implemented |
| ----------------------------------------------------- | ----------- |
| Use Terraform                                         | Yes         |
| VPC with CIDR block `10.0.0.0/16`                     | Yes         |
| 3 Public subnets across different availability zones  | Yes         |
| 3 Private subnets across different availability zones | Yes         |
| Internet access for public subnets only               | Yes         |
| No direct internet for private subnets                | Yes         |
| EC2 instance running NGINX on port 80                 | Yes         |
| HTTP access to EC2 by public IP                       | Yes         |
| Use modules as appropriate                            | Yes         |
| No third-party or community modules                   | Yes         |
| Parameterized and reusable code                       | Yes         |
| No comments in the code                               | Yes         |

---

## Project Structure

```
terraform-vpc-nginx/
│
├── main.tf                  # Root configuration to call modules
├── variables.tf             # Input variables for the root module
├── outputs.tf               # Outputs like public IP
├── terraform.tfvars         # Default values for variables
│
├── modules/
│   ├── vpc/                 # VPC module
│   │   ├── main.tf
│   │   ├── variables.tf
│   │   └── outputs.tf
│   │
│   └── ec2/                 # EC2 + NGINX module
│       ├── main.tf
│       ├── variables.tf
│       └── outputs.tf
```

---

## Infrastructure Overview

### VPC

* CIDR: `10.0.0.0/16`
* 3 public subnets in 3 different availability zones
* 3 private subnets in 3 different availability zones
* Public subnets routed via an **Internet Gateway**
* Private subnets associated with a route table that does not allow direct internet access

### EC2 Instance

* Launched in the first public subnet
* Public IP automatically assigned
* NGINX installed and started via **user data**
* Security group allows inbound traffic on **port 80** from anywhere

---

## Getting Started

### 1. Prerequisites

* AWS CLI and Terraform installed
* AWS credentials configured (`~/.aws/credentials` or environment variables)

---

### 2. Configure Variables

Edit the `terraform.tfvars` file:

```hcl
aws_region = "us-east-1"

vpc_cidr = "10.0.0.0/16"

public_subnet_cidrs  = ["10.0.1.0/24", "10.0.2.0/24", "10.0.3.0/24"]
private_subnet_cidrs = ["10.0.101.0/24", "10.0.102.0/24", "10.0.103.0/24"]

azs = ["us-east-1a", "us-east-1b", "us-east-1c"]

ami_id = "ami-00ca32bbc84273381" # Update this based on your region and OS
instance_type = "t3.micro"
```

### 3. Deploy the infrastructure

```bash
terraform init
terraform plan
terraform apply
```
