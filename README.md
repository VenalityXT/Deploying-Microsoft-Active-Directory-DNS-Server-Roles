# **Active Directory & DNS Server Deployment**

[![OS](https://img.shields.io/badge/OS-Windows%20Server%202022-blue?logo=windows)](https://learn.microsoft.com/en-us/windows-server/)
[![Service](https://img.shields.io/badge/Service-Active%20Directory-blue)](https://learn.microsoft.com/en-us/windows-server/identity/ad-ds/get-started/virtual-dc/active-directory-domain-services-overview)
[![Networking](https://img.shields.io/badge/Service-DNS-green)](https://learn.microsoft.com/en-us/windows-server/networking/dns/dns-top)
[![Role Installation](https://img.shields.io/badge/Focus-Server%20Role%20Installation-orange)](https://learn.microsoft.com/en-us/windows-server/administration/server-manager/install-or-uninstall-roles-role-services-or-features)
[![Domain Controller](https://img.shields.io/badge/Concept-Domain%20Controller-lightgrey)](https://learn.microsoft.com/en-us/windows-server/identity/ad-ds/plan/understanding-active-directory-domain-services)
[![System Administration](https://img.shields.io/badge/Skill-System%20Administration-red)](https://en.wikipedia.org/wiki/System_administrator)

---

## **Project Overview**

This project demonstrates how to **deploy, configure, and validate** an **Active Directory Domain Services (AD DS)** and **DNS Server** environment on a **Windows Server 2022** VM using VirtualBox.  
The lab simulates an enterprise domain controller setup from start to finish; Installing necessary roles, configuring static IP addressing, promoting the server to a domain controller, and validating functionality through DNS resolution and directory management.

The environment serves as a foundational exercise for **systems administrators and cybersecurity analysts** learning how identity and name resolution infrastructure functions within enterprise networks.  
By completing this configuration, users gain practical experience in **domain creation, DNS zone management, and Active Directory structure organization**.

---

## **Objectives**

1. Configure a **static IP address** to ensure reliable network and DNS operations.  
2. Install and verify the **Active Directory Domain Services (AD DS)** and **DNS Server** roles.  
3. **Promote the server** to a domain controller and establish a new forest (`rapidascent.local`).  
4. Validate proper **DNS name resolution** and **network connectivity** post-installation.  
5. Create an **Organizational Unit (OU)** and a sample user to demonstrate directory structure management.  
6. Review system **event logs and service health** to confirm successful configuration.

---

### Step 1: Access Server Manager

The **Server Manager** console is the primary interface for managing server roles and features on **Windows Server**. It provides the tools necessary to configure services like **Active Directory Domain Services (AD DS)** and **DNS**, which are required for setting up a domain environment.

Start by opening **Server Manager** from the **Start Menu** or taskbar. From the **Dashboard**, click **Add roles and features** to begin the installation process.

- Open **Server Manager** from the Start Menu or taskbar.  
- From the **Dashboard**, select **Add roles and features** to start the installation.

<img width="1599" height="810" alt="image" src="https://github.com/user-attachments/assets/58be3916-6925-43e3-8db5-a51c6a3d9419" />

> The **Add Roles and Features Wizard** will guide you through the process of adding server components, with a focus on enabling **AD DS** for domain management and **DNS** for name resolution.

---

### Step 2: Select Installation Type

When prompted, select **"Role-based or feature-based installation"**. This option allows us to install the **Active Directory Domain Services (AD DS)** and **DNS** roles, which are essential for setting up the domain environment.

Here’s a quick comparison of the two options available:

| Option                                     | Description | Why We're Choosing It |
|--------------------------------------------|-------------|-----------------------|
| **Role-based or feature-based installation** | Install and configure core roles like **AD DS** and **DNS**. | This is the right choice for building a domain controller and setting up DNS for name resolution. |
| **Remote Desktop Services installation**    | Used for configuring Remote Desktop environments for users. | We don't need this for a domain setup, but it's useful for deploying virtual desktop infrastructures or allowing remote desktop access to a server. |

<img width="784" height="558" alt="image" src="https://github.com/user-attachments/assets/b741298a-9bd1-41da-9c47-166435fc4b95" />

> The **Role-based installation** is the foundation for adding domain-related roles. Think of it like choosing to build the flooring of a house before adding the furniture!

---

### Step 3: Select the Destination Server

In this step, select **"This server from the server pool"** to apply the changes locally to the current server.  
Your server should appear in the list with its IP address and Windows version shown, confirming that it's the correct server for the installation.

<img width="784" height="558" alt="image" src="https://github.com/user-attachments/assets/6c15d4e1-4c87-406c-8ee8-2bead6a04dc5" />

> By selecting **"This server from the server pool"**, you're ensuring that the roles will be installed on the local server, rather than another machine in the network.

---

### Step 4: Install AD DS and DNS Roles

In the **Server Roles** list, check the boxes for:
- **Active Directory Domain Services (AD DS)**  
- **DNS Server**

These roles are essential for setting up your domain controller and ensuring internal name resolution for your network.

You may see a validation message about the absence of a static IP address.  
Since **AD DS** and **DNS** require a fixed IP to function properly, you'll need to resolve this before continuing.

<img width="782" height="558" alt="image" src="https://github.com/user-attachments/assets/32a392a4-937e-4594-84bb-ca6ee76583ae" />

> **Note:** If you see the static IP warning, don't worry! This is a reminder that a domain controller needs a consistent network identity. We'll address this in **Step 5** by configuring a static IP for the server.

---

### Step 5: Configure a Static IP Address

Before continuing with the role installation, it’s essential to configure a **static IP address** for the server. A **static IP** ensures that the server maintains a consistent network identity, which is necessary for domain services like **Active Directory** and **DNS** to function correctly.

By default, the server may obtain its IP address dynamically through **DHCP** (Dynamic Host Configuration Protocol), but this can lead to potential network issues or service interruptions, especially when the server’s IP address changes. A **static IP** eliminates this problem and guarantees the server’s address stays the same.

To configure the static IP address:
1. Open **Network Connections** and right-click your **Ethernet adapter**.
2. Select **Properties**, then click on **Internet Protocol Version 4 (TCP/IPv4)** and choose **Properties** again.
3. Select **"Use the following IP address"** and enter the following settings:

   - **IP address:** 10.0.2.15  
   - **Subnet mask:** 255.255.255.0  
   - **Default gateway:** 10.0.2.15  
   - **Preferred DNS server:** 10.0.2.15  
   - **Alternate DNS server:** 8.8.8.8  

This ensures that the server's network identity remains fixed, which is necessary for DNS resolution and domain controller services to function reliably.

<img width="1599" height="496" alt="image" src="https://github.com/user-attachments/assets/b45296f8-1a08-4a96-b50f-1f3a43698ca4" />

> After selecting **TCP/IPv4 Properties**, configure the server to **use a static IP** instead of obtaining one dynamically from **DHCP**. This ensures that the server’s IP address remains the same, preventing issues that can arise from a changing address.

<img width="1599" height="824" alt="image" src="https://github.com/user-attachments/assets/d81ea652-7d49-4e40-8868-29943c719416" />

Configuring a **static IP address** is critical for domain controllers because it guarantees that the server’s IP will not change. This consistency is necessary for **DNS** to reliably resolve names and for **AD DS** to

---

### Step 6: Confirm Installation Selections

On the **Confirmation** page, you will review the roles that you’ve selected for installation. Double-check that both **Active Directory Domain Services (AD DS)** and **DNS Server** are listed under the selected roles.

Once confirmed, click **Install** to proceed with the installation.  
You also have the option to select **“Restart the destination server automatically if required,”** which will ensure the server restarts automatically after the installation, minimizing downtime.

<img width="783" height="557" alt="image" src="https://github.com/user-attachments/assets/aeae904c-03ef-4d24-9f42-f80d072b5ae1" />

> It’s always a good idea to enable the automatic restart option. This ensures that the installation completes smoothly without requiring manual intervention after the server finishes setting up the roles.

After the installation completes, a post-deployment notification will prompt you to **“Promote this server to a domain controller”**. This step is essential to finalize the setup and make the server fully functional as a domain controller.

---

### Step 7: Promote the Server to a Domain Controller

Once the installation completes, you will see a **post-deployment notification** prompting you to **“Promote this server to a domain controller.”** Click on this link to begin the configuration.

<img width="1599" height="757" alt="image" src="https://github.com/user-attachments/assets/88cdcb98-e5cd-4c45-8582-7e3533dbc93c" />

In the **Deployment Configuration** window, choose **“Add a new forest”** since we are setting up a new domain environment.

After that, we'll specify the **root domain name** for the forest. In this case, we will use:

```
rapidascent.local
```

<img width="758" height="558" alt="image" src="https://github.com/user-attachments/assets/4a871896-aabd-4581-9c93-3b1a21a8fec2" />

> The root domain name forms the foundation of the forest. It is the highest-level domain in your Active Directory structure and will be used to identify the domain in your network.

Click **Next** and proceed with the wizard’s steps to complete the configuration.

By promoting the server, you are creating a **new Active Directory forest**. A forest is a hierarchical structure that houses the domain and all its objects, such as users, computers, and group policies.

---

### Step 8: Configure Domain Controller Options

In the **Domain Controller Options** window, keep the default settings for optimal compatibility:

- **Forest functional level**: Windows Server 2016
- **Domain functional level**: Windows Server 2016
- **DNS Server** and **Global Catalog (GC)** should both be checked

These options ensure that the domain controller will support key Active Directory services, such as DNS for domain name resolution and Global Catalog for faster search capabilities across domains.

Next, enter a **Directory Services Restore Mode (DSRM)** password.  
This password is critical for system recovery, as it grants you access to the server when booting into Directory Services Restore Mode. Be sure to store this password securely, as it will be needed for disaster recovery scenarios.

<img width="759" height="557" alt="image" src="https://github.com/user-attachments/assets/d92a1e4d-f555-4ba4-a380-0a07a89b7f06" />

The **Forest functional level** and **Domain functional level** determine the features available in your Active Directory environment. These default settings ensure compatibility with older versions of Windows Server, which is why we use

---

### Step 9: Review Prerequisites

Windows will perform a system validation to ensure everything is in place for AD DS and DNS installation.  
While warnings about legacy cryptography settings or DNS delegation may appear, these can typically be ignored in standalone lab environments. Once the validation passes successfully, click **Install** to proceed with promoting the server.  

The system will automatically reboot when the installation is complete.

<img width="758" height="557" alt="image" src="https://github.com/user-attachments/assets/8243a149-0413-443e-ad2f-dc4209c0acc7" />

> **Tip**: DNS delegation issues are only relevant in a multi-server environment with an existing DNS infrastructure. In a lab setup, we can bypass this step since we're configuring everything locally.

---

### Step 10: Verify AD DS and DNS Installation

After the server reboots, open **Server Manager → Manage → Add Roles and Features → Installed Roles** to confirm the roles have been successfully installed.  
You should see:
- **Active Directory Domain Services**
- **DNS Server**

<img width="1336" height="308" alt="image" src="https://github.com/user-attachments/assets/e4562e99-9140-48c3-b755-18260ceb2bee" />

Next, open **Active Directory Users and Computers (ADUC)** and **DNS Manager** to verify both services are active.  
ADUC should display your domain (`rapidascent.local`), and DNS Manager should show the corresponding forward lookup zones.

---

### Step 11: Test Name Resolution

To confirm DNS and AD integration, open **PowerShell** and test name resolution:

```powershell
nslookup rapidascent.local
ping rapidascent.local
```
<img width="500" height="511" alt="image" src="https://github.com/user-attachments/assets/a3f58c3c-9c7c-4e7c-b75b-5bbad57ceeb8" />

A successful lookup and reply will confirm that DNS and AD are functioning correctly. If the setup is correct, the nslookup command should return the server’s IP address, and ping should successfully reach the domain.

---

### Step 12: Create an Organizational Unit (OU) and User

In **Active Directory Users and Computers (ADUC)**:
1. Right-click your domain → **New → Organizational Unit** → Name it `IT_Department`.  
2. Inside the new OU, right-click → **New → User**.  
   - First Name: John  
   - Last Name: Doe  
   - User logon name: jdoe  
3. Set a password and uncheck **“User must change password at next logon.”**

This demonstrates creating and managing Active Directory objects like users and organizational units within a domain.

<img width="570" height="528" alt="image" src="https://github.com/user-attachments/assets/aadd6db3-7601-4a55-9cda-5d3f50b6db6b" />

<img width="570" height="528" alt="image" src="https://github.com/user-attachments/assets/f4e52eb6-68d8-44ae-a27b-7e8cac60eb0f" />

<img width="571" height="529" alt="image" src="https://github.com/user-attachments/assets/2e1f49cf-f964-4ae5-8c3b-3d9619afa12a" />

---

### Step 13: Review System Health

Return to **Server Manager → Local Server → Events** and check for any critical or warning messages.  
You may notice **Kernel Power** messages, which are expected due to reboots during role installation. As long as no persistent **DNS** or **AD** errors appear, the deployment is stable.

<img width="965" height="571" alt="image" src="https://github.com/user-attachments/assets/b53e8ff3-9713-4ace-9cce-f037f5640583" />

---

## **Summary**
This lab successfully implemented a **domain controller** and **DNS infrastructure** using Windows Server 2022.  
Through this setup, the server now manages authentication, domain membership, and name resolution within the `rapidascent.local` forest.  

The configuration provides a foundation for adding domain-joined clients, managing user access, and applying Group Policy.  
By verifying DNS resolution, ADUC structure, and event logs, this lab confirms a complete and operational Active Directory environment suitable for future network simulations or SOC-related exercises.

---
