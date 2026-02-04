                                                             
Before directly jumping into the AWS, we should know the basics of **CLOUD COMPUTING.**
**WHAT IS** **CLOUD COMPUTING**?
**Cloud Computing** is a technology that allows you to store and access data and applications over the internet instead of using your computer’s hard drive or a local server.

**CLOUD COMPUTING ARCHITECTURE**




<img width="800" height="400" alt="image" src="https://github.com/user-attachments/assets/affda1d4-7a67-4991-98bb-06d2e8066000" />





**Front End**
Client Infrastructure (Browser, Mobile App, Laptop)

**Back End**

**Application Layer** – Provides cloud-based applications to users.

**Serv****ice La****yer** – Provides core cloud services like compute, storage, database.

**Runtime Cloud** – Execution environment for applications.

**Storage Layer** – Stores data in the cloud (S3, EBS, EFS).

**Infrastructure Layer** – Physical servers, networks, virtual machines.

**Management** – Resource monitoring, billing, scaling.

**Security** – IAM, encryption, firewall policies.

**Communication happens over the Internet.**
# 5 Core Characteristics of Cloud Computing
Cloud computing has ***five essential characteristics*** that differentiate it from traditional IT infrastructure:

### 1) On-Demand Self-Service
Users can **create and manage resources** (like servers, storage, databases) **whenever they need**, without requiring support from the cloud provider.
Everything is done through a dashboard or API.
**Example:**
 Launching an EC2 server in AWS console in seconds, without calling any IT admin.
 
### 2) Broad Network Access
Cloud services can be accessed from anywhere using the **internet**.
Any device like **laptop, mobile, desktop, or tablet** can connect using standard web protocols.
**Example:**
 Logging into AWS Console from your laptop at home or office.
 
### 3) Resource Pooling
AWS shares its large pool of servers and storage across many customers.
This model is called **Multi-Tenancy**.
Resources are dynamically allocated depending on user demand.
**Example:**
Multiple companies use the same AWS data center but isolated logically, so data is secure.

### 4) Rapid Elasticity (Scalability)
Resources can be increased or decreased instantly based on demand.
Cloud automatically adjusts capacity.
**Example:**
 During festival sale, e-commerce websites handle high traffic using **Auto Scaling** in AWS.
 When traffic reduces, servers scale down to save costs.
 
### 5) Measured Service (Pay-as-You-Go)
You only pay for what you use.
Usage is measured, monitored, and billed accordingly.
**Example:**
 If your EC2 server runs for 3 hours, AWS charges only for those 3 hours.
### The Shared Responsibility Model
Security in the cloud is a partnership between the cloud provider and the customer. This is known as the** **Shared Responsibility Model**.
**The Cloud Provider (e.g., AWS, Azure, GCP)** is responsible for the security *OF* the cloud. This includes the physical security of the data centers, the hardware, and the core networking infrastructure.
**The Customer (You)** is responsible for security **IN** the cloud. This includes managing who has access to your resources, encrypting your data, configuring firewalls (like Network Policies), and securing your applications.



THERE ARE FOUR TYPES OF  CLOUD COMPUTING

<img width="800" height="400" alt="image" src="https://github.com/user-attachments/assets/3d9125bc-7be3-4923-b84d-00bf76a1549a" />


***IaaS (Infrastructure as a Service):***
Provides virtual servers, storage, networking.
User manages OS & applications.
Example: AWS EC2
Use: When full system control is required.

***PaaS (Platform as a Service):***
Provides platform to develop/deploy apps.
User manages only the application code.
Example: AWS Elastic Beanstalk, Heroku
Use: For developers who want to avoid server setup.

***SaaS (Software as a Service):***
Ready-to-use software delivered over the internet.
No installation or maintenance needed.
Example: Gmail, Office 365
Use: For end users.

***FaaS*** ***(Function as a Service) / Serverless:***
Run functions without managing servers.
Pay only when code executes.
Example: AWS Lambda
Use: Event-driven tasks & automation.


**CLOUD DEPLOYMENT MODELS**
**Cloud Deployment Models:**
**1) Public Cloud:**
   - Services offered over the internet to the general public.
   - Multi-tenant and cost-effective.
   - Examples: AWS, Azure, GCP.
   - 
**2) Private Cloud:**
   - Cloud used by a single organization only.
   - Very secure but expensive.
   - Examples: VMware Private Cloud, OpenStack.
   - 
**3) Hybrid Cloud:**
   - Combination of Public + Private Cloud.
   - Sensitive data in private cloud, applications in public cloud.
   - Best for flexibility and security.

**Multi-Cloud Deployment:**
Uses more than one public cloud provider (e.g., AWS + Azure + GCP).
Offers flexibility to use best services from each cloud.
Avoids vendor lock-in and improves reliability.
Example: AWS EC2 for servers + Google BigQuery for analytics.

**TOP LEADING CLOUD COMPUTING COMPANIES**

**AWS,AZURE**,**GCP,ALIBABA**,**ORACLE,IBM**,**SALESFORCE,TENCENT**

**Important words to **remeber:region,availability** **zone, edge** location.**

Source for cloud computing: 
VIRTUALISATION allows multiple virtual machines to run inside one physical server.
Because cloud is built on virtualization technology
Cloud provides three main types of storage
Object storage – store images, back up data, static files., file storage –shared file syste

**AWS-Amazon Web Services**

**WHAT ****IS**** AWS**

AWS (Amazon Web Services) is a cloud platform that allows us to use servers, storage, databases, and networking over the internet on a pay-as-you-go basis.
**AWS** is a **cloud computing service** provided by Amazon that lets us **rent computing resources** like virtual machines (EC2), storage (S3), and databases (RDS) instead of buying physical hardware.
 We pay **only for what we use**, and AWS handles the **maintenance, security, and scaling**.
 
**AWS – ARCHITECTURE**

<img width="1318" height="1020" alt="image" src="https://github.com/user-attachments/assets/5487efef-6104-454c-89c0-3014d9e743bb" />


***Why AWS?***

Pay-as-you-go pricing → Cost-effective
Highly scalable and elastic
High availability with multiple regions and AZs
Global presence with low latency access
Strong security and compliance
200+ services for compute, storage, database, networking, etc.
Supports DevOps tools and automation
AZURE IS BEST FOR THOSE WHO ARE USING AN MICROSOFT TOOLS
GOOGLE IS BEST FOR AI ML AND BIG QUERY AND DATA ANALYTICS.

**AS THERE ARE MULTIPLE CLOUD PLATFORMS, WHY ONLY AWS?**

Even though there are other cloud providers like Azure and GCP, AWS is preferred because:
1) AWS is the largest and most mature cloud platform.
2) It has the highest number of services and features.
3) It has the biggest global network of regions and availability zones.
4) It provides strong reliability, security, and scalability.
5) It offers flexible pricing and cost optimization options.
6) It has huge community support and training resources.
Amazon offers over 200+ services

**UNDERSTANDING AN AWS FUNDAMENTALS**

AWS operates in multiple geo graphical locations world wide. Each region consists of one or more data data centres and i designed to reduce latency to the users.

***AVAILABILITY ZONES***:

Each region has an multiple available zones which are isolated data centres these zones provides high availabilty and fault tolerance ensuring that your application remains operational even if one zone fails.

***GLOBAL NETWORK INFRASTRUCTURE***

AWS has high speed network of global fiber connections linking its data centers. This infrastructure enables an optimized performance and reduced latency, with better disaster recovery.
AWS Global Infrastructure:
Geographical Location (INDIA, AMERICA, SINGAPORE etc.)
    ↓
Region (e.g., ap-south-1, us-east-1)
    ↓
Availability Zones (multiple data centers inside a Region)
    ↓
Edge Locations (CDN points for faster content delivery)(HYD,CHENNAI)

**ROAD MAP FOR AWS 2025**- 

