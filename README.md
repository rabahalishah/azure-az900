# azure-az900
Evolution of Cloud Hosting:
- Dedicated servers (One physical machine is responsible for only one business and can run single application)
- VPS virtual private servers (One physical machine is responsible for only one business. This machine is virtualized into sub-machines and can run more than one applications)
- shared hosting (One physical machine can run multiple-businesses, multiple applications such as Hostinger etc.)
- cloud hosting (Multiple physical machines acts as one system. The system is abstracted into multiple cloud services)

Common cloud services:
- Compute
- Networking
- Storage
- Databases

Types of cloud computing:
- On premise: (Customer is completely responsible)
- IaaS (Cloud Service provider CSP is more responsible as compared to user)
- Paas (Cloud Service provider CSP is more responsible as compared to user)
- Saas (Cloud Service provider CSP is completely responsible)


CAPEX vs OPEX
Capital Expenditure, spending money upfornt on pyhical infrastructure like server costs, network costs, hardware cost, IT personal costs etc. In CAPEX you have to guess upfront what you plan to spend.

OPEX Operational Expenditure, this cost is associated with the cost of operation of the resources as physical datacenters cost has shifted to the service provider. With OPEX you can try a product or service without investing in equipment.

High Elasticity:
Ability to increase or decrease your capacity based on the current demand of traffic.
Horizontal scaling (Adding more resource to your system)
Scaling Out - Add more servers of the same size.
Scaling In - Removing more servers of the same size.

Highly Fault tolerance:
Ability to ensure that there is no single point of failure. Preventing the chance of failure. For this we have two databases one is primary and other one is secondary. In case of any fail overs the secondary database will act as the primary one until the actual primary do not get fixed.


RPO vs RTO:
Recovery point objective is the maximum acceptable amount of data loss after an unplanned data loss incident

Recovery time objective is the maximum mount of acceptable down time your business can tolerate without incurring the significant business loss.

Disaster recovery options:
- Backup and Restore (Time taking but cheap)
- Pilot Light (Data is replicated to another regions, faster but expensive)
- Warm Standby (Scaled down copy of your infrastructure running ready to scale up)
- Multi-site Active: Scaled up copy of your infrastrtucture. (Real time but very expensive almost double the amount of your current infrastructure)

# Types of Cloud Computing:
Dedicated servers
VMs
Containers
Functions


Domains in Cloud:
Availability zones (Data centers) are the combination of fault domain (they make sure that system do not have a single point of failure) and update domain (this make sure that your server/machine do not have a down time while updating it.)
You select the value of Fault Domain and Update Domain.
Imagine Fault domain as the number of columns of racks. 

# Types of Virtual Machines on Azure
Azure provides a variety of virtual machine (VM) offerings to cater to different workload requirements. Each VM type is designed with specific hardware configurations to meet diverse performance and scalability needs.

## General Purpose VMs
## Example: Standard_D2s_v3

**Description:** General-purpose VMs are well-balanced machines suitable for a variety of workloads. They offer a good balance of CPU-to-memory ratio and are suitable for development, testing, and small to medium-sized databases.

Use Case: Hosting websites, lightweight applications, or development and testing environments.

Compute Optimized VMs
Example: Standard_F2s_v2

Description: Compute optimized VMs are designed for compute-intensive workloads that require high CPU power. They provide a high CPU-to-memory ratio, making them suitable for data analytics and computational tasks.

Use Case: Batch processing, gaming applications, and other CPU-intensive workloads.

Memory Optimized VMs
Example: Standard_E16s_v3

Description: Memory optimized VMs are tailored for memory-intensive applications. They provide a high memory-to-CPU ratio, making them suitable for databases, in-memory caching, and analytics.

Use Case: Running large databases, in-memory caching, and analytics applications.

Storage Optimized VMs
Example: Standard_L8s_v2

Description: Storage optimized VMs are designed for workloads that require high storage throughput and I/O performance. They provide high local disk throughput, making them suitable for big data and large databases.

Use Case: Big data applications, data warehousing, and large-scale databases.

GPU VMs
Example: Standard_NC6s_v3

Description: GPU (Graphics Processing Unit) VMs are equipped with powerful graphics processors, suitable for graphics-intensive applications and parallel processing tasks.

