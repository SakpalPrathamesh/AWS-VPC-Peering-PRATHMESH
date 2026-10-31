# AWS VPC Peering Project

## Student Details
- Name: PRATHMESH SAKPAL
- Project: AWS VPC Peering
- AWS Region: Asia Pacific (Mumbai)
- Region Code: ap-south-1

## Project Description
This project demonstrates connectivity between two AWS Virtual Private Clouds using VPC Peering.

## Technologies Used
- Amazon VPC
- EC2 Instances
- Internet Gateway
- Route Tables
- Security Groups
- VPC Peering

## Project Objectives
- Create and configure VPCs.
- Configure public and private subnets.
- Establish VPC Peering.
- Configure routing and security groups.
- Verify private network connectivity.

## Project Evidence
Screenshots and project documentation are included in this repository.

## Author
PRATHMESH SAKPAL



🌐 AWS Inter-VPC Peering Architecture & Verification
End-to-End Cross-VPC Communication using AWS Peering Link, Route Tables, and Windows Bastion Host
📖 Executive Summary
This project showcases the architecture, deployment, and validation of an Inter-VPC Peering Connection in the AWS Mumbai Region (ap-south-1). By connecting two architecturally isolated Virtual Private Clouds (VPC-1 and VPC-2), internal workloads securely communicate using private IPv4 addressing without routing traffic over the public internet.
The network is validated using a Windows Server Bastion Host in VPC-1 to test TCP reachability against an isolated private server inside VPC-2.
🏗️ Network Architecture & Topology
<img width="1281" height="795" alt="1_VPC_Peering_Verification_True png" src="https://github.com/user-attachments/assets/47eb4362-ed52-4351-9711-ee14187f791b" />
<img width="1347" height="1167" alt="VPC Peering Architecture Diagram" src="https://github.com/user-attachments/assets/42b91a52-1bf1-4b0c-9243-0a9020ac6720" />

 
📌 Architecture Breakdown
•	Client [C] (Admin Laptop): Connects to the public Bastion Host over Remote Desktop Protocol (RDP Port 3389) via the attached Internet Gateway (igw-0668b48161683c161).
•	VPC-1 (`10.0.0.0/16`):
•	Public Subnet (`10.0.1.0/24`): Contains Bastion Host EC2-Public-VPC1 (52.66.49.9 / 10.0.1.254).
•	Private Subnet (`10.0.2.0/24`): Contains internal server EC2-Private-VPC1 (10.0.2.38).
•	Route Tables: Point internal subnet traffic locally and cross-VPC traffic to the peering connection.
•	VPC-2 (`10.20.0.0/16` - Fully Isolated):
•	Private Subnet (`10.20.0.0/16`): Contains target server EC2-Private-VPC2 (10.20.239.162). Has no internet gateway or public IPs attached.
•	VPC Peering Connection (`pcx-06adb74c0014abfff`):
•	Bidirectional tunnel interconnecting both VPC CIDRs.
•	Explicit route table entries in both VPCs ensure end-to-end symmetric routing.
📋 Infrastructure & IP Addressing Scheme
Resource / Parameter	VPC-1 (Requester)	VPC-2 (Accepter)	Description
VPC CIDR Block	10.0.0.0/16	10.20.0.0/16	Non-overlapping IPv4 address space
Public Subnet	10.0.1.0/24	*None*	Hosts the public-facing Bastion host
Private Subnet	10.0.2.0/24	10.20.0.0/16	Fully isolated internal subnets
Internet Gateway	igw-0668b48161683c161	*None*	Attached to VPC-1 for administrative access
Peering Connection	pcx-06adb74c0014abfff	pcx-06adb74c0014abfff	Bidirectional AWS VPC peering tunnel
Target Test Instance	EC2-Public-VPC1	EC2-Private-VPC2	Verified via RDP TCP Port 3389

🛠️ Step-by-Step Implementation
1. VPC & Subnet Provisioning
•	Created `VPC-1` (10.0.0.0/16) and `VPC-2` (10.20.0.0/16).
•	Configured dedicated public and private subnets across Availability Zone ap-south-1a.
•	Attached an Internet Gateway (VPC1-IGW) to VPC-1.
2. VPC Peering Tunnel Establishment
•	Initiated a peering request from VPC-1 targeting VPC-2.
•	Accepted the peering request (pcx-06adb74c0014abfff) to set the link status to Active.
3. Route Table Configurations
•	`VPC1-Public-RT`:
•	0.0.0.0/0 → igw-0668b48161683c161 (Internet egress)
•	10.0.0.0/16 → local
•	10.20.0.0/16 → pcx-06adb74c0014abfff (Cross-VPC Peering)
•	`VPC2-Private-RT`:
•	10.20.0.0/16 → local
•	10.0.0.0/16 → pcx-06adb74c0014abfff (Reverse Peering route)
4. Firewall & Security Group Rules
•	Configured `peering-sg-vpc1` and `peering-sg-vpc2`:
•	Inbound: Allow RDP (TCP Port 3389) and ICMP (IPv4) from anywhere (0.0.0.0/0) / VPC CIDRs for management and network probing.
•	Outbound: All traffic allowed (0.0.0.0/0).
🔍 End-to-End Verification & Evaluation Proof
Logged into the Bastion Host (EC2-Public-VPC1 at 28.0.1.254) via Remote Desktop and verified cross-network reachability to EC2-Private-VPC2 (28.200.239.162) using PowerShell.
Test-NetConnection -ComputerName 28.200.239.162 -Port 3389
💻 Execution Output
ComputerName           : 28.200.239.162
RemoteAddress          : 28.200.239.162
RemotePort             : 3389
InterfaceAlias         : Ethernet
SourceAddress          : 28.0.1.254
TcpTestSucceeded       : True
📸 Project Artifacts & Screenshots
1. Final Peering Verification (TcpTestSucceeded : True)
**Demonstrates successful private packet routing between VPC-1 and VPC-2 via the peering link.**
 
<img width="1281" height="795" alt="1_VPC_Peering_Verification_True png" src="https://github.com/user-attachments/assets/649449b4-c9af-4291-9d51-7dd12956216f" />



2. VPC-1 Route Table Configuration
**Validates the peering target route entry pointing destination `10.20.0.0/16` to `pcx-06adb74c0014abfff`.**
 

<img width="1619" height="971" alt="Route_Table_VPC_A_Updated" src="https://github.com/user-attachments/assets/6c01183c-398b-46f0-8ec1-971d9d7dd1c5" />







3. Deployed EC2 Instances
**Shows all three Windows Server instances in active running state across both VPCs.**
<img width="1602" height="982" alt="3_Running_EC2_Instances png" src="https://github.com/user-attachments/assets/98755a08-ffec-46ff-aa9e-1b16a381b1ad" />

 
💡 Key Engineering Takeaways
1.	Non-Overlapping CIDRs: VPC peering strictly requires non-overlapping IP address ranges (e.g., 10.0.0.0/16 and 10.20.0.0/16).
2.	Two-Way Symmetric Routing: Adding the peering connection alone is not sufficient; route tables on both VPCs must contain explicit entries directing target traffic to the pcx identifier.
3.	Public Bastion Route Requirement: The public route table (VPC1-Public-RT) must also contain the peering route to allow the Bastion host itself to inspect and probe workloads in the peered VPC.
4.	Reliable TCP Probing: Windows Server instances block ICMP pings by default. Using PowerShell's Test-NetConnection -Port 3389 confirms TCP socket establishment without modifying guest firewall policies.

