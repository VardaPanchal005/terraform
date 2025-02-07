# Terraform AWS Setup: VPC, Subnets, EC2 Instances, and Load Balancer

This Terraform project automates the deployment of AWS infrastructure, including:

- A **VPC** with the CIDR block `10.0.0.0/16`.
- Two **public subnets** across different Availability Zones (`ap-south-1a` and `ap-south-1b`).
- An **Internet Gateway (IGW)** for external internet access.
- Two **EC2 instances** running Apache with custom HTML pages.
- An **Application Load Balancer (ALB)** to distribute traffic to the EC2 instances.
- A **Security Group** allowing HTTP and SSH traffic.

## Prerequisites

Before starting, ensure the following:

1. **Terraform** is installed on your local machine.
2. **AWS Account**: Set up your AWS credentials locally using the [AWS CLI](https://docs.aws.amazon.com/cli/latest/userguide/cli-configure-quickstart.html).
3. **IAM Permissions**: Ensure your AWS account has permissions for VPC, EC2, ALB, and IAM roles.

## Steps to Implement

### 1. Install Terraform on Windows

#### **Download Terraform**
- Go to the [Terraform download page](https://www.terraform.io/downloads.html).
- Under the **Windows** section, download the **64-bit** version of Terraform.

#### **Extract the ZIP file**
- Extract it to a directory of your choice, e.g., `C:\terraform`.

#### **Add Terraform to the System Path**
1. Open **File Explorer**, right-click **This PC** → **Properties**.
2. Click **Advanced system settings** → **Environment Variables**.
3. In **System variables**, select **Path** and click **Edit**.
4. Add `C:\terraform` to the list.
5. Click **OK**.

#### **Verify Installation**
Run the following command:
```bash
terraform --version
```

### 2. Set up AWS Credentials

#### **Obtain AWS Access Key and Secret Access Key**
1. Log in to the **AWS Management Console**.
2. Go to **IAM (Identity and Access Management)**.
3. Navigate to **Users** → Select your user → **Security credentials**.
4. Under **Access keys**, click **Create access key**.
5. Copy and save the **Access Key ID** and **Secret Access Key**.

#### **Configure AWS CLI**
Run:
```bash
aws configure
```
Enter the required credentials:
- **AWS Access Key ID**
- **AWS Secret Access Key**
- **Default region** (e.g., `ap-south-1`)
- **Default output format** (`json`, `text`, or `table`)

### 3. Create Necessary Files

#### **main.tf**
Defines the VPC, subnets, security groups, EC2 instances, ALB, and route table.

#### **variables.tf**
Defines the VPC CIDR block (`10.0.0.0/16` by default).

#### **userdata.sh & userdata1.sh**
User-data scripts to install Apache and configure HTML pages on EC2 instances.

### 4. Get the ALB ID from AWS Console

1. Log in to the **AWS Management Console**.
2. Navigate to **EC2** → **Load Balancers**.
3. Select your **Load Balancer** (`myalb`).
4. Copy the **Load Balancer ARN**, which includes the ALB ID.

### 5. Get the ALB DNS Name

Run the following AWS CLI command:
```bash
aws elbv2 describe-load-balancers --names myalb --query "LoadBalancers[0].DNSName"
```
This returns the ALB DNS Name, e.g.:
```plaintext
myalb-1234567890.ap-south-1.elb.amazonaws.com
```

### 6. Application Load Balancer and Page Switching

The ALB routes requests to the two EC2 instances in a round-robin manner:
- **First request** → `webserver1`
- **Second request** → `webserver2`

Since each EC2 instance has a unique HTML page, refreshing the ALB URL will show different content from each instance.

### 7. Initialize Terraform

Run:
```bash
terraform init
```
This downloads the required providers and sets up the environment.

### 8. Review the Terraform Plan

Run:
```bash
terraform plan
```
This shows the resources Terraform will create, modify, or delete.

### 9. Apply the Terraform Configuration

Run:
```bash
terraform apply
```
Type `yes` to confirm and deploy the infrastructure.

### 10. Access the Application

Visit the ALB DNS Name in your browser:
```plaintext
http://myalb-1234567890.ap-south-1.elb.amazonaws.com
```
This displays the HTML page served by the EC2 instances.

### 11. Clean Up Resources

To destroy all AWS resources:
```bash
terraform destroy
```
Confirm with `yes` when prompted.

## Conclusion

By following these steps, you will successfully deploy a VPC with EC2 instances behind an ALB, accessible via the internet. This setup ensures high availability and scalability for your web application.

Terraform allows easy resource management and versioning, making it an ideal tool for modern cloud infrastructure.