Use Case: Machine learning, graphics rendering, and simulations that require GPU acceleration.

High-Performance Compute VMs
Example: Standard_H16r

Description: High-Performance Compute VMs are designed for demanding, parallel processing and high-performance computing (HPC) applications.

Use Case: Simulations, modeling, and scenarios that require massive parallel processing.

Burstable VMs
Example: B1s

Description: Burstable VMs provide a baseline level of CPU performance with the ability to burst above the baseline for a certain period. They are cost-effective for workloads with varying CPU usage.

Use Case: Development and testing environments, small websites, and applications with variable workloads.
Imagine Update domain as the number of servers in on column of racks.

Regions are the set of availability zones (data centers)

# How to deploy a jenkins application on Azure VM or How to connect to Azure VM using SSH:
Following are the steps that you have to follow to deploy an application on azure VM:
- First create a virtual machine by logging into Azure portal and create a virtual machine. For now you can follow the following settings for your learning purposes:
  - Subscription Free trial
  - Resource group (Create a resource group and select that one)
  - Give your VM a name (first-vm)
  - Select a region nearest to your location (I am using East US)
  - Select Zone 1
  - Security type do not change any thing
  - Image (I selected Ubuntu you can select any OS)
  - VM architecture x64
  - **Run with Azure Spot Discount ()Buy unused Azure compute capacity at deep discounts)**: Despite using several VM techniques there must be a space that is idle or left in Azure VMs. So, Azure gives us an option that you can also use those free spaces. But we cannot guarantee that these VM will be available all the time. So you can use them for testing purposes but not for production.
  - Size: Select size according to your need. I am selecting Standard_B1s that is general purpose VM.
  - Authentication type : SSH
  - Give your user name (this will be used for accessing your VM. In my case I am giving it as: azureuser)
  - SSH Public key source: Generate new key pair
  - key pair name: anyname
  - now click on create VM
- once the machine is created you will be asked to download SSH key file this is .pem file
- Now using this file you can access to your VM using SSH
# Commands to use for connecting to your VM using SSH:
- Once the VM is deployed go to your home > Vitual Machines > select your VM and go to its overview.
- Copy the public IP address of your VM
- open your Linux terminal/ Git bash terminal etc. And run the following commands:
  ```bash
  // general command
  ssh -i path_to_your_ssh_key_pem_file vm_username@public_ip_address_of_your_vm

  // in my case:
  ssh -i /users/dowloads/first-vm_key.pem azureuser@172.178.84.132
  ```
  **NOTE:** Here you will get error of permission denied so you have to change the permission to 600 for your pem key file
  ```bash
  
  sudo chmod 600 /users/dowloads/first-vm_key.pem

  ```

  now run again:
  ```bash
  ssh -i /users/dowloads/first-vm_key.pem azureuser@172.178.84.132
  ```

  Now you are successfully logged into your Azure VM. Now you can install your jenkins application here.

  ## How to install jenkins application in your azure VM:

### Install Jenkins.

Pre-Requisites:
 - Java (JDK)

### Run the below commands to install Java and Jenkins

Install Java

```
sudo apt update
sudo apt install openjdk-17-jre
```

Verify Java is Installed

```
java -version
```

Now, you can proceed with installing Jenkins

```
curl -fsSL https://pkg.jenkins.io/debian/jenkins.io-2023.key | sudo tee \
  /usr/share/keyrings/jenkins-keyring.asc > /dev/null
echo deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc] \
  https://pkg.jenkins.io/debian binary/ | sudo tee \
  /etc/apt/sources.list.d/jenkins.list > /dev/null
sudo apt-get update
sudo apt-get install jenkins
```

**Note: ** By default, Jenkins will not be accessible to the external world due to the inbound traffic restriction by AWS. Open port 8080 in the inbound traffic rules as show below.

# How to enable/manage traffic to your Azure VM Network:
- Go to your azure VM that you just have create.
- Go to Network settings and scroll down.
- You will see Rules.
### **Azure Virtual Machine (VM) Network Rules Overview**

When you create a Virtual Machine (VM) in Azure, it’s connected to a virtual network. To control the traffic (data) that comes in and goes out of your VM, Azure uses **network rules**. These rules help secure your VM by specifying what kind of traffic is allowed or blocked.

