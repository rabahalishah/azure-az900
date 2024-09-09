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
