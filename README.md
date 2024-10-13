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