There are two main types of network rules:

1. **Inbound Rules**
2. **Outbound Rules**

Let’s break these down:

---

### **1. Inbound Rules**

**Inbound rules** control the traffic **coming into** your VM from the internet or other networks.

**Imagine:** Your VM is like a house. Inbound rules are like the doors and windows of your house that decide who can come inside.

#### **Key Components in Inbound Rules: Source and Destination**

When creating inbound rules, you often see **source** and **destination**. Here’s what they mean:

- **Source:** This specifies **where the incoming traffic is coming from**.
  - **Example:** Think of the source as the address of the person trying to visit your house. It could be:
    - **Any** (anyone can try to come in)
    - **Specific IP addresses** (only certain people from specific places can come)
    - **Azure services** (like allowing traffic from other Azure resources)

- **Destination:** This specifies **where the traffic is going to**.
  - **Example:** Since it’s inbound, the destination is usually your VM itself. It defines which part of your VM (like a specific port or service) the traffic is trying to reach.
    - **Ports:** Think of ports as different rooms or functions in your house (e.g., port 80 for a web server, port 22 for SSH access).

**Example Inbound Rule:**
- **Allow** HTTP traffic (port 80) from **any** source to your VM.
  - This means anyone on the internet can access your website hosted on the VM.

---

### **2. Outbound Rules**

**Outbound rules** control the traffic **leaving** your VM to the internet or other networks.

**Imagine:** Outbound rules are like the doors and windows that decide who can leave your house and where they can go.

#### **Key Points about Outbound Rules:**

- They determine what your VM is allowed to **connect to**.
- For example, allowing your VM to access a database server or to update software from the internet.
- You can restrict it so the VM can only communicate with certain services or external addresses.

**Example Outbound Rule:**
- **Allow** your VM to access the internet on port 443 (HTTPS).
  - This lets your VM securely communicate with web services and download updates.

---
- Now click on Create Port Rule and set the following properties as:
   - source to any
   -  destination to any
   -  destination port range to 8080 (on which our jenkins application is running),
   -  Service to Custom
   -  Protocol Any (cuz we do not know which protocol our users are using)
   -  priority: 400 
   - Click on add
 
Now go to your browser and go to http://172.178.84.132:8080 (Now you will be able to access your jenkins which is running inside azure VM)

# What is Priority number in Azure port rules:
Great question! Understanding **priority** in Azure network rules is essential for managing how traffic is handled by your Virtual Machine (VM). Let’s break it down in an easy-to-understand way.

### **What is Priority in Azure Network Rules?**

**Priority** determines the **order** in which Azure evaluates your network rules (both inbound and outbound) to decide whether to allow or deny specific types of traffic to and from your VM.

- **Priority Number:** Each rule is assigned a **priority number**.
- **Order of Evaluation:** Azure processes rules **from the lowest priority number to the highest**.
  - **Lower Numbers = Higher Priority:** A rule with priority **100** is checked **before** a rule with priority **400**.
  - **Higher Numbers = Lower Priority:** A rule with priority **400** is checked **after** rules with priority **100**, **200**, etc.

### **Why is Priority Important?**

Imagine you have multiple rules that might affect the same type of traffic. Priority ensures that the most important rules are evaluated first.

#### **Example Scenario:**

1. **Rule A:** Allow **SSH (port 22)** from **specific IPs** with priority **200**.
2. **Rule B:** Deny **all SSH (port 22)** traffic with priority **400**.

**How It Works:**

- **Incoming SSH Request from an Allowed IP:**
  - **Rule A** (priority 200) is checked first.
  - It **allows** the traffic since the IP is on the allowed list.
  - **Rule B** is **not** evaluated because the traffic has already been allowed.

- **Incoming SSH Request from a Disallowed IP:**
  - **Rule A** is checked first but **doesn’t apply** since the IP isn’t allowed.
  - **Rule B** (priority 400) is then checked and **denies** the traffic.

### **What Does Priority 400 Mean?**

When someone sets a rule with **priority 400**, here’s what it signifies:

