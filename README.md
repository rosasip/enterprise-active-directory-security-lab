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
   - Install a couple of Windows client machines (**Windows 11 Pro**) to join them to the domain.

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
<summary><h2>2. Creating User Accounts</h2></summary>

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

<details> 
 <summary><h2> 3. Creating Groups </h2></summary> 
Objective: Security & Distribution Group Management

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
</details>

<details>
<summary><h2>4. Adding Users to Groups </h2></summary>  
Objective: Delegate permissions by nesting user accounts into functional security groups.
   
  - Access User Identity Properties:
      - In ADUC, locate the user account you want to manage (e.g., in the Accounts > Finance OU).
      - Right-click the user and select Properties.
  - Assign Group Affiliation:
      - Navigate to the Member Of tab.
      - Click Add and type the name of the security group you created earlier (e.g., SG-Finance-Read-Only).
      - Click Check Names to validate the object, then click OK.
  - Verify the membership (Security Best Practice: Always assign permissions to Groups, not individual Users.)
</details>

<details> 
<summary><h2>5. Configure Network settings for the Server</h2></summary> 
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
</details>

<details>
<summary><h2>6. Join Windows Client to the Domain</h2></summary>

**Objective:** Connect the Windows 11 workstation to the Active Directory environment and verify user authentication.

1. **Configure Client Network Settings:**
   - Open **Network Connections** on the Windows 11 machine.
   - Set the **Preferred DNS Server** to the IP address of your Domain Controller (e.g., `172.16.0.10`).
   - Set the **Alternate DNS Server** to `8.8.8.8` (Google DNS).

2. **Domain Join & OU Migration:**
   - Go to **Settings > System > About > Advanced System Settings > Computer Name**.
   - Click **Change**, select **Domain**, and enter your domain name (e.g., `lab.local`).
   - Authenticate with Domain Admin credentials and restart.
   - **Crucial Step:** On the Domain Controller, open **ADUC** and move the new computer object from the default `Computers` container into **`LAB_Assets > Endpoints > Workstations`**.

3. **Verify Domain Login:**
   - On the Windows 11 login screen, select **Other User**.
   - Log in using the credentials created earlier (e.g., `mwright`).
   - Run `whoami /fqdn` in Command Prompt to confirm the user is recognized by the domain.
</details>

<details>
<summary><h2>7. Creating and Linking Group Policy Objects (GPOs)</h2></summary>

**Objective:** Implement centralized management by creating and enforcing policies across the domain.

1. **Launch Group Policy Management:**
   - Open the **Group Policy Management Console (GPMC)** from the Server Manager Tools menu.

2. **Create a New GPO:**
   - Navigate to **`Forest > Domains > [Your Domain]`**.
   - Right-click **Group Policy Objects** and select **New**.
   - Name the GPO based on its function (e.g., `GPO_Disable_USB` or `GPO_Wallpaper_Standard`).

3. **Configure Policy Settings:**
   - Right-click your new GPO and select **Edit**.
   - **Example - Restrict Control Panel:** - *User Configuration > Policies > Administrative Templates > Control Panel > Prohibit access to Control Panel.*
   - **Example - Wallpaper:** - *User Configuration > Policies > Administrative Templates > Desktop > Desktop > Desktop Wallpaper.*

4. **Link the GPO to an OU:**
   - Right-click the specific OU where you want the policy to apply (e.g., `LAB_Assets > Accounts > Finance`).
   - Select **Link an Existing GPO** and choose your policy.
  
<!-- The type of each folder is different. The main difference between a container and a Organisational Unit is that in the container type Group Policies cannot be applied, in the Organisational Unit type of folder you can apply group policies.-->

5. **Enforce the Policy:**
   - On the Windows 11 client, open Command Prompt and run `gpupdate /force` to apply the changes immediately without rebooting.
</details>

<details>
<summary><h2>8. Configuring File Sharing and NTFS Permissions</h2></summary>

**Objective:** Set up a secure, department-specific network share using the Principle of Least Privilege.

1. **Initialize the Shared Directory:**
   - On the Server's `C:` drive, create a folder named **`Company_Data`**.
   - Inside that folder, create a sub-folder named **`Finance_Private`**.

2. **Configure Advanced Sharing (Share Permissions):**
   - Right-click `Company_Data` > **Properties > Sharing > Advanced Sharing**.
   - Check **"Share this folder"** and name it `Data$`. (The `$` makes it a "hidden" share).
   - Click **Permissions**: Set **`Everyone`** to **`Read/Write`**. 
   - *Note: We manage the actual restrictions in the next step via NTFS.*

3. **Configure NTFS Permissions (Security Tab):**
   - Go to the **Security** tab > **Advanced**.
   - **Disable Inheritance** and convert existing permissions to explicit permissions.
   - Remove the `Users` group.
   - Add your security group (e.g., **`SG-Finance-Read-Only`**) and assign **`Read & Execute`** permissions.

4. **Verify Access from Client:**
   - On the Windows 11 machine, press `Win + R` and type `\\<Server-IP>\Data$`.
   - Confirm that users in the Finance group can see the files, while users from other departments (like HR) are denied access.
</details>

<details>
<summary><h2>9. Map Network Drives via Group Policy</h2></summary>

**Objective:** Automatically mount network shares as drive letters for users when they log in.

