## **Overview**

This lab demonstrates how to build a scalable, fault-tolerant AWS infrastructure by using Elastic Load Balancing (ELB) and Amazon EC2 Auto Scaling.
I configured an Application Load Balancer to distribute traffic across multiple instances and created an Auto Scaling group to automatically adjust EC2 capacity based on demand.
The environment dynamically responds to traffic changes while maintaining performance and cost efficiency.

## **Objectives & Learning Outcomes**

After completing this lab, I was able to:

Create an AMI from a running EC2 instance.

Configure an Application Load Balancer (ALB) for cross-AZ high availability.

Create a Launch Template and an Auto Scaling Group.

Integrate CloudWatch Alarms to trigger scaling events based on CPU utilization.

Verify the system’s ability to handle load spikes and reduce capacity automatically.

## **Architecture**

<img width="1024" height="1200" alt="9c3c3a9c-556a-409f-9714-09a8bf3b70e9" src="https://github.com/user-attachments/assets/4f34da3b-af56-4ecc-a42b-dc0b803541f6" />

Architecture Summary:

Load Balancer: Routes traffic to instances across 2 Availability Zones.

Auto Scaling Group: Maintains 2–4 EC2 instances based on CPU utilization.

Launch Template: Uses a custom “Web Server AMI” for consistent instance configuration.

CloudWatch Alarms: Trigger scale-out when CPU > 50 %, and scale-in when CPU < 50 %.

VPC Setup: Public subnets host the ALB; private subnets host EC2 instances for improved security.

## **Commands & Steps**
```bash
# Step 1 – Create an AMI from the existing EC2 instance
# EC2 Console → Actions → Image and Templates → Create Image
# Name: Web Server AMI

# Step 2 – Create the Application Load Balancer (LabELB)
# EC2 Console → Load Balancers → Create → Application Load Balancer
# Select VPC, Public Subnets (AZ1 & AZ2), Web Security Group

# Step 3 – Create Target Group
# Type: Instances | Name: lab-target-group

# Step 4 – Create Launch Template
# Name: lab-app-launch-template
# AMI: Web Server AMI | Instance type: t3.micro | SG: Web Security Group

# Step 5 – Create Auto Scaling Group
# Name: Lab Auto Scaling Group
# Subnets: Private Subnet 1 & 2
# Load Balancer: lab-target-group | Health check: ELB
# Capacity: Desired=2, Min=2, Max=4
# Scaling Policy: Target tracking | Metric: CPU Utilization | Target=50%

# Step 6 – Verify and Test
# Check healthy targets in lab-target-group
# Access Load Balancer DNS via browser
# Simulate load using the “Load Test” page

```

## **Screenshots**

create-ami.png	Creating “Web Server AMI” from EC2 instance
<img width="1667" height="290" alt="AMI" src="https://github.com/user-attachments/assets/8afa3896-fc6a-4db8-837a-924ec692fbbc" />

create-elb.png	Application Load Balancer (LabELB) successfully created
<img width="1638" height="517" alt="ELB" src="https://github.com/user-attachments/assets/c9197c3d-305f-405d-b0c7-03bfecaca838" />

auto-scaling-group.png	Auto Scaling group configuration (desired = 2, max = 4)
<img width="1834" height="309" alt="auto-scaling group" src="https://github.com/user-attachments/assets/9d073c3f-cd17-4bcc-bad1-f4366c68bebe" />

cloudwatch-alarms.png	CloudWatch AlarmHigh triggered and scaling initiated
<img width="1502" height="378" alt="highalarm" src="https://github.com/user-attachments/assets/221d5918-e065-479c-90ac-e0f5d7579edd" />

load-test-browser.png	Load Test app accessible through ALB DNS name
<img width="1550" height="513" alt="load test browser" src="https://github.com/user-attachments/assets/22db20d4-c091-48cb-9e26-89b86e1b8a81" />

## **Tools Used**

Amazon EC2 – Compute service for web servers

Elastic Load Balancing (ALB) – Distributes incoming traffic

Auto Scaling Group – Automatically adjusts instance count

Amazon CloudWatch – Monitors metrics and triggers scaling

Amazon Machine Image (AMI) – Template for consistent server deployment

VPC with Subnets and Security Groups – Networking and access control

## **What Actually Happened** 

Created a reusable Web Server AMI from an existing EC2 instance.

Built an Application Load Balancer to distribute web traffic across Availability Zones.

Designed a Launch Template to define how new EC2 instances should be configured.

Set up an Auto Scaling Group to maintain desired capacity (2–4 instances).

Configured CloudWatch alarms to trigger scaling when CPU > 50 %.

Verified scaling behavior by performing a Load Test that increased CPU utilization, resulting in new EC2 instances being launched.

Terminated the original standalone web server to rely solely on the scalable group.

## **Author**
AMarachi Emeziem

Cloud Engineer / AWS cloud Certified