- **Placement in Order:** The rule will be evaluated **after** all rules with priorities **lower than 400** (e.g., 100, 200, 300).
- **Common Use:** Often used for more general or less critical rules because lower priority numbers are reserved for specific and important rules.
- **Example Use Cases:**
  - **Priority 100:** Allow web traffic (port 80/443) from anywhere.
  - **Priority 200:** Allow SSH from specific IPs.
  - **Priority 400:** Deny all other SSH attempts from any other IPs.

### **How to Choose the Right Priority**
 **Determine Importance:**
   - **High Importance:** Rules that need to be evaluated first (e.g., allowing access from trusted sources) should have **lower priority numbers**.
   - **Low Importance:** General deny rules or less critical allow rules can have **higher priority numbers**.

# Understanding Azure Virtual Network (VNET):
Before getting jump into Azure Vnet lets first understand what is the need of vnet and what happen when we do not have vnet.
For example: There devops engineers from NIKE and PUMA both request for VM in US-East region and availability zone 1.
Since, both companies are requesting the VM in the same Data center. So now if hacker get access to NIKE's data then it would be easier to access the PUMA data as well. Cause both application are residing in same Data center and have same network. Just to solve this problem and to manage the network traffic coming inside and going outside from applications and VMs. Azure introduced the concept of VNET.

### How we will define the size of our VNET?
The size of a network is defined by its number of ip-addresses.
**You can define the size of your VNET using CIDR (Class inter-domain routing). For example for CIDR /16 standard you will have 65536 IP addresses**

Using VNET you can define your n-number of Vnets in your VM. So, In short **VNET is basically a logical isolation of network inside a physical network in a physical server**

In Vnet you can also create your subnets and what is the benefit of dividing the VNET into subnet is that you can separate your application, databases and websites from each other and manage their network traffic separately.

In azure we have NSG (Network security group) and ASG (Application Security group). We manage the traffic using these two features. NSG allows you to define the rules of flowing traffic from one subnet to other and where as ASG allows you to defines the network traffic flow rules from one particular application in a particular subnet to the other applications of the other subnets.

**Using the NSG and ASG you can define the rules that who can access which application from outside (AKA source) and what can go out from VNET. And Even it allows you to define network traffic rules from one subnet to the other subnets.**

**For example:** You have a VNET which is divided into two subnets. One subnet contains website and business logic application and the other contains Databases. So using NSG we can define the rule that no body from the internet can access the business login application and databases. Only request from the website to the business logic application or databases is allowed. And using ASG we can even define the rule the any particular application among several applications inside the same subnet can only send request to the other network particular application. Means ASG provide us define rules at application level. 

**NOTE:** In industry we use both, NSG + ASG

**For example in above section where we deployed jenkins on our VM. We set the network rule for Jenkins only.**
# Azure Networking

## Virtual Network Definition:

A Virtual Network (VNet) in Azure is a logically isolated network that securely connects Azure resources and extends on-premises networks. Key features include:

- **Isolation**: VNets provide isolation at the network level for segmenting resources and controlling traffic.

- **Subnetting**: Divide a VNet into subnets for resource organization and traffic control.

- **Address Space**: VNets have an address space defined using CIDR notation, determining the IP address range.

## Subnets, CIDR

### Subnets

Subnets are subdivisions of a Virtual Network, allowing for better organization and traffic management.

### CIDR (Classless Inter-Domain Routing)

CIDR notation represents IP addresses and their routing prefix, specifying the range of IP addresses for a network.

## Routes and Route Tables

### Routes

Routes dictate how network traffic is directed, specifying the destination and next hop.

### Route Tables

Route Tables are collections of routes associated with subnets, enabling custom routing rules.

## Network Security Groups (NSGs)

NSGs are fundamental for Azure's network security, allowing filtering of inbound and outbound traffic. Key aspects include:

- **Rules**: NSGs define allowed or denied traffic based on source, destination, port, and protocol.

- **Default Rules**: NSGs have default rules for controlling traffic within the Virtual Network and between subnets.

- **Association**: NSGs can be associated with subnets or individual network interfaces.

## Application Security Groups (ASGs)

ASGs group Azure virtual machines based on application requirements, simplifying network security:

- **Simplification**: ASGs allow defining rules based on application roles instead of individual IP addresses.