**IAM (Identity Access Management)**
Security service used to control access to AWS.
Works by Authentication (who) + Authorization (what).
Used to manage users, groups, roles, and policies.
Ensures least-privilege and prevents unauthorized access.

**Key IAM Components:**
1. User - Individual identity to log in.
2. Group - Collection of users with same permissions.
3. Policy - JSON document defining allowed actions.
4. Role - Permissions for AWS services to access other services.
5. MFA - Extra layer of login security.

# S3 (Simple Storage Service)

**S3 is a cloud storage service in AWS used to store files.**
Think of S3 like **Google Drive**, but:
**More secure**
**More scalable**
**Used by applications, not just people**
You can store:
Images
Videos
Documents
Backups
Website files
Logs
# How S3 Works (Simple Explanation)
Inside S3, you store files in **Buckets**.
S3
 └── Bucket (Folder)
       └── Objects (Files)

**Bucket** = A storage container (like a drive/folder)
**Object** = Actual file stored inside bucket
# Why S3 is Used (Benefits)

| Feature | Meaning |
| --- | --- |
| Unlimited Storage | You can store any amount of data |
| Secure | You control who can access files |
| Cheap | Pay only for storage & data usage |
| Durable | AWS keeps multiple copies → Data never gets lost |
| Simple | Easy to upload/download files |
| Supports Website Hosting | You can host static websites directly from S3 |


# Key Terms to Remember

| Term | Simple Meaning |
| --- | --- |
| Bucket | Main storage container |
| Object | File stored in bucket |
| Region | Buckets are created in a specific geographic area (e.g., Mumbai region) |
| Versioning | Keeps old versions of files (useful to recover deleted data) |
| Bucket Policy | Rules that allow or deny access |
| Public Access Block | By default buckets are private for security |


### What is Amazon S3? (Perfect Answer)

**Amazon S3 (Simple Storage Service)** is a **cloud storage service** provided by AWS.
 It is used to **store and retrieve any type of data**, such as:
Images
Videos
Documents
Backups
Logs
Static website files

S3 stores data in **Buckets**, and inside the bucket, the **actual files are stored as Objects**.

S3 is a **flat storage system**, meaning there are **no real folders**; what look like folders are actually **prefixes** in object names.

### Security & Permissions

Each bucket has a **Bucket Policy**, which defines:
**Who can access the bucket**

**What actions they can perform** (upload, delete, read, list, etc.)

We can also control access using:
**ACLs (Access Control Lists)**
**IAM Roles**
**Public/Private Access Settings**
By default, **S3 buckets are private**, and we have to explicitly allow public access if needed.

### One-Liner Summary

S3 is a highly scalable, secure, and durable storage service in AWS that stores data as **Objects** in **Buckets**, with access controlled using **Bucket Policies** and IAM.

### In Simple, Why is S3 used?

It provides **secure, scalable, and cost-effective** storage.
 It can store **unlimited data**, and we only pay for how much we store and use.
 
### Bonus Point (If you want to impress)

S3 is designed for **99.999999999% (11 nines) durability**, which means your data is extremely safe.

**CREATING AN S3 BUCKET**

While creating an bucket it asks which one u want to create General purpose and Directory

**GENERAL PURPOSE BUCKET(NORMAL BUCKET):**

Stores any type of data
Data is stored across the multiple zones-> high durability
Access is slightly lower compared to the Directory access but still good.
Best for general usage.

**USED IN:**

Website content 
application logs
backups and archieves
media storage
we can think of it like a big ware house that stores every thing safely in it.

**DIRECTORY S3(EXPRESS ONE ZONE):**

This is a new **high-performance bucket type**.
It stores data in one availability zone.
Very low latency much more faster access.
Support high request rates like more read write 
Typically accessed from EC2 lambda in the region

**USED IN**

High speed analytics.
ML training.
Data processing systems.
Real time streaming applications.

**WE CAN THINK OF IT LIKE**
**A small but super fast local storage like keeping all the files in the SSD instead of remote ware house.**
**WHY DID AWS INTRODUCE THIS...?**

Because some work loads didn’t get global durability .
So they need extreme fast data access.
Example:
A real time analytics system reading data from millions of times per second.
A machine learning model training using the large data sets.
SOO for such cases s3 normal buckets are too slow so s3 directory is used.

**AWS RDS**

**Amazon RDS (Relational Database Service)** is a **managed database service** provided by AWS.
 It allows us to run **SQL-based relational databases** in the cloud **without manually installing, configuring, or maintaining them**.
RDS supports databases like:
**MySQL**
**PostgreSQL**
**MariaDB**
**Oracle**
**SQL Server**
**Amazon Aurora**

# How RDS Works (Simple Explanation)

Normally, if we setup a database on our own:
We must install database software
Maintain the server
Take backups
Apply security patches
Handle failover & recovery

→ **RDS does all of this automatically.**

 It manages the database **so you can focus on application logic**, not server maintenance.
 
# Where is Data Stored?

Data in RDS is stored in **DB Instances** (database servers) and managed storage.
RDS provides:
**Automatic Backups**
**Multi-AZ Replication (high availability)**
**Point-in-time recovery**
**Scaling storage and compute when needed**

# Why & When to Use RDS

### Use RDS when:
Your application needs **structured data** (tables, rows, columns)
You require **ACID transactions** (banking, finance, login systems)
Your system requires **high reliability and automatic backups**
### Examples of systems that use RDS:

| System | Why RDS fits |
| --- | --- |
| Login/Authentication System | Needs stored user records |
| E-commerce / Orders | Needs relational tables & transactions |
| Employee/Student Database | Structured tabular data |
| Billing / Accounting Systems | Requires data accuracy |


# When NOT to Use RDS
Don’t use RDS when:
You need **very fast, unstructured, flexible data storage**
 → Use **DynamoDB (NoSQL)** instead.
You need to store large files
 → Use **S3** instead.
# RDS → Simple Interview Summary
Amazon RDS is a managed relational database service that allows us to use SQL databases in the cloud without managing the underlying server. It stores structured data in tables, provides automatic backups, scaling, patching, and high availability. It is used when applications require reliable, transactional, and structured data storage.  
                                                             AWS RDS ARCHITECTURE
# AWS ENDPOINT, In AWS an end point is the URL that serves as an entry point for accessing an AWS web service.

# EC2
EC2-ELASTIC COMPUTE CLOUD, is a service thn  at provide an virtual servers

| Term | Meaning |
| --- | --- |
| AMI (Amazon Machine Image) | A template for your server (e.g., Ubuntu, Windows) |
| Instance | The actual virtual server you run |
| Instance Type | Hardware configuration (CPU + RAM size) |
| Key Pair | Used for secure login (SSH) |
| Security Group | Firewall rules (controls allowed ports) |
| EBS Volume | Hard disk/storage attached to instance |
| Elastic IP | Static IP address for your server |


We only pay the time the instance is running.
IN EC2 depending on the applications there are types of the instances
General
Compute optimised
storage
Accelerated based on the usage and the application it is used** **







**ABOUT EC2 DASHBOARD**

***INSTANCES:***

This shows how many instances are running or created or available.
These are the virtual servers you launched.
If it shows one that means u r currently having one virtual server running.
Running instances can cost money based depending on type and the usage.

***CAPACITY RESERVATIONS:***

Used to reserve compute capacity in advance.
Mostly used in enterprise when they want to ensure EC2 capacity is always available.

***ELASTIC IP:***

Genarally static ip had an same ip even after restarting the ec2 or instances.
Useful for production servers.

***KEY PAIRS:***

 This is your **.pem file** used for SSH login.
You have **1** key pair (example: jashwanth_239.pem).
Losing this means you **cannot SSH** into your instance.

***PLACEMENT GROUPS:***

Helps in controlling **how EC2 instances are placed** across hardware for performance or fault tolerance.
Used mostly for **high performance computing** or **low latency systems**.
You are **not using any** (common for beginners).

