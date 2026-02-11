## Basic Active Directory, Networking, and File Sharing

<div style="background: #f6f8fa; padding: 20px; border-radius: 6px; border: 1px solid #e1e4e8; max-width: 800px;">
  <img width="757" 
       alt="Active Directory Setup Steps" 
       src="https://github.com/user-attachments/assets/473de028-a937-4015-89aa-b294ea1b368e" />
</div>


### Hardware Requirements
- VMware Workstation Pro: 1.2 GB Disk Space and 64-bit OS
- Windows Server 2025: 4 GB Memory, 60 GB Disk Space
- Windows 11 Pro: 4 GB Memory, 64 GB Disk Space (Minimum)
- Virtualization: VT-x (Intel) or AMD-V enabled in BIOS

### Lab Setup
1. Set Up a Virtual Lab Environment:
- Use virtualization software like VMware Workstation Pro, Hyper-V, or VirtualBox.
- Install Windows Server 2022 (or 2025) and configure it as a Domain Controller.
- Install a couple of Windows client machines (Windows 11 Pro) and join them to the domain.

### Activity 
1. Rename the server hostname
2. Install Active Directory Tools and Promote as Domain Controller.
3. Creating Organizational Units (OUs)

Objective: Organize the domain environment using a structured Organizational Unit (OU) design that separates users, groups, and devices.

- Launch Management Tools:
  - Open Active Directory Users and Computers (ADUC) from the Tools menu in Server Manager.

- Create the Root Container:
  - Right-click your domain (e.g., lab.local) and select New > Organizational Unit.
  - Name it LAB_Assets. (Creating a single root OU keeps your custom lab objects separate from the default Windows containers).

- Establish Object-Based Tiers:
  - Right-click your new LAB_Assets OU and create three nested OUs:
  - Accounts (For all human and service users)
  - Endpoints (For all physical and virtual hardware)
  - Security_Groups (For managing permissions)

- Refine the Endpoint Hierarchy:
  - Inside the Endpoints OU, create two more nested OUs to separate policies:
    - Workstations (Target for Windows 11 client GPOs)
    - Servers (Target for application or file server GPOs)

Departmental User Organization:
- Inside the Accounts OU, create nested OUs for your specific teams:
  - IT_Admins
  - Finance
  - Human_Resources



## 2. Creating User Accounts - Identity & Access Management

- Locate the Target Container:
  - Select your LAB_Assets > Accounts OU.
  - Select the specific department (e.g., IT_Admins or Finance).

- Provision a New User:
  - Right-click the departmental OU and select New > User.

- Define Identity Attributes:
 - Follow a standard naming convention for the User Logon Name (e.g., First Initial + Last Name).
   - Example: * First Name: Marcus
   - Last Name: Wright
   - User Logon Name: mwright

- Security & Password Configuration:
  - Assign a temporary password that meets the domain's complexity requirements.
  - Settings: Select "User must change password at next logon" (best practice) or "Password never expires" (common for lab service accounts).

- Assign Professional Metadata:
  - Once the user is created, right-click the account and select Properties.
  - In the Description field, add their specific role (e.g., Senior Systems Engineer).
  - Under the Organization tab, fill in the Job Title and Department.

Batch Creation:

Repeat the process to populate the Finance and Human_Resources OUs with at least two users each to test permissions later.

Why this version is better:
Metadata Focus: Real-world AD management relies heavily on the Organization and Description tabs for filtering and automation.

Naming Conventions: Instead of just "jdoe," it explains the logic (First Initial + Last Name).

Security Standards: It mentions the "change password at next logon" setting, which is a fundamental security step in any corporate environment.






































