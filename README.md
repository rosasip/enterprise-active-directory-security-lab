# For Absolute Beginners
> From zero to connecting to remote servers - a step-by-step journey


## 📋 Prerequisites: What You Need
Before we start, let's understand what we're working with to ensure a smooth setup:

 **💻 Two Computers** (or a computer and a virtual machine)
   - **Client:** Your main computer (where you'll type commands)
   - **Server:** The computer you want to connect to (could be a VM on your same machine)

**🌐 Network Connectivity**
   The two computers need to be able to reach each other (on the same network, or with a proper internet setup).

**🔑 Administrator Access**
   To install software or change system settings, you'll need admin/sudo privileges.

---





<details>
<summary><h3>1. What is SSH?</h2></summary>
<br>

> SSH (Secure Shell) is a cryptographic network protocol that enables secure communication between two systems. Think of it as a **secure pipe** that allows you to send data or commands over an unsecured network (like the internet).

### Why do we need it?
---
> **Remote Management:**
> Log into a server's terminal from your own laptop as if you were sitting right in front of it.
>
> **Git operations:**
> Securely push code to GitHub, GitLab, or other repositories.
>
> **File Transfers:**
> Move files safely using SFTP or SCP.
>
> **Tunneling:**
> Create secure bridges for web traffic (more advanced).

### How it Works?
---
1. **Your Computer** = The Client (you initiate the connection)
2. **The Remote Server** = The Host (the computer you're connecting to)
3. **The Connection** = Encrypted tunnel that keeps everything private
</details>

<details>
<summary><h3>2. What is a Shell?</h3></summary>

>To use SSH effectively, you need to understand what a **shell** is. A shell is simply a program that takes commands from your keyboard and gives them to the operating system to perform.

### Two Types of Shells

#### 1. ``Command Line Shell (CLI)``: A text-based interface where you type commands. You're using this right now!

Common types:
- **Bash (Bourne Again Shell):** Most common on Linux and older macOS
- **Zsh (Z Shell):** Current default on macOS; highly customizable
- **PowerShell:** The modern standard for Windows administration
- **Sh (Bourne Shell):** The "grandfather" of shells; simple and present on almost all Unix systems

#### 2. ``Graphical Shell (GUI)``: A visual interface with windows, icons, and menus:
- **Windows Explorer:** The shell you use to browse folders in Windows
- **GNOME / KDE:** Popular graphical shells for Linux desktops
- **macOS Finder:** The graphical shell on Mac

> ⚠️ **Important:** When you use SSH, you're getting access to the remote computer's **command line shell**, not its graphical interface.
</details>



<details>
<summary><h3>3: Check if SSH is Installed</h3></summary>

> Before we install anything, let's see if SSH is already on your system.

#### For Windows (PowerShell or Command Prompt):
`` where ssh``
#### For macOS/Linux (Terminal): 
```which ssh```
> When you run where ssh or which ssh, you're looking for the SSH Client - the program that lets you initiate connections. This is the first piece we need.


> If SSH is installed: It returns a path ending in .exe, if NOT installed: It returns nothing or an error message.
</details>


<details>
<summary><h3>4. Install OpenSSH Server (Windows)</h3></summary>

> For SSH to work, you need two components:

- **SSH Client:** Installed on your computer (lets you connect out to others)
- **SSH Server:** Installed on the computer you want to connect into (lets others connect to you)

### First, check what's already installed

Run this command in PowerShell:

```Get-WindowsCapability -Online | Where-Object Name -like 'OpenSSH*'```

What this command does:

```Get-WindowsCapability```: Asks Windows what optional features are available

```-Online```: Checks your current Windows installation

```Where-Object Name -like 'OpenSSH*'```: Filters to show only OpenSSH-related items