***SNAPSHOTS:***

Snapshots are **backups of your EBS volumes (disk)**.
If you create a snapshot, you can later restore or clone the volume.
You currently have **0** backups.

***AUTO SCALING GROUPS:***

Automatically **adds or removes EC2 instances** based on CPU usage or traffic.
Used in production to handle **scaling automatically**.
You have **0**, which is normal.

***DEDICATED HOSTS:***

A **physical server** dedicated only to your EC2 instances.
Very expensive, used in enterprise **compliance scenarios**.
You have **0**, which is normal.

***INSTANCES:***

This just shows the **total number of instances**, whether running or stopped.
You have **1 instance in total**.

***LOAD BALANCERS:***

Distributes traffic across multiple servers.
Used when you run **websites with multiple EC2 servers**.

***SECURITY GROUPS:***

These act like **firewalls** that control **what traffic can enter or leave** your EC2 instance.
You have **2**:
One default (created by AWS).
One likely created when launching the EC2 instance.

***VOLUMES:***

These are **storage disks attached to your EC2** (also called **EBS volumes**).
We have 1 volume attached to your instance (example: 8GB/10GB type storage).  


**SUMMARY TABLE**

| Term | Meaning | Your Count | Simple Use |
| --- | --- | --- | --- |
| Instances | Your servers | 1 | Where your code runs |
| Key Pairs | SSH login keys | 1 | Used to connect securely |
| Security Groups | Firewall rules | 2 | Controls allowed ports (like SSH/HTTP) |
| Volumes | Storage disks | 1 | The hard disk for your instance |
| Elastic IPs | Permanent Public IP | 0 | (Optional) stable IP |
| Snapshots | Backups of disks | 0 | Used for recovery |
| Auto Scaling Groups | Auto-add/remove EC2 | 0 | Used in heavy traffic |
| Load Balancers | Distribute traffic | 0 | Used for websites |
| Dedicated Hosts | Entire server reserved | 0 | Very expensive |
| Placement Groups | Special placement for performance | 0 | Only in advanced setups |








**AWS VPC CORE TOPICS**


**VPC**: An isolated private network in the AWS its like the private cloud inside the public cloud
In each VPC we can define IP address range and acts like an private address.
We can create an multiple VPC for different projects.

**SUBNETS**: Logical divisions inside the VPC.

To separate resources based on the access level or on purpose.
Public subnet: Accessible from the internet(via internet Gate way).
Private subnet: Not directly accessible for databases, backends.
Each subnet belong to the one available zone.
**ROUTE TABLES: Routing** an maps where the network traffic should go.
      example: public subnets route table sends to internet gate way.
                         private subnets route table sends to an NAT.
                         
**INTERNET GATEWAY**: A gate way that allows communication between your VPC and the internet.
Without this your VPC cannot connect to an internet.
Usually attached to the public subnet.


**EGRESS ONLY INTERNET** **GATEWAYS: Used** only for IPV6 traffic.
Allow instances in private subnet to send out bound traffic to the internet but does not receive the inbound traffic.

**DHCP OPTIONS SET**:

we can configure DNS and network options for instances launched in our own VPC.
Example:we can set custom DNS servers, domain names etc.
By default AWS provide but later we can change it.

**ELASTIC IP:**

**static public ip4 that addresses wont change**
attach to ec2 instances ,NAT gateways or load balancers that need a fixed public ip.
It is mainly use full for the stable connection.

**MANAGED PREFIX LISTS:** collection of CIDR blocks that can be re used in multiple places.
Its main purpose is simplifies the managing of the large networks.

**NAT NETWORK ADDRESS TRANSLATION GATEWAY:**

Allows **instances the private subnets to access the internet outbound. For updates and downloads without exposing the Ip.**

**VPC PEERING CONNECTIONS: connects** two vpcs together privately.
It allow communication between two resources in different VPCS with out using the internet.

**SECURITY GROUPS: These** are the virtual firewalls for the ec2 instances.
Control what type of network traffic should be allowed in or out of that instance.

**CUSTOMER GATEWAY:** A **Customer Gateway** is a **resource in AWS that represents your on-premises network device** (such as a router, firewall, or VPN appliance) that you use to connect your **local network to your AWS VPC** through a **VPN connection**. 

## Virtual Private Gateway (VGW)

A **Virtual Private Gateway** is the **AWS-side router** that connects your **VPC** to your **on-premises network** (your Customer Gateway) through a **VPN connection**.
### Simple meaning:
Think of it as **AWS’s router** inside your VPC.
Your **Customer Gateway** (your router) connects to this **Virtual Private Gateway** to create a **secure tunnel**.
### Example:
Your office router (Customer Gateway) in Bangalore
AWS Virtual Private Gateway in your AWS VPC (N. Virginia)
A VPN connection securely links both routers.

## VPN Connection

A **VPN Connection** (Virtual Private Network) is a **secure, encrypted tunnel** between your **on-premises network** and your **AWS VPC**.
### Simple meaning:
It’s like a **private cable through the internet** connecting your office to AWS.This tunnel is built between:

**Customer Gateway** → your router

**Virtual Private Gateway** → AWS’s router
All the traffic goes safely through this tunnel, no one else can see it.
### Example:
Your company’s data (like payroll system, internal apps) stays safe while being transferred to AWS, even though it travels over the public internet.

## VPC Peering
### Definition:
A **VPC Peering Connection** is a **private network link** between **two ****VPCs** so they can communicate **as if they are in**** the same ne****twork**.

### Simple meaning:
It’s like building a **private road** between two AWS buildings (VPCs).
### Example:
VPC 1 → hosts your **frontend web app**
VPC 2 → hosts your **database**
You create a **VPC Peering connection**, so your web app can talk directly to the database  securely, without using the internet.

**VPC Endpoints**

A **VPC Endpoint** lets you connect your **VPC to AWS services (like S3 or DynamoDB)** **without going through the public internet**.
**Simple meaning:**
It’s a **private shortcut** inside AWS to reach AWS services safely.
### Example:
Normally, your EC2 instance connects to S3 using the internet.
 But if you create a **VPC Endpoint**, your EC2 instance connects to S3 **internally** — no internet access needed.
### Two types:

| Type | Used For | Example |
| --- | --- | --- |
| Gateway Endpoint | S3, DynamoDB | Simple, free option |
| Interface Endpoint | Other AWS services (EC2, SNS, etc.) | Uses AWS Private Link |

## Instance Connect Endpoint
An **Instance Connect Endpoint** allows you to **connect (SSH)** to EC2 instances in **private subnets** securely, without needing a **public IP** or **bastion host**.
### Simple meaning:
It’s like a hidden door AWS gives you to log in to private EC2s directly.
Example:
You have a database EC2 in a private subnet with no internet access.
 You can open a terminal in AWS Console and use **EC2 Instance Connect Endpoint** to access it securely — without making it public.
 
## AWS Network Manager
**AWS Network Manager** helps you **monitor** and manage all your AWS networks**, like VPCs, VPNs, and Direct Connect links, in one dashboard.
### Simple meaning:
It’s like a **control room** that shows a map of all your AWS networks around the world.
### Example:
If your company has:
One VPC in India
Another VPC in the US
A VPN connection from your office to AWS
Network Manager shows them all visually in one place — helping you monitor and troubleshoot easily.

## What are Endpoint Services?
### Simple Definition:
**Endpoint Services** (also called **Private Link services**) let you **privately share your own application or service** with other AWS accounts **without using the public internet**.**Think of it like this:**
You have built a web service (like an API or an internal app) that you want other teams or clients to use,
 but you **don’t** want to make it public** on the internet.
So instead of using a **public IP or domain**,
 you create a **Private Endpoint Service** in AWS.
 Then others can connect to it **through AWS’s private network** — not through the internet.
