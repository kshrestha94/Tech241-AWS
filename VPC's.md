```
AMI copy of disk and faster time to load. More efficent 
```
`A Virtual Private Cloud (VPC)` 
```
is a virtual network infrastructure in the cloud that allows you to securely isolate and control your resources, providing a private and customizable network environment.
```
### why we use a VPC? 
```
data is usually stored in a default VPC which is accessible by many other. 

own private house (Vnet) and can set rooms (subnets) to be public or private. 
you define pathways to get to the public rooms or it wil default be private. (routes for public access)
```
## INTERNET GATEWAY - door into our VPC
```
An internet gateway is a component in a cloud network infrastructure that enables communication between resources within a virtual private cloud (VPC) and the internet, allowing inbound and outbound traffic to flow in and out of the VPC.
```

`2 subnets - public subnet and private subnet`

```
`public subnet - App VM`
`private subnet - DB VM`

subnets allow better control of network traffic and security. They are subdivisions of VPC's helping organise and manage IP address ranges.
```

## ROUTER 
```
A networking component within a VPC that facilitates communication between resources within the VPC and connections to external networks, such as the internet or other VPCs. It handles routing of network traffic, manages the flow of data between different subnets, and controls the flow of data in and out of the VPC.
```
```
- routes traffic around VPC, it uses a route table.
public router needs to be created 

eg. allows user to enter internet gateway to router to public subnet to acces app VM
```
```
private  router and route table is set as default
router path will stil go to NSG for protection to get into DB VM using 27017 port on AWS.
```
```
no route for SSH on private subnet due to no path 
therfore SSH into public VM in public subnet and then SSH into Private subnet to access db vm provided SSH in NSG rule. 

private vm is not connected to the internet so use a AMI with all dependencies for access 
```
```
basdian server - ssh into your VPC (top quality security)

```

## NETWORK SECURITY GROUP
```
there is NSG for both public and private subnet 
```
# CIDR Blocks

```
CIDR (Classless Inter-Domain Routing) blocks in VPC refer to the range of IP addresses that are allocated to a virtual private cloud. It is a notation that specifies the network prefix and the number of available IP addresses in the VPC subnet. CIDR blocks help define the IP address range and subnetting within the VPC, allowing for efficient allocation of IP addresses to resources and effective network management.

```
CIDR Block eg 10.0.0.0/16 (VNET)
if it starts with 10. it is private 

CIDR block private subnet 10.0.3.1-255/24

![Alt text](/images/VPC%20Image.png)

# CODE ALONG
### Creating a VPC STEP1

1.Create VPC tab 
2.VPC settings - VPC only 
3.Name tag: tech241-kevin-2-tier-vpc
4.IPv4 CIDR: 10.0.0.0/16
5.Create VPC

### Internet gateway STEP2

1.create internet gateway tab
2.Name tag: tech241-kevin-2-tier-vpc-igw
3.create internet gateway
4.Actions tab -attach to VPC STEP 3
5.Available VPC - select your VPC
6.attach internet gateway tab

### Subnets (private and Public) STEP4

1.subnets 
2.VPC -select yours 
3.subnet setting
-Name: public-subnet
-Availablity zone - euro -west 1a
CIDR block 10.0.2.0/24
4.add new subnet
- NAme :private subnet 
- Avail Zone: 1b
- CIDR block: 10.0.3.0/24
5.create subnet tab

# Route tables - to allow access from outside STEP5

1.route tables 
2.create routetable 
3.NAme tech241-kevin-public-rt
4.VPC select yours 
5.create route table tab

# STEP 6
6. subnet assocaition tab
7. edit subnet assoc on explicit subnet association
8. public subnet select 
9. save accociations

# STEP 7
1. routes 
2. add route
3. edit IG 0.0.0.0/0 
4. target igw kevin
5. save

## Resource Map
![Alt text](/images/resource%20map.png)

# step 8 creating your VMS


## First create db vm
```
create db vm
tech241-kevin-db-app-vpc
Image: ami from user data
instance: t2 micro
key pair: tech241
Network settings
VPC - tech241-kevin-2-tier-vpc
subnet - private
SG -tech241-kevin-app-only-vpc-sg-SSH-27017
custom TCP port 27017
source type - anywhere
```


## second create app in public subnet 
```
instances 
lanch instances 
name:tech241-kevin-app-only-vpc 
image: AMI for asg - quicker 
2t micro 
network settings:


edit - VPC 2 tier VPC
subnet - pubic
create new NSG - 
SG Name: tech241-kevin-app-only-vpc-sg-SSH-HTTP-3000
add http 
source anywhere 

add custom tcp
source type anywhere
port 3000
source anywhere des sparta app

user data 
script with only app and insert private IP of DBvm 


lanuch instance 
details 
copy public IP/posts
insert on new tab


```

# Research task!

VPCs (Virtual Private Cloud) - What, why, benefits

`How does it help the business?`

A VPC helps businesses by providing a secure and isolated network environment in the cloud, allowing them to have full control over their resources, ensuring data privacy, enabling secure communication between services, and facilitating the seamless integration of cloud-based infrastructure into their existing IT infrastructure.

`How does it help DevOps?`

A VPC helps DevOps by providing a dedicated network environment for deploying and managing applications and services, enabling secure communication, facilitating network segmentation for different environments (such as development, testing, and production), and allowing for fine-grained control over network configurations to support DevOps practices like automation, scalability, and efficient resource utilization.

`Why did AWS need to introduce VPCs`

AWS introduced VPCs to provide customers with a secure and isolated network environment in the cloud, allowing them to have granular control over their resources, establish private networks, ensure data privacy, and seamlessly integrate their cloud infrastructure with their existing on-premises infrastructure.





include a diagram in your doc

Public + private subnets

Route table/s
How security groups work on instance level
