# Active Directory Group Documentation

### Group Types
In Active Directory, there are two primary types of groups:

* **Security Groups:** These have a Security Identifier (SID). They are used to assign permissions to shared resources (folders, printers) and can also be used as email distribution lists.
* **Distribution Groups:** These do not have a SID. They are used exclusively for email distribution lists and cannot be used to assign permissions to any resources.

---

### The AGDLP Methodology
To manage your lab like a pro, follow this sequence. This ensures that if a user changes departments, you only change their group membership, not the permissions on every single folder.

* **A (Account):** Create the User account (e.g., jsmith).
* **G (Global):** Add the user to a Global group based on their role (e.g., SG-Finance).
* **DL (Domain Local):** Create a Domain Local group for a specific resource (e.g., DL-Finance-Folder-Read). Add the SG-Finance group into this group.
* **P (Permissions):** Assign the DL-Finance-Folder-Read group "Read" permissions on the actual folder.

---

### Group Reference Table

| Group Name | Group Type | Group Scope | Explanation / Use Case |
| :--- | :--- | :--- | :--- |
| **SG-Finance** | Security | Global | Departmental: For members of the Finance team. |
| **SG-IT-Admins** | Security | Global | Departmental: For IT staff with administrative needs. |
| **SG-Human-Resources** | Security | Global | Departmental: For HR staff to access employee records. |
| **SG-Marketing** | Security | Global | Departmental: For access to design and social media assets. |
| **SG-Sales** | Security | Global | Departmental: For access to CRM data and lead lists. |
| **ADM-Domain-Admins** | Security | Global | Administrative: For high-level server or domain admin rights. |
| **DL-File-Data-Modify** | Security | Domain Local | Resource: Grants "Modify" access to the root data share. |
| **DL-Finance-Read** | Security | Domain Local | Resource: Specifically for read-only access to the Finance folder. |
| **DL-Printer-Legal** | Security | Domain Local | Resource: To restrict who can use the Legal department printer. |
| **SG-VPN-Users** | Security | Global | Policy: Controls who is allowed to connect via VPN. |
| **SG-Service-Accounts** | Security | Global | Management: A container for non-human service accounts. |