## Example Story
Let’s say:
You (Jashwanth Tech) run a service that checks stock prices.
Your API is hosted on EC2 inside your **VPC**.
Another company (your partner) wants to use your API securely.
Normally, you’d make it public (using a public IP or load balancer),
 but that’s **less secure**.
Instead, you do this:
You create a **VPC Endpoint Service** (PrivateLink) for your API.
AWS gives your partner a **private link name**.
Your partner creates a **VPC Endpoint** (on their side) that connects to your service privately.
Now they can use your API **privately, securely, and faster** without internet.

## Real-Life Scenario

Imagine:
You own a company building (your VPC).
Your friend owns another building (their VPC).
You want your friend to use your service, but not through public roads.
So AWS builds a **private tunnel** between your two buildings.
 That tunnel = **Private Link (Endpoint Service)**.
A **Site-to-Site VPN** securely connects your **office network** to your **AWS VPC** over the internet — so they act like one private network.

**Bastion Host:**

**A Bastion Host is a secure gateway server used to access private resources inside a VPC.**
 It acts as a controlled entry point that allows administrators to connect into private EC2 instances that do not have public IPs.
 
# Why Bastion Host is Needed
In a secure AWS architecture:
Application servers
Database servers
Backend services
are kept in **private subnets** with **no public internet access**.
Because of this, we **cannot SSH directly** from the internet into those private machines.
So we place **one EC2 instance in a public subnet** and treat it as the *only allowed entry point.
 This is called a **Bastion Host** or **Jump Host**.
 
# How It Works (Short Explanation)

Admin Laptop → Bastion Host (Public Subnet) → Private EC2 (Private Subnet)
The bastion host has a **public IP**.
It allows **SSH only from trusted IPs** (admin machines).
Private EC2 allows SSH **only from the bastion’s security group**.
So all SSH traffic flows like this:

**Admin → Bastion → Private Server**

# Where It Is Used
A Bastion Host is typically used in:
Production environments
Secure back-end systems
Private database servers
Private application servers
Any environment where direct SSH from internet must be avoided
Teams use bastion hosts for:
Server maintenance
Log checking
Deploying applications
Debugging
Investigating issues

# Benefits
### Increases Security: Only one server is exposed to the internet, not all backend machines.
### Controlled Access: We restrict SSH on private EC2 to bastion only, not public internet.
### Audit and Monitoring: All admin access can be logged and monitored from a single point.
### Follows Industry Best Practices: Enterprises never allow direct SSH access to private servers.

How We Use It (Your scenario)
We:
Created a public EC2 (bastion) in public subnet
SSH’ed into it using our key
From inside that bastion EC2, we connected to the private EC2 using its private IP
Private EC2 allowed SSH only from the bastion host
This is a **standard AWS security pattern**.

# Difference Between Bastion and NAT (Common Interview point)

| Feature | Bastion Host | NAT Gateway |
| --- | --- | --- |
| Purpose | Admin login to private servers | Give private servers internet access |
| Used by | Humans (SSH) | Servers/Applications |
| Traffic direction | INBOUND → Private EC2 | OUTBOUND → Internet |
| Requires SSH keys | Yes | No |

Both are used together in secure architectures, but they serve different roles.
# One-Line Summary
“A Bastion Host is a hardened public EC2 instance used as a secure entry point to access private servers inside a VPC. It centralizes and restricts SSH access so that backend servers remain invisible and protected from the internet.”

**HOW TO SET UP AN BASTION HOST IN AWS:**

## 1. Create VPC and Subnets

**Create VPC**

Go to **VPC → Create VPC**
Name: my-vpc
IPv4 CIDR: 10.0.0.0/16

**Create Public Subnet**

Go to **Subnets → Create subnet**
VPC: my-vpc
Name: public-subnet
CIDR: 10.0.1.0/24
After creating →
Select public-subnet → Actions → Edit subnet settings
Enable **Auto-assign public IPv4**

**Create Private Subnet**
Subnets → Create subnet
VPC: my-vpc
Name: private-subnet
CIDR: 10.0.2.0/24
**Do NOT** enable public IP.

## 2. Internet Gateway and Route Tables
**Create and Attach Internet Gateway**
VPC → Internet Gateways → Create IGW
Name: my-igw
After creating → **Attach to VPC** → select my-vpc

**Public Route Table**
VPC → Route Tables → Create route table
Name: public-rt
VPC: my-vpc
Open public-rt → Routes → Edit routes
Add route:
Destination: 0.0.0.0/0
Target: **Internet Gateway (my-igw)**
Then public-rt → Subnet associations → Edit
Select **public-subnet** and save

**Private Route Table**
Create another route table:
Name: private-rt
VPC: my-vpc
private-rt → Subnet associations → Edit
Select **private-subnet**
(No internet route added here – stays private.)

## 3. Security Groups
**Security Group for Bastion (Public EC2)**
EC2 → Security Groups → Create
Name: sg-bastion
VPC: my-vpc
Inbound rule:
Type: SSH
Port: 22
Source: **My IP** (your laptop IP)

**Security Group for Private EC2**
Create SG: sg-private
VPC: my-vpc
Inbound rule:
Type: SSH
Port: 22
Source: **sg-bastion**
 (Choose “Custom” → select security group sg-bastion)
 
## 4. Launch EC2 Instances
**Launch Bastion Host (Public EC2)**
EC2 → Launch instance
Name: bastion-host
AMI: Ubuntu (or Amazon Linux)
Instance type: t2.micro (for testing)
Key pair: adarsh_384.pem (or your key)
Network:
VPC: my-vpc
Subnet: public-subnet
Auto-assign public IP: **Enable**
Security Group: **sg-bastion**
Launch

**Launch Private EC2**
EC2 → Launch instance
Name: private-server
AMI: Ubuntu (or Amazon Linux)
Instance type: t2.micro
Key pair: **same** as bastion (adarsh_384.pem)
Network:
VPC: my-vpc
Subnet: private-subnet
Auto-assign public IP: **Disable**
Security Group: **sg-private**
Launch

## 5. SSH Flow: Laptop → Bastion → Private EC2
### 5.1. SSH from Laptop to Bastion (Public EC2)
From your **WSL2 terminal**:
ssh -i ~/adarsh_384.pem ubuntu@<BASTION_PUBLIC_IP>
(Use ec2-user@... if Amazon Linux.)

### 5.2. Copy Key to Bastion
From **WSL2 (not inside EC2)**, run:
scp -i ~/adarsh_384.pem ~/adarsh_384.pem ubuntu@<BASTION_PUBLIC_IP>:/home/ubuntu/
SSH into bastion again:
ssh -i ~/adarsh_384.pem ubuntu@<BASTION_PUBLIC_IP>
Inside bastion, fix key permissions:
chmod 400 adarsh_384.pem

### 5.3. SSH from Bastion to Private EC2
In AWS console → EC2 → select **private EC2** → note its **Private IP** (example: 10.0.2.65)
From **inside bastion**, connect:

For Ubuntu private EC2:
ssh -i adarsh_384.pem 
For Amazon Linux private EC2:
ssh -i adarsh_384.pem 
Now we are inside **private EC2 via bastion**

# SET UP AN APPLICATION LOAD BALANCER (ALB) IN AWS
# Prerequisites
Before creating a Load Balancer, you must already have:
### VPC
Example: my-vpc
### At least TWO Public Subnets
ALB requires a minimum of **2 Availability Zones**
public-subnet-1 (e.g., 10.0.1.0/24)
public-subnet-2 (e.g., 10.0.3.0/24)

### EC2 Instances
Running a web server (Apache/Nginx) on:
Port 80
In *public* or *private* subnets behind the ALB

### Security Groups
ALB SG → Allow HTTP (80) from anywhere
EC2 SG → Allow HTTP (80) **from ALB SG only**

### Target Group Creation
Target type: Instances
 Protocol: HTTP
 Port: 80
 