# Azure Advance Networking concepts:
Following are the concepts that we are gonna cover in this section:
- Azure App Gate way & WAF (Web Application Firewall)
- Azure Load Balancer
- Azure DNS
- Azure Firewall
- Virtual Network Peering and VNET Gateway
- VPN Gateway


Ok, so I am gonna compare the terminologies of azure VNET with a Housing Scheme.
Assume VNET as a Housing Scheme. And Blocks inside it as subnets. Since the housing scheme is secure. So, the walls will acts as a firewall. And the gate of the scheme will be act as Gateway. Assume there is a security Guard at the gate that will make sure that the person/guest that is coming inside the premisses of the housing scheme must reach to the right destination. That will be done by routing system. And assume each block of scheme that are subnets in Azure VNET have a security guard that will make sure that the person reaches to the right home. And in VNET that will be called as NSG (Network Security Group) that will make sure that the traffic reaches the right resource.

Okay so now you have a basic understanding of Azure VNET architecture. Lets get deeper inside it.
Assume you have a 3 tier application that contains front-end, backend and database. You deployed all this in an Azure VNET with three respective subnets and each subnet contains the copy of the project/instance that will be in different availability zone. so to manage traffic. Like how much traffic should be go to this instance and how much to that, this will be done by load balancer. 

### Load balancers in Azure
In azure we have two types of load balancer:
- App gateway (L-7):This deals with the external HTTP traffic coming from outside the VNET such as your URL and so on.
- Load balancer/ Azure Load balancer (L-4): This applies at the application level inside the VNET. Request coming from front-end to the backend will be deal by App gateway as it will decide to which instance of backend this request should send.

