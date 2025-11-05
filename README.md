# **Active Directory & DNS Server Deployment on Windows Server 2022**

## **Project Overview**
This project demonstrates how to install, configure, and validate **Active Directory Domain Services (AD DS)** and **DNS Server** on a Windows Server 2022 virtual machine.  
The lab simulates a small enterprise domain environment, providing foundational experience in directory services, DNS name resolution, and domain management.

The process includes:
- Installing Active Directory Domain Services and DNS Server roles  
- Promoting the server to a domain controller  
- Creating a new forest and domain  
- Configuring static IP addressing  
- Verifying DNS resolution  
- Managing users and organizational units through Active Directory Users and Computers (ADUC)

---

## **Objective**
To deploy a functional **Active Directory domain controller** with integrated **DNS** on Windows Server 2022 and validate the environment through connectivity and name resolution testing.

---

## **Step 1: Open Server Manager**
After logging into Windows Server 2022, open **Server Manager**.  
From the **Dashboard**, select **“Add roles and features.”**

<img width="1599" height="810" alt="image" src="https://github.com/user-attachments/assets/58be3916-6925-43e3-8db5-a51c6a3d9419" />

This launches the **Add Roles and Features Wizard**, which is used to add or configure core Windows Server components.

---

## **Step 2: Select Installation Type**
Choose **“Role-based or feature-based installation.”**  
This option allows you to install AD DS and DNS roles directly onto this server.

<img width="784" height="558" alt="image" src="https://github.com/user-attachments/assets/b741298a-9bd1-41da-9c47-166435fc4b95" />

---

## **Step 3: Select the Destination Server**
Select **“This server from the server pool”** to apply changes locally.  
Your server should appear with its IP address and Windows version listed.

<img width="784" height="558" alt="image" src="https://github.com/user-attachments/assets/6c15d4e1-4c87-406c-8ee8-2bead6a04dc5" />

---

## **Step 4: Install AD DS and DNS Roles**
In the **Server Roles** list, check the boxes for:
- **Active Directory Domain Services**  
- **DNS Server**

You may see a validation message warning about the absence of a static IP address.  
AD DS and DNS require a fixed IP to prevent future connectivity issues.  
Continue to the next step to fix this.

<img width="782" height="558" alt="image" src="https://github.com/user-attachments/assets/32a392a4-937e-4594-84bb-ca6ee76583ae" />

---

## **Step 5: Configure a Static IP Address**
Before continuing, open **Network Connections** → Right-click your Ethernet adapter → **Properties** → **Internet Protocol Version 4 (TCP/IPv4)** → **Properties**.  
Select **“Use the following IP address”** and configure it manually:
- **IP address:** 10.0.2.15  
- **Subnet mask:** 255.255.255.0  
- **Default gateway:** 10.0.2.15  
- **Preferred DNS server:** 10.0.2.15  
- **Alternate DNS server:** 8.8.8.8  

This ensures the server maintains a consistent network identity and can reliably act as its own DNS authority.

<img width="1599" height="496" alt="image" src="https://github.com/user-attachments/assets/b45296f8-1a08-4a96-b50f-1f3a43698ca4" />

<img width="1599" height="824" alt="image" src="https://github.com/user-attachments/assets/d81ea652-7d49-4e40-8868-29943c719416" />

---

## **Step 6: Confirm Installation Selections**
On the **Confirmation** page, review your selected roles.  
Ensure both AD DS and DNS Server are listed, then click **Install**.  
Optionally, select **“Restart the destination server automatically if required.”**

<img width="1599" height="824" alt="image" src="https://github.com/user-attachments/assets/5a798baa-94b7-4c79-89e1-e63db29b3f87" />

After installation, you’ll receive a post-deployment notification prompting to “Promote this server to a domain controller.”

---

## **Step 7: Promote the Server to a Domain Controller**
Select **“Promote this server to a domain controller.”**  
In the **Deployment Configuration** window, choose **“Add a new forest”** and specify the root domain name:

```
rapidascent.local
```

<img width="783" height="557" alt="image" src="https://github.com/user-attachments/assets/c18ed309-bb9c-4003-9696-743f271f54d7" />

This creates a new Active Directory forest — a hierarchical structure that contains the domain and all its objects.

---

## **Step 8: Configure Domain Controller Options**
Keep default settings:
- **Forest functional level:** Windows Server 2016  
- **Domain functional level:** Windows Server 2016  
- **DNS Server** and **Global Catalog (GC)** checked  

Enter a **Directory Services Restore Mode (DSRM)** password.  
This is used for system recovery and should be securely stored.

<img width="1599" height="757" alt="image" src="https://github.com/user-attachments/assets/47de928a-e858-4cff-88b6-361337d0a30b" />

---

## **Step 9: Review Prerequisites**
Windows will perform a system validation before proceeding.  
Warnings about legacy cryptography or DNS delegation can be ignored in standalone lab setups.  
Once validation passes, click **Install** to promote the server.  
The system will automatically reboot when complete.

<img width="758" height="558" alt="image" src="https://github.com/user-attachments/assets/7d334b33-387f-4cc4-a86e-7396957de437" />

---

## **Step 10: Verify AD DS and DNS Installation**
After reboot, return to **Server Manager → Manage → Add Roles and Features → Installed Roles**.  
You should now see:
- **Active Directory Domain Services**
- **DNS Server**

<img width="1336" height="308" alt="image" src="https://github.com/user-attachments/assets/e4562e99-9140-48c3-b755-18260ceb2bee" />

Open **Active Directory Users and Computers (ADUC)** and **DNS Manager** to confirm both services are active.  
ADUC should show your domain (`rapidascent.local`) and DNS Manager should display matching forward lookup zones.

<img width="964" height="525" alt="image" src="https://github.com/user-attachments/assets/bd64d134-b991-497a-a498-6100762fd2fe" />

---

## **Step 11: Test Name Resolution**
Open PowerShell and test name resolution:
```
nslookup rapidascent.local
ping rapidascent.local
```

A successful lookup and reply confirm DNS and AD integration are functioning properly.

<img width="500" height="511" alt="image" src="https://github.com/user-attachments/assets/73617cd7-fb84-4b59-b86e-6fae211f8a10" />

---

## **Step 12: Create an Organizational Unit (OU) and User**
In **Active Directory Users and Computers**:
1. Right-click your domain → **New → Organizational Unit** → Name it `IT_Department`.  
2. Inside the new OU, right-click → **New → User**.  
   - First Name: John  
   - Last Name: Doe  
   - User logon name: jdoe  
3. Assign a password and uncheck **“User must change password at next logon.”**

This demonstrates Active Directory object creation and management within an organizational structure.

<img width="570" height="528" alt="image" src="https://github.com/user-attachments/assets/aadd6db3-7601-4a55-9cda-5d3f50b6db6b" />

<img width="570" height="528" alt="image" src="https://github.com/user-attachments/assets/f4e52eb6-68d8-44ae-a27b-7e8cac60eb0f" />

<img width="571" height="529" alt="image" src="https://github.com/user-attachments/assets/2e1f49cf-f964-4ae5-8c3b-3d9619afa12a" />


---

## **Step 13: Review System Health**
Return to **Server Manager → Local Server → Events** and check for critical or warning messages.  
Kernel power messages are normal from reboots during role installation.  
If no persistent DNS or AD errors appear, the deployment is stable.

<img width="965" height="571" alt="image" src="https://github.com/user-attachments/assets/b53e8ff3-9713-4ace-9cce-f037f5640583" />

---

## **Summary**
This lab successfully implemented a **domain controller** and **DNS infrastructure** using Windows Server 2022.  
Through this setup, the server now manages authentication, domain membership, and name resolution within the `rapidascent.local` forest.  

The configuration provides a foundation for adding domain-joined clients, managing user access, and applying Group Policy.  
By verifying DNS resolution, ADUC structure, and event logs, this lab confirms a complete and operational Active Directory environment suitable for future network simulations or SOC-related exercises.

---
