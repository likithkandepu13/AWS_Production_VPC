(Real Time Project): How Web applications hosted in AWS VPC with Private and Public Subnets
This example demonstrates creating a VPC for deploying servers in a production environment.

To improve resiliency, servers are deployed across two Availability Zones using an Auto Scaling group and an Application Load Balancer. For added security, servers are placed in private subnets, receiving requests via the load balancer. These servers can connect to the internet through a NAT gateway, which is also deployed in both Availability Zones to enhance resiliency.

The VPC is configured with public and private subnets in two Availability Zones.

Each public subnet includes a NAT gateway and a load balancer node.

An Auto Scaling group manages servers within the private subnets, handling both the launching and termination processes. They receive traffic routed from the load balancer.

Servers access the internet through the NAT gateway.

Key components:

NAT Gateway: Masks the IP address when the application communicates with other applications.

Auto Scaling Group: Automatically scales server instances based on incoming traffic by replicating EC2 instances.

Load Balancer: Distributes incoming requests based on traffic flow to ensure balanced resource utilization.

Bastion Host or Jump Server: Used for secure SSH access through the private subnet, directing SSH traffic to the application.

Implementation steps:



Maximize image
Edit image
Delete image

Public-Private Subnet Architecture
1. Create a VPC
Step 1: In the AWS Management Console, navigate to the VPC Dashboard.

Step 2: Click on "Create VPC."

Step 3: Define the CIDR block for your VPC (e.g., 10.0.0.0/16).

Step 4: Name your VPC and create it.

2. Create Public and Private Subnets
Step 1: Still in the VPC Dashboard, navigate to "Subnets."

Step 2: Create two public subnets, each in different Availability Zones (e.g., 10.0.1.0/24 in AZ1 and 10.0.2.0/24 in AZ2).

Step 3: Similarly, create two private subnets in the same Availability Zones (e.g., 10.0.3.0/24 in AZ1 and 10.0.4.0/24 in AZ2).

3. Create and Attach an Internet Gateway
Step 1: In the VPC Dashboard, navigate to "Internet Gateways."

Step 2: Click "Create Internet Gateway," name it, and create it.

Step 3: Attach the Internet Gateway to your VPC.

4. Create Route Tables
Step 1: Go to "Route Tables" in the VPC Dashboard.

Step 2: Create a route table for the public subnets.

Step 3: In the routes section, add a route that directs 0.0.0.0/0 traffic to the Internet Gateway.

Step 4: Associate this route table with the public subnets.

Step 5: Create a separate route table for the private subnets (it should only route within the VPC by default).

5. Create NAT Gateways
Step 1: In the VPC Dashboard, navigate to "NAT Gateways."

Step 2: Create a NAT Gateway in each public subnet. Allocate an Elastic IP for each.

Step 3: Update the private subnet route table to direct 0.0.0.0/0 traffic to the NAT Gateway.(port 8000)

6. Launch EC2 Instances
Step 1: Go to the EC2 Dashboard.

Step 2: Launch EC2 instances in the private subnets. (aws_production )

Step 3: Ensure the Security Groups allow inbound traffic from the load balancer and outbound traffic to the NAT Gateway.

7. Configure Auto Scaling
Step 1: In the EC2 Dashboard, set up an Auto Scaling Group.

Step 2: Define the launch configuration or template with instance details.

Step 3: Set the desired capacity, minimum, and maximum number of instances.

Step 4: Attach the Auto Scaling group to the private subnets.

8. Set Up an Application Load Balancer
Step 1: In the EC2 Dashboard, navigate to "Load Balancers."

Step 2: Create an Application Load Balancer, selecting the public subnets for availability.

Step 3: Define listeners and configure health checks.

Step 4: Register your private EC2 instances with the load balancer.

9. Configure a Bastion Host (Optional but Recommended)
Step 1: Launch an EC2 instance in one of the public subnets to act as a Bastion Host.

Step 2: Ensure the Security Group allows SSH access from trusted IPs.

Step 3: Use this Bastion Host to SSH into the private instances.

10. Test and Validate
Step 1: Ensure the public instances can access the internet directly.

Step 2: Ensure the private instances access the internet via the NAT Gateway.

Step 3: Test traffic flow through the load balancer to the private instances.

Step 4: Verify that the Auto Scaling Group launches and terminates instances based on load

Follow the video for detailed execution of the complete architecture:
