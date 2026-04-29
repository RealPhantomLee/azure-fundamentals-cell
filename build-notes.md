# Cloud Storm Cells
## Cell A — Azure Fundamentals

Author: RealPhantomLee Tucker  
Environment: Kali Linux (local)  
Cloud Provider: Microsoft Azure  
Region: West US 2  

Objective: Establish a reproducible Azure foundation by creating a structured network baseline (resource group, VNet, segmented subnets, NSG controls) with cost governance in place, then deploy and validate compute in a controlled way.

---

## 1. Purpose of This Build

Cell A is the Azure foundation layer for the Cloud Storm Cells project. The purpose of this cell is to build the baseline cloud infrastructure that future cells will rely on, including cloud security monitoring, detection engineering, and governance automation.

This build was completed in phases to match real-world workflow:

- **Phase 1 (Completed):** subscription + budget guardrails + network architecture baseline (RG, VNet, segmented subnets, NSGs attached)
- **Phase 2 (Completed):** VM deployment + access/validation + compute lifecycle discipline (stop/deallocate)
- **Phase 3 (Later Cells):** logging, SIEM forwarding, detections, policies, and automation

Primary goals of Cell A:

- Create and organize Azure resources under a clean lifecycle boundary (Resource Group)
- Design a scalable private network (VNet) with enterprise-style segmentation (subnets)
- Establish security enforcement points using NSGs (subnet-level access control)
- Implement early cost governance (budget) to avoid surprise charges
- Document architecture and decisions so the environment can be recreated by any reader

---

## 2. Subscription Information (Completed)

Subscription Type: **Azure Free Trial**  
Region Selected: **West US 2**

Design Decision: A budget was configured immediately after subscription activation to enforce cost awareness from the beginning of the project. Even in training environments, cost governance is a core cloud engineering habit, and this build intentionally treats cost controls as a first-class requirement.

---

## 3. Resource Group Creation (Completed)

Resource Group Name: **rg-cloudcells-a**  
Region: **West US 2**

Design Decision: A dedicated resource group was created to provide a clean boundary for all assets belonging to Cloud Storm Cells — Cell A. In Azure, the resource group is an organizational and lifecycle container. It simplifies cost tracking, teardown, access control patterns (RBAC), and audit clarity.

---

## 4. Networking Configuration (Phase 1 Completed)

Virtual Network Name: **vnet-cloudcells-a**  
Address Space: **10.0.0.0/16**

Design Rationale: A **/16** address space provides long-term flexibility while remaining simple. This enables subnet expansion later without redesigning the VNet.

### 4.1 Subnet Segmentation (Completed)

Subnet A (Workstations / Workloads):
- Subnet Name: **snet-workstations**
- Subnet Range: **10.0.1.0/24**
- Intended use: workstation VMs (Windows/Linux), workload nodes, testing systems

Subnet B (Management):
- Subnet Name: **snet-mgmt**
- Subnet Range: **10.0.2.0/24**
- Intended use: management/jump host, admin VM, monitoring node, controlled access layer

Design Rationale: Segmenting management pathways from workload systems improves clarity and creates a foundation for least-privilege routing, future firewall insertion, and controlled admin ingress.

### 4.2 Network Security Groups (NSGs) (Completed)

NSG for Workstations subnet:
- NSG Name: **nsg-cloudcells-workstations**
- Associated subnet: **snet-workstations**
- Custom rules: **None added during Cell A**
- Posture: baseline deny inbound unless explicitly allowed (Azure default behavior)

NSG for Management subnet:
- NSG Name: **nsg-cloudcells-mgmt**
- Associated subnet: **snet-mgmt**
- Custom rules: **None added during Cell A**
- Posture: baseline deny inbound unless explicitly allowed (Azure default behavior)

Security Decision: Custom inbound rules were intentionally not expanded for this cell. Validation focused on correct placement, private addressing, and safe lifecycle handling (deallocate).

---

## 5. Virtual Machine Deployment (Phase 2 Completed)

VM Name: **vm-mgmt-ubuntu-01**  
OS: **Ubuntu 24.04**  
Architecture: **x64**  
Size: **Standard_B2ats_v2 (2 vCPU, 1 GiB)**  
Subnet: **snet-mgmt**  
Private IP: **10.0.2.4**

Notes:
- The initially desired size (**Standard_B1s**) was unavailable in West US 2 for this subscription, so **Standard_B2ats_v2** was selected as an available low-cost alternative.
- SSH authentication is the intended access method. During validation, **Serial Console** was used to confirm system state and networking without depending on public ingress.

---

## 6. Public Exposure Review (Current State)

Cell A goal: **Security-first baseline** with minimal exposure.

Observed behavior during VM creation:
- A public IP may be assigned by default depending on the chosen options during VM creation.
- Even when a public IP exists as a resource, inbound access should remain restricted unless explicitly allowed via NSG rules.

Current intent for Cell A:
- Treat the environment as **private-first**.
- Avoid widening inbound rules.
- Use Serial Console / future Bastion / controlled access patterns in later cells.

---

## 7. Cost Governance (Completed)

Budget Alert Configured: **Yes (screenshots captured)**  
Auto-shutdown: **Not configured in this cell** (VM was manually deallocated after validation)

Cost Discipline Applied:
- VM was **Stopped (deallocated)** after validation to halt compute billing.
- Networking and storage objects may continue to exist, but compute charges are stopped while deallocated.

---

## 8. Deployment Validation (Completed)

### Validation Steps Performed

1. Confirmed **Serial Console** access
2. Executed on VM:
   - `whoami`
   - `hostname`
   - `ip a | grep 10.0`
3. Verified:
   - Username: **azureuser**
   - Hostname: **vm-mgmt-ubuntu-01**
   - Private IP: **10.0.2.4** assigned within **snet-mgmt** range
4. Confirmed VM placed in the expected VNet/subnet
5. Deallocated VM to enforce cost discipline

### Final State

VM Status: **Stopped (deallocated)**  
Network: **Private subnet placement confirmed**  
Cost posture: **Compute charges halted**

Cell A validation complete.

---

## 9. Future Expansion

This foundation will be used in future Cloud Storm Cells for:

- Logging and monitoring integration (Azure Monitor / Log Analytics or hybrid)
- Honeypot or instrumented VM deployment
- Security event forwarding to SIEM tooling (Azure-native or hybrid)
- Detection engineering (alert rules and test triggers)
- Governance automation (policies, RBAC, cost controls, lifecycle discipline)

Cell A establishes the foundational network architecture and governance posture required for the rest of the Cloud Storm Cells project.
