# 🚀 Terraform EKS Cluster Deployment on AWS

This guide provisions an Amazon Elastic Kubernetes Service (EKS) cluster using Terraform, including configurations for accessing and managing the cluster.

---

## 🛠️ Prerequisites

Ensure the following are installed and configured before starting:

- **AWS CLI**: Version 2.x. [Install guide](https://docs.aws.amazon.com/cli/latest/userguide/install-cliv2.html).
- **Kubectl**: Compatible with your EKS cluster version (e.g., 1.30). [Install guide](https://kubernetes.io/docs/tasks/tools/install-kubectl/).
- **Terraform CLI**: Version 1.x (recommended: 1.5 or later). [Install guide](https://developer.hashicorp.com/terraform/install).
- **AWS IAM User**: With programmatic access and sufficient permissions (e.g., a custom policy with EKS, VPC, IAM, and EC2 permissions).
- **AWS Credentials**: Configured via `~/.aws/credentials` or environment variables:
  ```bash
  export AWS_ACCESS_KEY_ID="your-access-key"
  export AWS_SECRET_ACCESS_KEY="your-secret-key"
  export AWS_DEFAULT_REGION="us-east-1"
  ```

---

## 🧪 Steps to Deploy EKS Using Terraform

### 1️⃣ Authenticate with AWS CLI

Configure your AWS CLI with the appropriate credentials:

```bash
aws configure
```

Provide:

- AWS Access Key ID
- AWS Secret Access Key
- Default region (e.g., `us-east-1`)
- Output format (e.g., `json`)

Verify authentication:

```bash
aws sts get-caller-identity
```

### 2️⃣ Clone or Set Up Terraform Code

Navigate to your Terraform project directory containing the EKS configuration (e.g., `main.tf`, `variables.tf`, `outputs.tf`). If you don’t have one, use or adapt the [AWS EKS Terraform module](https://registry.terraform.io/modules/terraform-aws-modules/eks/aws/latest).

Example directory setup:

```bash
cd IAC-EKS-Cluster
```

Ensure your Terraform configuration includes:

- VPC setup (subnets, route tables, NAT gateway, etc.)
- EKS cluster with managed node groups or Fargate profiles
- IAM roles for EKS and node groups
- Security group rules for cluster communication

### 3️⃣ Initialize Terraform

Initialize the Terraform working directory to download providers and modules:

```bash
terraform init
```

### 4️⃣ Preview the Terraform Execution Plan

Review the resources Terraform will create or modify:

```bash
terraform plan
```

Check the output for correctness (e.g., cluster name, region, node group size).

### 5️⃣ Deploy the EKS Cluster

Apply the Terraform configuration to provision the EKS cluster:

```bash
terraform apply
```

Type `yes` when prompted to confirm. This may take 10–15 minutes.

### 6️⃣ Configure Kubeconfig for EKS

Update your `kubectl` configuration to connect to the EKS cluster. Replace `ap-south-1` and `cluster-name` with your region and EKS cluster name (as defined in your Terraform config):

```bash
aws eks --region ap-south-1 update-kubeconfig --name cluster-name
```

Verify cluster access:

```bash
kubectl get nodes
```

You should see the nodes in your EKS cluster.

---

## 🧹 Cleanup

To destroy the EKS cluster and associated resources, run:

```bash
terraform destroy
```

Type `yes` when prompted. Ensure you’ve backed up any critical data before destroying.

---
