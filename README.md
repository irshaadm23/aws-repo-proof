# aws-repo-proof
Overview

This project demonstrates the design and implementation of a secure AWS VPC architecture with public and private subnets, controlled internet access, and restricted administrative access via a bastion host.

The objective is to isolate application resources from direct internet exposure while enabling controlled outbound connectivity and secure access for administration.

Architecture diagram
Architecture Summary VPC CIDR: 10.0.0.0/16 
Public Subnet: 10.0.0.0/24 Private Subnet: 10.0.1.0/24
Availability Zone: eu-west-2a 
Components Internet Gateway (IGW) NAT Gateway with Elastic IP Bastion Host (public subnet) Private EC2 Instance Public and Private Route Tables Security Groups Security Design Network Isolation

The private EC2 instance is deployed in a private subnet with no public IP address, preventing direct access from the internet.

Bastion Host Access

SSH access to the bastion host is restricted to a trusted IP address. The bastion host acts as the only entry point into the private subnet.

Private Instance Access

The private EC2 instance only allows SSH access from the bastion host via security group referencing. No inbound traffic from the public internet is permitted.

Controlled Outbound Access

The private subnet routes outbound traffic through a NAT Gateway, allowing instances to access external services without exposing them to inbound connections.

Traffic Flow Inbound Access: User → Internet Gateway → Bastion Host → Private EC2 (SSH) Outbound Access Private EC2 → NAT Gateway → Internet Route Tables Public Route Table 0.0.0.0/0 → Internet Gateway Private Route Table 0.0.0.0/0 → NAT Gateway Deployment Steps

Created a VPC with CIDR block 10.0.0.0/16
Created one public subnet and one private subnet Attached an Internet Gateway to the VPC Created a NAT Gateway in the public subnet with an Elastic IP Configured route tables: Public subnet routes traffic to the Internet Gateway Private subnet routes traffic to the NAT Gateway Launched EC2 instances: Bastion host in the public subnet Private instance in the private subnet Configured security groups to enforce restricted access Verified connectivity through SSH via the bastion host Monitoring

Basic monitoring was performed using Amazon CloudWatch to observe instance-level behaviour.
Monitored CPU utilisation, network traffic, and CPU credit usage Used near real-time metrics to understand system activity Verified that instances were running under low load with stable resource usage

Metrics:
This provided visibility into system performance and reinforced how infrastructure behaves in a controlled environment.

Validation:
Verification Successfully connected to bastion host via SSH from a trusted IP Successfully accessed private EC2 instance through bastion Verified outbound internet access from private EC2 (e.g. package updates)