Below is the picture of the whole flow:
![image](https://github.com/user-attachments/assets/83dfbbf2-ddf8-4b10-b439-e3fa5720f0fb)



### Summary of the above figure:
Now let say, a user wants to access your application let say the domain of your application is abc.com. Now you will map your IP of your load balancer and website domain. So that whenever a user try to access your web app by its domain then the traffic should go to load balancer first that will decide, and then forward the request to the respective application.

When a user makes a request, it does by the name of the domain. That requests goes to your IPS (Internet service provider) that sends to the DNS and then request get forwarded to the Azure's DNS and then it get redirected to app-gateway and so on.

**Virtual Network Peering**:
In virtual network peering concept, we conncet two virtual networks. Let say in NIKE they have two projects, NIKE billing and NIKE Store. And they have their separate VNETs then using the using this Virtual network peering we can conncet them. **For this we must have admin permissions in both VNETs** So that we are gonna change the route tables so the traffic can flow form one VNET to the other.

This can also be done using VNET Gateway.

### VPN Gateway:
Here let say your industry have a hybrid cloud model. Like they have their own datacenter in their office and they also use Azure VMs. So in this case if  they want to connect their on premisses Data center with Azure VM then they can do it using VPN Gateway.

#  Azure VNet, Firewall, NSG and Bastion Practical Implementation and Step by Step Guide:
Okay so first we are gonna do is that, we are gonna create a VNET and in that VNET are gonna create 4 subnets.
- Firewall subnet
- Firewall manager
- Bastion Subnet
- Nginx subnet (any project)

And then our goal is to flow the traffic from the external user to the application via firewall not directly. Here Bastion is a new thing that is explained below:
## When Should You Use Bastion?
When you don’t want to expose public IPs: Bastion allows you to keep your VM private without exposing it to the internet.
When you need secure access to VMs in private networks: Bastion is perfect for accessing VMs that reside in private subnets or VNETs.
When you need browser-based access: For quick, web-based access to VMs without needing SSH/RDP clients installed locally.
When security is a top priority: With Bastion, you minimize security risks by not exposing SSH ports to the public internet.

**NOTE: IF VM is created using Bastion enabled then it will not have any Public IP. But only private IP. So we are gonna access our VM then?**
ANSWER: 
How it works:
**No Public IP:**The VM operates without a public IP address, meaning it is not directly exposed to the public internet. This significantly enhances security because there’s no direct path for external access.

**Private IP Address:** The VM will have a private IP address within the virtual network (VNET) that it is deployed in. This private IP is only accessible within the Azure infrastructure (and other connected networks like VPNs or ExpressRoute).

**Access via Azure Bastion:** Instead of connecting to the VM using its public IP (like in a traditional SSH or RDP setup), you use Azure Bastion to connect. Bastion provides a secure, browser-based connection directly from the Azure portal without the need for a public IP. Bastion handles the communication internally within Azure.

**Bastion is the Gateway:** Bastion acts as a gateway or intermediary between your local machine and the VM. You connect through Bastion (via the Azure portal), and it routes your connection securely to the VM's private IP using Azure's internal network.

![image](https://github.com/user-attachments/assets/57db8dfc-754d-4e65-a213-c20bd27c65ee)

Here round circle is the VNET and blocks are subnets. You might be wondering that where is the subnet of firewall manager. So dont worry we do not create it manually. But Azure create it automatically when we create a firewall. 

## STEP BY STEP PROCEDURE to install nginx in a VM created using bastion and firewall implementation:
- First create a resource group if you haven't already. Go to resource group, set name, and select region and click on create.
- Now create a VNET. Search VNET. Select resource group that you just have created. Set the name, **In the security tab don't forget to enable Bastion and Azure Firewall. Cause we want Firewall**.
- For firewall policy click on create a new policy.
- Now in your IP address tab. Select the size of your VNET (size means number of IP addresses you want)
- Now click on create VNET and wait unitl it the VNET is not created.
- Now create a VM that is very simple, but in the networking tab you have to select your VNET which you just have created above. For subnet select default. Cuz we want to put our application in the default subnet. (you can also change the name from default to anything you want)
- Select Public IP to none (cuz we want this VM to be deployed behind the firewall and in the private subnet that is nginx-subnet in our case)
- now click on create VM and download the pem file.
- Now its time to get access to this VM. But do not have Public IP but private IP so we have to access it using Bastion. So go to your VM and look for connect option, go to Bastion. Select authentication-type as select SSH file from local machine and give username. and click on connect.
- Now a new tab in your browser will be open that will contain a terminal. Here you have access to your VM and you can install what you want. So here we are gonna install nginx and gonna set a static HTML page.
- Following are that you have to follow once you have get access to your VM using bastion:
   ```bash
   sudo su -
   apt-update
   apt-get install nginx -y
   cd /var/www/html
   nano index.html
   <h1>I have learnt Azure Networking.</h1>
   systemctl restart nginx
   curl localhost:80 //this will return you your HTML. Actually here you are verifying your Deployement.
   ```
**NOTE**: Uptil now, we have created our VNET, subnets and successfully connected to our VM using Bastion. But how users are gonna interact with our HTML page that we just have deployed in our VM? So for that we have to configure our Firewall.

## Configuring Firewall to get access to the app:
- Go to firewall manager > in security tab, go to Azure Firewalls > and look for firewall policy and click on it.
- After that go to DNAT (Network access translation) rules. Click on add rules collection. Give it a name. and set priority to 100. Keep rest as it is and click on add.
- Now click on add rule and follow the steps carefully:
  - Select the collection of the rules that you just have created.
  - Give this rule a name
  - In Source IP address put your laptops IPv4 only (source IP is what, who wants to access this application. In this case only I want to bypass firewall so I'll use only my IP)
  - In destination IP address you have to put the Public IP address of your Firewall not private IP. Cause we are flowing our traffic from user to firewall and firewall will forward that request to the subnet and application.
  - Select protocol to TCP
  - Destination Port to any number. I am using 4000. (since we have only one application right now in our VNET so we can use any port for this. But there will be different ports for other applications.)
  - In Translated IP address is the Private IP address of your VM in which nginx application is deployed. You can get that by going into your VM and network settings.
  - and in translated port select the port as the port on which your nginx/or your application is running. in my case its 80.
  - Now save it. And your are done.
- Here we have give the source as our laptop (means only our laptop is allow to bypass firewall) and set the destination as our firewall and translated IP as our VM private IP. So now when the user from my laptop is gonna hit request on the browser on Public IP address of the firewall and the port 4000 or whatever I should be able to see. My HTML page.

**Congrates you are done. You have successfully created a VNET that have firewall and only your laptop is allowed to bypass firewall.**
