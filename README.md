### Basic Active Directory, Networking, and File Sharing Project

<div style="background: #f6f8fa; padding: 20px; border-radius: 6px; border: 1px solid #e1e4e8; max-width: 800px;">
  <img width="757" 
       alt="Active Directory Setup Steps" 
       src="https://github.com/user-attachments/assets/473de028-a937-4015-89aa-b294ea1b368e" />
</div>

## Hardware Requirements
- **VMware Workstation Pro:** 1.2 GB Disk Space and 64-bit OS
- **Windows Server 2025:** 4 GB Memory, 60 GB Disk Space
- **Windows 11 Pro:** 4 GB Memory, 64 GB Disk Space (Minimum)

## Lab Setup
1. ** Set Up a Virtual Lab Environment:**
   - Use virtualization software like **VMware Workstation Pro**, **Hyper-V**, or **VirtualBox**.
   - Install **Windows Server 2022 (or 2025)** and configure it as a **Domain Controller**.
   - Install a couple of Windows client machines (**Windows 11 Pro**) and join them to the domain.

## Activity: Infrastructure & OU Architecture

1. **Rename the server hostname**
   - Assign a unique, descriptive name (e.g., `SVR-DC-01`) and reboot to apply.

2. **Install Active Directory Tools**
   - Use Server Manager to add the **AD DS** role and promote the server to a **Domain Controller**.

3. **Creating Organizational Units (OUs)**
   - **Objective:** Organize the domain environment using a structured design that separates users, groups, and devices.

   - **Launch Management Tools:**
     - Open **Active Directory Users and Computers (ADUC)** from the Tools menu.

   - **Create the Root Container:**
     - Right-click your domain (e.g., `lab.local`) > **New > Organizational Unit**.
     - Name it **`LAB_Assets`**.

   - **Establish Object-Based Tiers:**
     - Right-click `LAB_Assets` and create three nested OUs:
       - **`Accounts`** (For all users)
       - **`Endpoints`** (For all hardware)
       - **`Security_Groups`** (For permissions)

   - **Refine the Endpoint Hierarchy:**
     - Inside **`Endpoints`**, create two nested OUs:
       - **`Workstations`** (Target for Windows 11 GPOs)
       - **`Servers`** (Target for server-specific GPOs)

   - **Departmental User Organization:**
     - Inside **`Accounts`**, create nested OUs for:
       - **`IT_Admins`**
       - **`Finance`**
       - **`Human_Resources`**





<details>
<summary><h2>2. Creating User Accounts - Identity & Access Management</h2></summary>

**Objective:** Provision domain user accounts with standardized naming conventions and attributes.

- **Locate the Target Container:**
  - Select your **`LAB_Assets > Accounts`** OU.
  - Select the specific department (e.g., **`IT_Admins`** or **`Finance`**).

- **Provision a New User:**
  - Right-click the departmental OU and select **New > User**.

- **Define Identity Attributes:**
  - Follow a standard naming convention for the User Logon Name (e.g., First Initial + Last Name).
  - **Example:**
    - First Name: Marcus
    - Last Name: Wright
    - User Logon Name: `mwright`

- **Security & Password Configuration:**
  - Assign a temporary password that meets the domain's complexity requirements.
  - **Settings:** Select **"User must change password at next logon"** (best practice) or **"Password never expires"** (common for lab service accounts).

- **Assign Professional Metadata:**
  - Once the user is created, right-click the account and select **Properties**.
  - In the **Description** field, add their specific role (e.g., Senior Systems Engineer).
  - Under the **Organization** tab, fill in the **Job Title** and **Department**.

> 💡 **Professional Note:** In a production environment, SysAdmins rarely create users manually. This process is typically automated using PowerShell scripts or Identity Management (IdM) systems to sync users from an HR database via a CSV file.

</details>






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
Objective: Delegate permissions by nesting user accounts into functional security groups.
  - Access User Identity Properties:
    - In ADUC, locate the user account you want to manage (e.g., in the Accounts > Finance OU).
    - Right-click the user and select Properties.
  - Assign Group Affiliation:
    - Navigate to the Member Of tab.
    - Click Add and type the name of the security group you created earlier (e.g., SG-Finance-Read-Only).
    - Click Check Names to validate the object, then click OK.
  - Verify the membership (Security Best Practice: Always assign permissions to Groups, not individual Users.)

## 5. Configure Network settings for the Server
Objective: Assign a persistent identity to the Domain Controller to ensure reliable connectivity for all domain clients.

- Static IP Assignment:
  - Open Network Connections and modify the IPv4 properties of your primary Ethernet adapter.
  - Switch from "Obtain an IP address automatically" to "Use the following IP address."
  - Example IP: 172.16.0.10 (or any address within your lab's subnet).
 
- DNS Server Hierarchy:
  - Preferred DNS: Set this to 127.0.0.1 (the Loopback address).
    - Reason: This tells the server to look at itself first to resolve Active Directory queries.
  -  Alternate DNS: Set this to 8.8.8.8 (Google Public DNS) or 1.1.1.1 (Cloudflare).
    -  Reason: This allows the server to reach the internet for updates if it can't resolve a query locally.
  - Verification:
    -   Open Command Prompt and run ipconfig /all to confirm the settings are applied and that the "DHCP Enabled" flag is now set to No.
   