***Why Target Group?***
Load Balancer cannot send requests directly to EC2 instances. It sends traffic only to a target group. And this target group maps traffic to actual instances. 

**1. Create VPC and Subnets**
**Create VPC**
Go to **VPC → Create VPC**
Name: test-vpc
IPv4 CIDR: 12.0.0.0/16
Create

## Create Public Subnets (ALB needs at least 2)
### Public Subnet 1
VPC → Subnets → Create subnet
VPC: test-vpc
Name: test-public-subnet-1a
CIDR: 10.0.1.0/24
After creation →
Select test-public-subnet-a
Actions → Edit subnet settings
Enable: **Auto-assign public IPv4**

### Public Subnet 2
Subnets → Create subnet
VPC: test-vpc
Name: test-public-subnet-b
CIDR: 10.0.2.0/24
Enable auto-assign public IP (same steps as above)

# 2. Internet Gateway and Route Tables
## Create and Attach Internet Gateway
VPC → Internet Gateways → Create
Name: test-igw
Attach IGW → select test-vpc
## Public Route Table
VPC → Route Tables → Create
Name: test-rt
VPC: test-vpc
Open test-rt → Routes → Edit routes
Add route:
Destination: 0.0.0.0/0
Target: **Internet Gateway (test-igw)**
Associate Subnets → select:
 test-public-subnet-a
 test-public-subnet-b
 
# 3. Security Groups
## Security Group for EC2 Instances (Web Servers)
EC2 → Security Groups → Create
Name: sg-web
VPC: test-vpc
Inbound rules:
Type: HTTP
 Port: 80
 Source: **Anywhere (0.0.0.0/0)** for testing
Outbound: allow all
**(Later, restrict HTTP to ALB SG only.)**

## Security Group for Load Balancer
EC2 → Security Groups → Create
Name: sg-alb
VPC: test-vpc
Inbound rule:
HTTP (80) → 0.0.0.0/0
Outbound: allow all

# 4. Launch EC2 Instances (Backend Servers)
Launch **at least 2 EC2 instances** (one in each public subnet).

### EC2 Setup:
AMI: Ubuntu
Instance type: t3.micro
VPC: test-vpc
Subnets:
EC2-1 → test-public-subnet-a
EC2-2 → test-public-subnet-b
Auto-assign public IP: Enabled
Security Group: sg-web
Install Apache/nginx** (user data):**
#!/bin/bash
 apt update -y
 apt install -y apache2
systemctl enable apache2
 systemctl start apache2
cat > /var/www/html/index.html <<'HTML'
<!doctype html>
<html>
<head><title>My Free-Tier EC2 Page</title></head>
<body>
<h1>Hello from my Ubuntu EC2</h1>
<p>Launched at: $(date -u)</p>
</body>
</html>
 HTML
chown -R www-data:www-data /var/www/html

# 5. Create Target Group
EC2 → Load Balancing → Target Groups → Create
Target type: **Instance**
Name: test-ec2-tg
Protocol: HTTP
Port: 80
VPC: my-vpc
Health check path: /
Register targets:
 add both EC2 instances
Create Target Group

# 6. Create the Application Load Balancer
### Step 1:
EC2 → Load Balancers → **Create Load Balancer**
Choose **Application Load Balancer**
### 6.1 Basic Configuration
**Name:** test-alb
**Scheme:** Internet-facing
**IP address type:** IPv4

### 6.2 Network Mapping
Select VPC:
test-vpc
Select **TWO public subnets**:
test-public-subnet-1
test-public-subnet-2

### 6.3 Security Groups
Select:
**sg-alb(Or create new one with only HTTP port 80 open)**

### 6.4 Listeners & Routing
You will see:
**Listener: HTTP:80**
Under “Default action” select:
**Forward to:** test-ec2-tg

### 6.5 Create Load Balancer
Click **Create Load Balancer**
Wait until:
**Status = Active**

# 7.Register EC2 Instances to Target Group

### Step 1:
Go to **Target Groups → test-ec2-tg → Targets → Register**

### Step 2:
Select your EC2 instances and register them.

### Step 3:
Wait until:
**Health status = Healthy (Green)**

# 8.Test the Load Balancer
Copy the ALB DNS name:
Example:
 test-alb-123456.eu-north-1.elb.amazonaws.com
Open it in browser → You should see your webpage.
# Final Architecture Summary
User → Load Balancer (test-alb)
                  ↓
         Target Group (test-ec2-tg)
                  ↓
        Healthy EC2 instances (Apache/Nginx)
                  ↓
           VPC (test-vpc)
              Public Subnets

**SET UP AUTO SCALING GROUPS (ASG)** using:
A **baseline EC2 instance**
A **custom AMI**
A **Launch Template**
A **Target Group**
**AUTO SCALING GROUPS**

# PREREQUISITES
Before creating an Auto Scaling Group, you must have:
### VPC
Example:
 test-vpc
### At Least TWO Subnets - ASG distributes instances across AZs.
test-public-subnet-1a
test-public-subnet-1b
(Subnets can be public OR private.)

### Baseline EC2 Instance
This is your **Master EC2** that has:
Apache/nginx installed
Your web application running
Verified via browser

### AMI Created from Baseline Instance
Name example:
 test-web-ami
 
### Target Group (Instance target type)
Example:
 test-ec2-tg
**DO NOT register any EC2 instances manually**
 (ASG will register automatically → explained later.)
 
### Load Balancer Created
Example:
 test-alb
 Listener: HTTP:80 → Forward to target group
 
# 2. Create the Baseline EC2 Instance
### Launch EC2
AMI: Ubuntu
Instance type: t3.micro
VPC: test-vpc
Subnet: test-public-subnet-1a
Auto-assign public IP: Enabled
Security Group: allow HTTP (80) + SSH

### User Data to install Apache
#!/bin/bash
apt update -y
apt install -y apache2
systemctl enable apache2
systemctl start apache2

cat > /var/www/html/index.html <<'HTML'
<h1>Hello from Baseline EC2</h1>
HTML

### Verify
Open browser: http://<EC2-Public-IP>

# 3. Create AMI from the Baseline EC2
EC2 → Instances → Select your instance →
 **Actions → Image and templates → Create Image**
Name: test-web-ami
Description: baseline image
Leave “No reboot” unchecked
Create Image
Wait until AMI status = **Available**

# 4. Create Launch Template (VERY IMPORTANT)
EC2 → Launch Templates → Create launch template
### Name: test-launch-template
### AMI: Choose: test-web-ami
### Instance Type: t3.micro
### Key Pair
Choose your key
### Security Group
Select EC2 security group:
HTTP (80) allowed (later will restrict to ALB SG)
SSH allowed from your IP
### User Data
**Leave EMPTY**
 Because AMI already contains Apache + files.
### Create Template

# 5. Create Target Group (Instance Target Type)
EC2 → Target Groups → Create target group
Target Type: **Instances**
Name: test-ec2-tg
Protocol: HTTP
Port: 80
VPC: test-vpc
Health check path: /
NOTE: **Do NOT register targets manually.**
 Because Auto Scaling will automatically add/remove instances.
### Why you should NOT register manually?
Because:
ASG will create new EC2 instances
ASG automatically adds them to the target group
Manual registration breaks automation
When ASG scales out → new EC2s won’t be registered
When ASG scales in → old EC2s stay incorrectly registered
So **ASG must control registration**, not you.

# 6. Create the Application Load Balancer (ALB)
EC2 → Load Balancers → Create → **Application Load Balancer**
### Basic Config
Name: test-alb
Scheme: Internet-facing
IP type: IPv4
### Network Mapping
VPC: test-vpc
Select both public subnets:
test-public-subnet-1a, test-public-subnet-1b
### Security Group
Choose: sg-alb (Allow HTTP:80 from anywhere)
### Listeners
Listener: **HTTP:80**
 Default action → **Forward to test-ec2-tg**
### Create
Status will become **Active**.

