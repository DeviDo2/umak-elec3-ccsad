# Assignment 2 Submission

<!--
How to use this template:

1. Copy this file to submissions/assignment-2/<your-github-username>/submission.md
2. Replace every answer placeholder with your own answer. Delete the angle brackets too.
3. Save your four images in the same folder, with the exact file names below.
4. Do not write your full name or your student number in this file.
-->

## About me

- GitHub username: romeonspotify
- Section: ccsad
- IAM user name that I signed in with: ccsad-g07
- X: 107

---

## Part A. Explore

### A1. The VPC

Default VPC IPv4 CIDR:

172.31.0.0/20
172.31.32.0/20
172.31.16.0/20

Number of addresses in that CIDR:

3

### A2. The subnets

| Availability Zone | IPv4 CIDR |
| --- | --- |
| apne1-az1 (ap-northeast-1c)| 172.31.0.0/20 |
| apne1-az4 (ap-northeast-1a)| 172.31.32.0/20 |
| apne1-az2 (ap-northeast-1d)| 172.31.16.0/20 |

Screenshot 1. Save it as `screenshot-1-subnets.png` in your folder. The image line below shows it.

![Screenshot 1: subnet list](screenshot-1-subnets.png)

### A3. Available addresses

Available IPv4 addresses in each subnet:

4,091

Why is the number lower than 4,096?

because a /20 subnet has 4,096 total IPv4 addresses but AWS reserves 5 addresses in each subnets for its own networking purposes

What uses the missing address in the subnet with the lowest number?

172.31.0.0

### A4. The route table

| Destination | Target |
| --- | --- |
| 0.0.0.0/0 | 	
igw-0eb50cb3b8dc34012 |
| 172.31.0.0/16 | local |

Screenshot 2. Save it as `screenshot-2-routes.png` in your folder. The image line below shows it.

![Screenshot 2: routes of the route table](screenshot-2-routes.png)

### A5. Public or private

Are the default subnets public or private? Which route proves it?

Public, route 0.0.0.0/0 sends traffic to internet gateway

### A6. The internet gateway

State of the internet gateway:

Attached

What happens to the default subnets if the gateway is detached?

The default subnets lose their direct route to the internet

### A7. NAT gateways

Number of NAT gateways:

0

Can a server in a new private subnet download updates? Why?

No,a private subnet should have a route through a NAT Gateway

### A8. The network ACL

| Rule number | Source | Allow or Deny |
| --- | --- | --- |
| 100 | 0.0.0.0/0 | Allow |
| * | 0.0.0.0/0 | Deny |

How is a network ACL different from a security group?

A network ACL controls traffic at the subnet level and is stateless, while a security group controls traffic at the instance level and is stateful

Screenshot 3. Save it as `screenshot-3-network-acl.png` in your folder. The image line below shows it.

![Screenshot 3: inbound rules of the network ACL](screenshot-3-network-acl.png)

### A9. The default security group

Inbound rule (type and source):

All traffic sg-095ea6a6641c75d87 / default

Which resources can send traffic to an instance that uses it?

Only resources that also use the default security group


---

## Part B. Prepare

### B1. Plan two subnets

- Public subnet CIDR: 10.172.0.0/24
- Private subnet CIDR: 10.172.1.0/24

### B2. Route tables

Route table of the public subnet:

| Destination | Target |
| --- | --- |
| 10.172.0.0/16 | Local |
| 0.0.0.0/0 | Internet Gateway|

Route table of the private subnet:

| Destination | Target |
| --- | --- |
| 10.172.1.0/24 | Internet Gateway|

### B3. My VPC diagram

Tool used (Excalidraw, draw.io, Lucidchart, or paper):

Yes, The local route for the VPC range is still in the route table, which connects every subnet in the VPC

Save your diagram as `vpc-diagram.png` in your folder. The image line below shows it.

![B3: my VPC diagram](vpc-diagram.png)

### B4. Predict a change

Can you still open the web page from your laptop? Why?

No. Even though the laptop has internet access, there is no 0.0.0.0/0 route, so traffic going to an internet address has no route. Having a public IP address alone is not enough.

Can the instance still reach another instance in the VPC? Why?

Yes. The route table still has a local route for the VPC's IP range, allowing instances in different subnets within the same VPC to communicate.

### B5. Place a database

Which subnet gets the database? Why?

The database should be placed in the private subnet, 10.132.1.0/24. Since its route table does not have a route to the Internet Gateway, the database cannot be directly accessed from the internet.


### B6. My question about VPCs

What is your question, and what made you think of it?

How can developers or administrators securely access and manage a database that is located in a private subnet without internet access? I thought of this because if the private subnet has no route to the Internet Gateway, I wondered how a database administrator using a laptop from home could securely log in, run queries, update the database schema, or perform maintenance.