# Terraform_with_AWS_Project

This project demonstrates how to implement Infrastructure as Code (IaC) using Terraform on AWS.
It provisions a complete environment including VPC, subnets, Internet Gateway, route tables, security groups, EC2 instances, S3 bucket, and an Application Load Balancer (ALB).

## 🛠️ Prerequisites
- An AWS Account
- Installed Terraform → Download here "https://developer.hashicorp.com/terraform/downloads?utm_source=copilot.com"
- Installed AWS CLI and configured credentials:
  
```
aws configure
```
Provide your Access Key, Secret Key, region (e.g., us-east-1), and output format (json).
****
📂 Project Structure
```
Terraform_Nav/
│── provider.tf
│── variables.tf
│── main.tf
│── userdata.sh
│── userdata1.sh
```
****
## ⚙️ Steps Implemented in main.tf

1. Create a VPC
    - Define a Virtual Private Cloud (VPC) using the CIDR block specified in variables.tf.
    - This provides an isolated network environment for all AWS resources.
![vpc](../images/terraform_vpc.jpeg)

2. Create Two Subnets
    - Add two public subnets (10.0.0.0/24 and 10.0.1.0/24) in different Availability Zones (us-east-1a and us-east-1b).
    - Enable automatic public IP assignment for instances launched in these subnets.
![vpc](../images/terraform_subnet.jpeg)

3. Create an Internet Gateway
    - Attach an Internet Gateway to the VPC.
    - This allows resources in the public subnets to connect to the internet.

4. Create a Route Table and Associate It
    - Define a route table with a default route (0.0.0.0/0) pointing to the Internet Gateway.
    - Associate this route table with both subnets so they become public.
![vpc](../images/terraform_rt.jpeg)
![vpc](../images/terraform_step1.png)

5. Create a Security Group
    - Configure inbound rules to allow:
    - HTTP traffic on port 80 from anywhere.
    - SSH traffic on port 22 from anywhere.
    - Configure outbound rules to allow all traffic.
    - Attach this security group to EC2 instances and the load balancer.
      
6. Create an S3 Bucket
    - Define an S3 bucket for storage.
    - This can be used for logs, backups, or static content.
![vpc](../images/terraform_s3.jpeg)

7. Launch Two EC2 Instances
    - Deploy two t2.micro instances in different subnets.
    - Use Amazon Ubuntu AMI.
    - Apply startup scripts (userdata.sh and userdata1.sh) to configure webservers.
    -  Attach the security group for secure access.
![vpc](../images/terraform_instances.jpeg)
![vpc](../images/terraform_step2.png)

Here Check the public ip's are accessible of both the instances.
![vpc](../images/terraform_output1.png)
![vpc](../images/terraform_output2.png)

8. Create an Application Load Balancer (ALB)
    - Deploy an ALB across both subnets.
    - Attach the security group.
    - This distributes incoming traffic across the EC2 instances.
![vpc](../images/terraform_step3.jpeg)

9. Create a Target Group and Attach Instances
    - Define a target group for HTTP traffic on port 80.
    - Attach both EC2 instances to the target group.
    - Configure health checks to monitor instance availability.
![vpc](../images/terraform_targetgroup.jpeg)

10. Create a Load Balancer Listener
    - Configure the ALB to listen on port 80 (HTTP).
    - Forward traffic to the target group.
![vpc](../images/terraform_loadbalancer.jpeg)

11. Output the Load Balancer DNS
    - Print the DNS name of the ALB after deployment.
    - Use this DNS in a browser to access the webservers.
![vpc](../images/terraform_outputdns1.png)
![vpc](../images/terraform_outputdns2.png)
****
▶️ Deployment Flow

```
terraform init       # Download providers
terraform validate   # Validate configuration
terraform plan       # Preview resources
terraform apply      # Create resources
```
****
After apply, Terraform outputs:
```
loadbalancerdns = <your-alb-dns>
```
Open this DNS in a browser to test your webservers.
****
🧹 Cleanup
To destroy all resources:
```
terraform destroy
```
![vpc](../images/terraform_destroy.png)