# 7. Create the Auto Scaling Group (ASG)
EC2 → Auto Scaling → Auto Scaling Groups → Create

### Step 1: Choose launch template
Select: test-launch-template

### Step 2: Group details
Name: test-asg

### Step 3: VPC settings Select VPC: test-vpc
Select both subnets: test-public-subnet-1a, test-public-subnet-1b 

# 8. Attach Load Balancer to ASG
In the load balancing section:
### Choose:
**Attach to an existing load balancer**
### Select:
Target Group: test-ec2-tg
ALB: test-alb
### Enable:
**Elastic Load Balancer health checks**
Set **Health Check Grace Period**:
 60 seconds
 
# 9. Configure Group Size
**Desired capacity:** 2
**Minimum:** 2 (Desired capacity = Minimum capacity)
**Maximum:** 3
ASG will automatically create 2 EC2 instances from your AMI.

# 10. Configure Scaling Policies (Optional)
Choose:
 Target Tracking Policy
 Metric: **Average CPU Utilization**
 Target value: **50%**
ASG now scales out/in automatically.

# 11. Create ASG
Click: **Create Auto Scaling Group**

# 12. Validate Everything
### Check EC2 Instances
EC2 → Instances
 You should see:
2 EC2 instances created by ASG
Names auto-tagged
Each in different subnets
**Check Target Group Health**
EC2 → Target Groups → test-ec2-tg → Targets
Instance status should become **Healthy** (green)
### Check ALB
Open:
 http://test-alb-xxxxxx.region.elb.amazonaws.com
We should be able to see:
"Hello from Baseline EC2"
 and each refresh may hit a different EC2.

**Amazon DNS Service: ROUTE53**
What is Amazon Route53?
Amazon Route 53 is a highly available, scalable Domain Name System (DNS) web service provided by AWS.
It performs **3 main functions**:
DNS Service: Translates domain names (example.com) → IP addresses (192.168.1.1)
Domain Registration: **You can buy/register domain names through Route 53.**
**Health Checks & Failover: Monitors your website/app and can redirect traffic if a server fails.**
In simple words:
Route 53 is AWS’s DNS + Domain Registration + Traffic Routing service.
# Why Use Route 53?
### 1. Highly available (global DNS infrastructure): Your domain stays reachable worldwide with no downtime.
### 2. Scalable: Handles millions of queries automatically.
### 3. Fast DNS resolution: Uses AWS’s global network of DNS servers.
### 4. Supports routing to:
EC2
ALB/NLB
S3 static websites
CloudFront
API Gateway
On-premise servers
 and many more.
**5. Health checks**: **Route 53 checks if your app is healthy and redirects traffic if it fails.**
***6. Advanced Routing Policies***
Supports intelligent traffic routing like:
Weighted
Latency-based
Failover
Geolocation
Geoproximity
Multivalue
Working:


When a user enters a website:

Route 53 performs DNS resolution:
User sends request → DNS resolver (ISP)
Resolver queries Route 53 authoritative name servers
Route 53 returns the IP address of the resource (EC2, ALB, S3, etc.)
User’s browser connects to that IP
Website loads
Route 53 is the service that tells the world **“this domain belongs to this IP.”**
Important DNS Record Types in Route 53

| Record Type | Purpose |
| --- | --- |
| A Record | Maps domain → IPv4 address |
| AAAA Record | Maps domain → IPv6 address |
| CNAME | Maps domain → another domain (alias) |
| NS Record | Lists the Name Servers for the hosted zone |
| MX Record | Mail servers for the domain (used for email) |
| TXT Record | Arbitrary text (used for verification, SPF, DKIM) |
| Alias Record | AWS-specific, maps domain to AWS services like ALB, S3, CloudFront |

### Alias Record is special because:
Free (no DNS lookup charges)
Faster
Supports AWS resources
Can point root domain (example.com) → ALB/S3 directly

# Routing Policies in Route 53 (6 Types)
These decide **how Route 53 directs user traffic**.

## 1. Simple Routing
Maps domain → single IP/instance
No health checks
Basic use-case
**Example:**
 example.com → 1 EC2 instance
 
## 2. Weighted Routing
Split traffic by weight (percentage)
**Example:**
70% traffic → server A
30% traffic → server B
Used for:
Testing new versions
Gradual deployment (blue/green)

## 3. Latency-Based Routing
Routes users to the server with **lowest latency** (fastest region)
**Example:**
India user → ap-south-1
US user → us-east-1

## 4. Failover Routing
Primary → Secondary routing.
If primary fails → send traffic to backup.
**Used for:**
 Disaster recovery
 High availability
 
## 5. Geolocation Routing
Routes traffic based on the **geographic location** of the user.
Example:
India users → Indian server
Europe users → EU server
Useful for:
Compliance
Localization
Regional pricing

## 6. Geoproximity Routing
Routes traffic based on:
Geographic location **AND**
**Bias (weight shift)** between regions.
Allows increasing/decreasing traffic to specific regions.
Used for:
Advanced global traffic control
Disaster recovery load shifting

**AWS CloudWatch**
AWS CloudWatch is a monitoring and observability service that collects metrics, logs, alarms, and events from AWS resources such as EC2, RDS, Lambda, and custom applications.
It helps users monitor system performance, detect issues early, and automate actions.

# CloudWatch Metrics (Theory)
**Metrics** are numeric data points that CloudWatch collects from AWS services every minute or every 5 minutes.
For EC2, the main built-in metrics include:
CPUUtilization
NetworkIn / NetworkOut
DiskReadOps / DiskWriteOps
StatusCheckFailed (System + Instance)
CloudWatch automatically collects these metrics when an EC2 instance is running.

# Practical – Viewing EC2 CloudWatch Metrics

### Step 1: Open CloudWatch
Go to:
AWS Console → CloudWatch → Metrics

### Step 2: Select EC2
Choose: EC2 → Per-Instance Metrics

**Step 3: Select your instance**: **Click your instance ID (e.g., i-1234567890).**
**Step 4: View graphs**
You will see real-time graphs such as: CPU utilization %, Network packets, Disk operations.
You can change: Time range (5 min, 1 hour, 1 day), Statistic (Average, Maximum, Minimum).

# CloudWatch Alarms
A **CloudWatch Alarm** continuously monitors a metric and performs an action when the metric crosses a threshold.
Example:
If CPU > 70% for 2 minutes → Trigger alarm
Actions include:
Sending an email (via SNS)
Triggering Auto Scaling
Stopping/Rebooting EC2

# Practical – Creating Alarm for EC2 CPU

### Step 1: Go to Alarms
CloudWatch → Alarms → Create Alarm

### Step 2: Select Metric
EC2 → Per-Instance → CPUUtilization
Click Select Metric

### Step 3: Set Conditions
Threshold Type: Static
Condition: Greater than 70%
Period: 5 minutes

### Step 4: Create SNS Notification
If you don’t have SNS topic:
Choose “Create New SNS Topic”
Topic name: ec2-alarm-topic
Email Subscription: enter your email
Open your email → Confirm subscription.

### Step 5: Name and Create Alarm
Alarm Name: **High-CPU-Alarm**
Click **Create Alarm**.

# Simple Notification Service (SNS)
SNS is a messaging service used by CloudWatch to send notifications.
In alarms:
CloudWatch triggers SNS → SNS sends email
You receive alerts for high CPU, failures, etc.
SNS supports:
Email
SMS
Lambda triggers
Webhooks
# Practical – SNS Email Notification Test
Go to SNS → Topics
Select your topic (ec2-alarm-topic)
Click Publish Message
Enter subject + message
Click Publish
Here, we should immediately receive an email.  And confirmation check is must.

# CloudWatch Dashboard

A **Dashboard** is a customizable monitoring view where you can add graphs, alarms, and widgets to monitor resources in one place.
Dashboards help teams visualize performance.
# Practical - Creating CloudWatch Dashboard

