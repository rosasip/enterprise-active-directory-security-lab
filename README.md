### Basic Active Directory, Networking, and File Sharing Project

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
 Objective: Provision domain user accounts with standardized naming conventions and attributes.
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

💡 Professional Note: In a production environment, SysAdmins rarely create users manually. This process is typically automated using PowerShell scripts or Identity Management (IdM) systems to sync users from an HR database via a CSV file.

## 3. Security & Distribution Group Management
Objective: Create logical containers to manage permissions and communication across the domain.

- Navigate to the Security Groups OU:
  - In ADUC, go to your LAB_Assets > Security_Groups container. (Centralizing groups here makes auditing much easier).
- Initialize a New Group:
  - Right-click inside the OU and select New > Group.  
- Standardized Group Naming:
  - Use a prefix to identify the group’s purpose (e.g., SG for Security Group or DL for Distribution List). Example Name: SG-Finance-Read-Only or SG-IT-Admins. 
- Configure Group Scope and Type:
  - Group Scope: Select Global. (This is the standard for most departmental groups within a single domain).
  - Group Type: Select Security. (Note: Security groups are used for permissions (folders, printers), while Distribution groups are strictly for email lists.)
- Assign Membership:
  - Right-click your new group, go to Properties > Members, and add the user accounts you created in the previous exercise.  

## 4. Adding Users to Groups
Objective: Manage group memberships
  - Open the properties of a user account
  - Go to the Member Of tab
  - Click Add and select the group (e.g., #HR_Department)
  - Verify the membership

## 5. Configure Network settings for the Server
  - Change the domain controller’s IP address to static IP
  - Change DNS Servers to loopback and google DNS






























