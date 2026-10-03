# Lab 02: Secure Small-Business VPC

## Scenario: 
Desert Bloom Dental, a small business, needs a public website and a private internal application server. The internal server must never be reachable from the internet, and no server may expose SSH. 

## Objective
1. Create two-tier VPC across two availability zones (AZ).
2. Used least privileged security groups.
3. Use Private Administration through *Session Manager*
4. Implement CloudTrail to support flow logging to troubleshoot.

## Service Used
EC2, Security Group, CloudWatch, NAT Gateway, Internet Gateway, Session Manager
>[!Warning]
>Nat Gateways are billed hourly, as well as the use of public IPv4 address. The implementation of ***Budget*** to provide warning to you is highly suggested for this lab. refer to lab [01_Secure_Accaount_Foundation](Awsl_Labs/Secure_Account_Foundation/01_Lab_Secure_Foundation.md) for instructions. 

## Architecture
![Cloud Architecture](./diagram/Secure_Small_Business_VPC.png)

|Subnet|CIDER|TYPE|
|------|------|-------|
|dbd-public-a| 10.0.1.0/24 | Public |
|dbd-public-b| 10.0.2.0/24 | Public |
|dbd-private-a | 10.0.11.0/24 | Private |
|dbd-private-b | 10.0.12.0/24 | Private |


VPC CIDR 10.0.0.0/16 chosen assuming no overlap with on-premises networks; a production design would confirm the client's existing ranges to support future VPN connectivity." It shows you're thinking about hybrid networking, which matches your background.

## Implementation

## Verification
Please navigate to [Verification where](./CLI/verification-commands.md) I used the AWS CloudShell to verify my work.

## Trouble Ticket Scenario 

## Lessons Learned
### Subnetting AWS for VPCs 
1. Its better to use a /24 on the public VPC Subnets as the your external resources sit their along with the NAT Gateways. Also, some of the AWS resources need a larger subnet as they scale up.