### Step 1: Create Dashboard
CloudWatch → Dashboards → Create Dashboard
Enter: ec2-monitoring-dashboard

### Step 2: Add Widgets
Choose Line Graph → Metrics
Add metrics like:
CPUUtilization
NetworkIn
NetworkOut
StatusCheckFailed

**Step 3: Add Alarm Widget**
Add existing alarm: High CPU- alarm
### Step 4: Save Dashboard
Now your dashboard shows:
 Real-time CPU usage
 Network traffic
 System health
 Active alarms
This is extremely useful for operations and monitoring.

**AWS – Lambda**
AWS Lambda = Serverless Computing Servce
**Meaning:**
You upload your code -> AWS runs it only when needed.
We don’t manage services -> we pay only for the execution part.

***Why Lambda is Used?***
Lambda is perfect to use when u need to run the code.
Therefore, only when an event happens for a short time without running servers 24/7.
Lambda receives event details as json.
A Lambda function is a piece of code that runs when an event happens.
In Lambda, the basic unit is always function. Everything in Lambda revolves around function.
Lambda functionn is the smallest and main component
It contains:
     Your Code, Configuration, Triggers, Memory n Time out, Permissions, Environment    variables.
Lambda is a service, inside that we just see a function.
In real, this is how it reflects:


**Copy Files from S3 to EC2**
**Step-1:** **sudo apt update
              sudo apt install awscli
Ubuntu EC2 instances do not come with awscli installed. And the awscli v2 package is not available in the Ubuntu app repository.
AWS officially recommends installing awscli v2 using the downlodable installer. This is why we use curl.
The apt repository only contains awscli v1 which is outdated and sometimes not available. 
awscli v2  ->  which supports master AWS features.
curl “link” -o “saveas” in the current directory. 
**Step-2:** Unzip the file.
              sudo apt install unzip –y
              unzip awscliv2.zip
**Step-3:** Install AWS CLI (awscli)
              sudo ./aws/install
The command runs the awscli installer script. It copies all the awcli v2files into the correct system directories creates the aws command and sets up the reuired systemlinks, so we can use awscli globally outside the machine.
**Step-4****: **Verify installation
              aws –version
Runs the installed aws binary and prints it’s version and run time tools/details.**
**Step-5:** Copy files from S3 to EC2 
               aws s3 cp s3://<Your Bucket Name>/<filename>  /home/ubuntu
Here, aws -> awscli tool
          s3 -> Using S3 service commands
          cp -> Action = Copy
          s3://..  -> Source (S3 Object)
          path/  ->  Destination local EC2 folder.

**Example:** 
If we want to display all the files in particular object, we use
                      aws s3 ls s3://<Your bucket name> / 
To copy an entire folder
                        aws s3 cp s3:<bucket name>/<folder name>  /home/ubuntu
To Copy entire bucket
                         aws s3 cp s3://<bucket name>  /home/ubuntu/  --recursive

**Attach new EBS Volume to your current EC2**

# Step-1: Attach New EBS Volume to EC2
Go to AWS → EC2 → Volumes
Click **Create Volume**
Choose: 
Volume Type: gp3 (or gp2)
Size: any (ex: 18 GB)
Availability Zone: must match your EC2 AZ
Create volume
Select the volume → **Attach Volume**
Attach to your instance

# Step-2: Verify New Volume in EC2
On your EC2:
lsblk 
We should see:
nvme1n1   18G   disk    (UNMOUNTED)

# Step-3: Create a Partition
sudo fdisk /dev/nvme1n1
Inside fdisk type:
n (new partition)
p  (primary)1    (partition number)
ENTER (default first sector)
ENTER (default last sector)
w  (write changes)
Output will show a new partition:
/dev/nvme1n1p1

**Step-4: Format the New Partition**
Format as ext4:
sudo mkfs.ext4 /dev/nvme1n1p1

**Step**-5: Create a Mount Directory**
sudo mkdir /mnt/myvolume


**Step-6:Mount the Volume**
sudo mount /dev/nvme1n1p1 /mnt/myvolume

# Step-7: Verify the Mount
df -h
You should see:
/dev/nvme1n1p1   18G   /mnt/myvolume

# Step-8: Create Test File (optional)
sudo touch /mnt/myvolume/test.txt/dev/nvme1n1

**CRON JOBS**
A Cron job works like **scheduling a task** in any system.
It is mainly used for **backups,** **system maintenance**, etc.
Cron's job works on both local systems as well as cloud services.
To run the **cron** job in AWS, we have to use **AWS Lambda**. In AWS Lambda, we set up the **functions** and **schedule a time to run** them.
**In a Definition:** We can say it as, Cron jobs are scheduled tasks that enable the automated execution of operations by running at predetermined times or intervals.
***Syntax to schedule a cronjob***: ***minute hour day-of-month month day-of-week year**

**Example in Usage Scenario:**
Amazon CloudWatch is used in AWS Lambda to notify users whenever any changes occur.
If we add any resources or make any changes, then CloudWatch will send me a message as a notification. Now, in this we can set up anything, such as if our server resource will automatically scale up, then run the Lambda function. In CloudWatch, we can also schedule the time. If we want to shut down all the instances at 6 PM and start at 8 AM, then we can do so.

**Key Points to Remember:**
# Lambda Function:
AWS Lambda is a **serverless compute service** that runs code **without managing servers**.
Used for automating tasks like **starting and stopping EC2 instances quickly**.
Implemented using **Python + Boto3**, following clean code practices and proper error handling.
Requires correctly configured **IAM roles** with granular permissions for safe execution.

# Benefits of Using Lambda
**No server management,** AWS handles infrastructure.
**Automatic scaling** based on workload.
**Cost-effective,** pay only for execution time.

# IAM Role for Lambda (EC2 Management)
Create an IAM role that allows Lambda to manage EC2 resources.
Attach permissions such as **AmazonEC2FullAccess** (or more restrictive, least-privilege policies).
Attach this IAM role to the Lambda function for proper access and integration.

# Lambda Permissions
Add IAM policies that grant only the required EC2 actions.
Follow **least privilege** principle to restrict unnecessary access.
Ensure the IAM role is linked to the Lambda function so it can execute smoothly inside AWS.
**Create and test the script that captures file details from the mounted volume and uploads them to S3, for every 6hrs of Monday-Friday using a cron job.**
**Step-1:** **Bash Script**
#! /bin/bash   -> Shebang line, ths tells Lnx to run this script using the bash shell.
OUTPUT = “/home/ubuntu/volumes_files.txt”
echo = “ “ > $OUTPUT
Here,
           OUTPUT = “....”  -> Creates a variable named OUTPUT
           Stores the files path where the script will save results.
            Echo “ “   -> prints an empty line
             >  -> overwrites the file
This creates the fresh empty file at /home/ubuntu/ 
If the file already exists, it clears the old content.

**Step-2:** **Check if volume is mounted**
VOLUME = “/dev/nvme1n1”
It stores the device name of EBS volume in a variable named VOLUME.
We attached this volume from AWS.