1. **Initialize the Drive Mapping GPO:**
   - In the **GPMC**, create a new GPO named **`GPO_Map_Finance_Drive`**.

2. **Configure Drive Mapping Preferences:**
   - Right-click the GPO and select **Edit**.
   - Navigate to: **User Configuration > Preferences > Windows Settings > Drive Maps**.
   - Right-click > **New > Mapped Drive**.
   - **Action:** Select `Update`.
   - **Location:** Enter the path to your share (e.g., `\\SVR-DC-01\Data$`).
   - **Label as:** Enter a friendly name (e.g., `Finance Department`).
   - **Drive Letter:** Assign a specific letter (e.g., `F:`).

3. **Targeting & Linking:**
   - **Link** the GPO to the **`LAB_Assets > Accounts > Finance`** OU.
   - *Note: By linking here, only the Finance users will see this drive, keeping the workspace clean for other departments.*

4. **Verify on Client:**
   - On the Windows 11 machine, log in as a Finance user.
   - Open **File Explorer > This PC**.
   - Confirm the `F:` drive appears automatically. 
   - If it doesn't appear, run `gpupdate /force` and log out/in.
</details>

<details>
<summary><h2>10. Creating & Configuring Service Accounts</h2></summary>

**Objective:** Provision a dedicated account for automated system processes or kiosk environments.

1. **Locate the Service Account Container:**
   - In **ADUC**, navigate to your **`LAB_Assets > Accounts`** OU.
   - (Optional) Create a specific sub-OU named **`Service_Accounts`** to keep these separate from human users.

2. **Provision the Account:**
   - Right-click the OU > **New > User**.
   - **Name:** Use a prefix to identify it as a service (e.g., `SVC_AutoLogin`).
   - **Description:** Clearly state the purpose (e.g., `Account for Warehouse Kiosk Autologon`).

3. **Security Hardening:**
   - Assign a high-entropy (strong) password.
   - **Settings:** Check **"User cannot change password"** and **"Password never expires"**.
   - *Note: In a production environment, you would eventually move to Managed Service Accounts (gMSAs) for even better security.*

4. **Assign Permissions:**
   - Add the account to any specific **Security Groups** required for the task it performs. 
   - Ensure it has the "Log on as a service" or "Log on locally" right if required by the application.

5. **Configure Client Autologon (Sysinternals):**
   - On the target Windows 11 client, download the **Microsoft Sysinternals Autologon** tool.
   - Run `Autologon.exe`, enter the `SVC_AutoLogin` credentials, and the domain name.
   - This encrypts the credentials in the registry, allowing the machine to bypass the login screen and go straight to the desktop after a reboot—ideal for dashboard monitors or kiosks.
</details>


<details>
<summary><h2>11. Configuring Account Lockout Policy</h2></summary>

**Objective:** Mitigate brute-force and dictionary attacks by automatically disabling accounts after multiple failed login attempts.

1. **Initialize the Security GPO:**
   - In the **GPMC**, create a new GPO named **`GPO_Account_Security_Policy`**.

2. **Configure Lockout Thresholds:**
   - Navigate to: **Computer Configuration > Policies > Windows Settings > Security Settings > Account Policies > Account Lockout Policy**.
   - **Account lockout threshold:** Set to `5` invalid attempts. (This is a balance between security and user convenience).
   - **Account lockout duration:** Set to `30 minutes`.
   - **Reset account lockout counter after:** Set to `30 minutes`.

3. **Linking & Inheritance:**
   - **Important:** Account policies like these are typically linked at the **Domain Level** (the very top folder) because they are handled by the Domain Controller's security database. 

4. **Testing the Policy:**
   - On the Windows 11 client, attempt to log in with a valid username but a **wrong password** 5 times.
   - Verify that the 6th attempt results in an "Account Locked" message.
   - On the Server, open **ADUC**, find the user, and observe the "Unlock Account" checkbox in their properties.
</details>

<details>
<summary><h2>Testing & Verification</h2></summary>

**Objective:** Confirm that all configurations—Identity, Networking, and Policy—are functioning as intended.

1. **Verify User Authentication:**
   - Log on to a Windows 11 client machine using a newly created domain account (e.g., `mwright`).
   - **Success Criteria:** The user profile is created successfully, and the desktop loads without "Temporary Profile" errors.

2. **Audit Group Policy & Membership:**
   - Open **Command Prompt** (as the user) and run: `gpresult /r`
   - **Success Criteria:** - Under "Applied Group Policy Objects," you should see your custom GPOs (e.g., `GPO_Account_Security_Policy`).
     - Under "The user is a part of the following security groups," you should see your `SG-` groups.

3. **Validate Network Resource Access:**
   - Open **File Explorer > This PC**.
   - **Success Criteria:** The mapped network drive (e.g., `F:`) appears with the correct label. Double-click it to ensure you can create/read files based on your NTFS permissions.

4. **Verify Account Lockout Logic:**
   - Intentional Failure: Attempt to log in with a wrong password 5 times.
   - **Success Criteria:** The 6th attempt should trigger a message stating the account is locked. Confirm you can unlock the user from the Domain Controller.
</details>



<!-- 
"I love my Dropdown" or/and "Accordion" effect. :)
<details>
<summary><h2> </h2></summary> 
</details>
-->