if mount | grep  –q “$VOLUME”; then 
     Here,  mount displays all mounted filesystems. And | -> pipe sends o/p of mount into grep
                 grep  -q “$VOLUME”  -> searches for the device name quietly
                -q  = quiet mode -> does not show up o/p only returns success/failure.
                  if ..... then -> find grep finds mounted volume it’s true, else false. 

 MOUNT_DIR = $(mount | grep “$VOLUME” | awk {$print $3}
         Here, mount | grep “$VOLUME finds line where your EBS volume is mounted
                     awk ‘{print $3}’ -> splits the line into columns
                      $(...) command substitution -> stores the result into variable
{MOUNT_DIR}, now contains the mount path
else 
    echo “No Volume is mounted.” >> $OUTPUT
     exit 1
fi
**Step-3:** Check if file exists
FILES = $(ls –1 “$MOUNT_DIR” 2>/dev/nv11)
        Here,  ls –1 “$MOUNT_DIR”  -> Lists files inside the mount directory, -1 = one file per line
                    2>/dev/nv11 -> Hides error msgs (e.g. Permission Denied)
                   
Result is stored in the variable files.
if [ -z “$FILES]; then
    Here, -z checks if the string is empty or not
               so this means, if no files are present = True, else if EXISTS = False
echo “No files found in the mounted volume.” -> Output
     writes msg in the result file.
else
    echo “Files inside the mounted volume:”  >> $OUTPUT
ls –l “$MOUNT_DIR” >> $OUTPUT
fi

**Amazon SES**

Amazon SES is a **cloud-based email sending service** used to send **transactional, notification, and marketing emails**.
Supports sending emails via **SMTP**, **AWS SDKs**, or **AWS CLI**.
Designed for **high deliverability, scalability, and reliability**.

## Key Features
Send emails using **verified email addresses or domains**.
Supports **HTML and plain-text emails**.
Handles **bounces, complaints, and delivery notifications**.
Integrates with **SNS, SQS, Lambda, and CloudWatch**.
Built-in **DKIM, SPF, and DMARC** support for authentication.

**SSE Workflow:**
1. Verify **email address** or **domain**
2. Configure **email authentication **(SPF/DKIM)
3. Create and emails using **SMTP** or SDK.
4. Track **Delivery, Bounce and Complaint events.**

**SES Sandbox Mode:**
It is a restricted mode that limits how SES can be used.

**Restrictions:**
- You can send an email to only verify email address.
- Daily sending limit is low
- Used mainly for testing
- 
**Production Mode:**
- In this, the daily mail limit is increased.
- We can email any email address 
- Suitable for real applications.

**How to move to Production Mode?**
- Request in SES Console -->  Production mode
- AWS approves if ur use case is valid.
- 
**Identity Verification**
SES must verify who you are before you send emails.
We can Verify: Email Identity / Domain Identity. 
                                |                                   |
                          for Testing                 used for Production systems

***Why Verify Identity?*** (Relay servers needs identity)
- Prevents fake email sending.
- Protects email reputation.
- Required by AWS before sending any email.

**DKIM Setup (Domain Keys Identified Mail):**
It ensures that your email is legitimate and not modified by anyone.
***Why DKIM is Important?***
- Improves deliverability
- Reduces the chances of going to spam.
- Adds digital signature to emails. 

**How it Works?**
AWS gives you 3 CNAME records,
You add them to ROUTE53.
Your domain becomes DKIM-enabled.
This increases the trust from Gmail, Outlook etc.


## Feedback Handling - Bounce, Complaint & Delivery Handling
SES detects:
**Bounces:** email not delivered.
**Complaints:** recipient marks email as spam.
**Deliveries:** email successfully delivered.
Events can be published on **Amazon SNS** for processing.

** Configuration Sets**
These sets help you track and organize your email sending.
Configuration set is a feature in SES that tells Amazon SES: “***How should we track this email after we send it?***”

SES only sends the email. No tracking, No analysis, No event information. 
But,  ***WITH CONFIGURATION SET***, SES can ***track delivery, complaint, open, click, reject events***. 
-SES can send these events to SNS, CloudWatch, S3 etc.

**Sender Reputation**
Sender reputation in AWS SES is a ***score that measures how trustworthy***your email sending is.
- It determines whether your emails go to inbox/spam.
- It is improved by reducing bounces, reducing spam complaints, using DKIM and sending clean emails to valid users.

**Factors affecting sender reputation:**
Bounce rate – when the email address does not exist or cannot receive email.
Complaint rate, sending to old/invalid emails, sudden increase in email volume.
Bad content/Spamming mail.


| A High Sender Reputation | A Low Sender Reputation |
| --- | --- |
| Ur emails go into Inbox | Emails land in spam |
| AWS allows you to send more emails | AWS reduces your sending limit |
| Your domain is considered trusted | AWS may temporarily block SES sending |


**Dedicated IP:** A dedicated IP address is an IP address that is permanently assigned to you, and remains available for your use only. It allows you to use a dedicated server to handle only your traffic and no one else's.

How to create a sample mail template in SES **using AWS CLI?:**
**Step-1:** nano template.json (Paste ur json code here)
**Step-2:** aws ses create-template \
  --cli-input-json file://template.json
	SES no longer supports sending template emails directly from the console. Template  emails can only be sent using SendTemplatedEmail API or cli. So, we used AWS CLI to test the template instead of the test mail option.
So, to run it automatically, 1) we go with AWS Lambda
			         2) Create an API endpoint using API gateway + Lambda
                                                 3) We use Python Script/ Backend Server
**Step-3:** aws ses send-templated-email \
  --source "sender@example.com" \
  --destination "ToAddresses=receiver@example.com" \
  --template "TemplateName" \
  --template-data '{"name":"User"}'
### Quick check command: aws ses list-templates

AWS RDS
RDB: Relational db organizes data into structured tables(relations) with rows(records) and columns(attributes) defining relationship b/w thses.
RDS fully manages and makes it easy to setup, operate and scale relational dbs in AWS.
RDS automates provisioning, patching, backups, failure detection and recovery.
RDS should ALWAYS be in PRIVATE SUBNET.
Supported Engines: Amazon Aurora(MySQL and PostgreSQL compatible), MySQL, PostgreSQL, MariaDB, Oracle etc.
Deployment Types: 
1. Single AZ: One db instance (lower cost)
2. Multi AZ: Synchronous standby in another AZ for high availability and automatic failure.
DB Instance: Your actual DB server (managed by AWS)
DB Subnet Group: A group of subnets across at least two availability zones.
Without this, RDS will NOT launch.  
Security Groups: Controls who can connect (ec2, bastion host etc.)
Read Replicas: Asynchronous replicas used to scale read traffic or for cross region reads
Backups & Recovery: 1) Automated backup        2) DB Snapshots
Scaling: 1) Horizontal     2) Vertical             3) Storage
Security: IAM DB authentication, AWS KMS etc.
Operational Monitoring: Cloudwatch, Performance insights for SQL etc.

**RDS Setup:**
Before creating a RDS Database, there are few prerequisites. They are as follows.
VPC already created or you can use default one also.

**At least 2 subnets in different AZs** (for DB subnet group)
Security Group allowing MySQL (3306) from EC2 SG

**EC2 instance** in same VPC (client machine)
### Step 1: Create a DB Subnet Group
Required so RDS runs in multiple AZs
AWS Console → **RDS**

**Subnet groups** → Create DB subnet group
Name: my-rds-subnet-group
VPC: select your VPC
Select **at least 2 subnets from different AZs**
Create


**Step-2: Create Security Group for RDS**
EC2 → **Security Groups** → Create
Name: rds-sg
VPC: same as EC2
Inbound rule:
Type: **MySQL/Aurora**
Port: **3306**
Source: **EC2 Security Group ID** (recommended)
Save

### Step 3: Create RDS Instance
RDS → **Create database**
Choose **Standard create**
Engine: **MySQL**
Template: **Free tier**
DB identifier: my-rds-db
Master username: admin
Password: set password
Connectivity:
VPC: same as EC2
Subnet group: my-rds-subnet-group
Public access: **No** (recommended)
Security group: rds-sg
Create database
Wait until status becomes **Available**

**Step 4: Launch EC2 Instance (Client)**
EC2 → Launch instance
AMI: **Ubuntu**
VPC: same as RDS
Subnet: public subnet
Security group:
Allow **SSH (22)** from your IP
Launch

### Step 5: Install MySQL Client on EC2
sudo apt update
sudo apt install mysql-client –y

### Step 6: Connect EC2 to RDS
Copy **RDS Endpoint**
RDS → Databases → Connectivity & security
Run:
mysql -h <rds-endpoint> -u admin -p
Enter DB password
Connected successfully

