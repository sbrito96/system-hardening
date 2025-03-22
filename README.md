# Systems Hardening with Patch Manager via AWS Systems Manager

## 📌 Project Overview

In organizations with hundreds or thousands of workstations, keeping all **operating systems (OS) and applications** up to date is a challenge. **AWS Systems Manager Patch Manager** simplifies this by allowing administrators to manage patches across their infrastructure efficiently.

In this project, I used **Patch Manager** to: </br> 
✅ **Patch Linux instances** using the **default baseline**.  
✅ **Create a custom patch baseline** for Windows instances.  
✅ **Use patch groups** to patch Windows instances.  
✅ **Verify patch compliance** across all instances.  

---

## **1️⃣ Patching Linux Instances Using Default Baselines**

AWS provides **predefined patch baselines** for various OS versions. In this step, I applied the default patch baseline to Linux instances.

### **Step 1: Access AWS Systems Manager**
1. In the **AWS Management Console**, search for **Systems Manager** and open it.
2. In the left navigation pane, go to **Fleet Manager**.
![image](https://github.com/user-attachments/assets/32e2742b-590b-4888-968d-404be09ce93f)

4. View the **pre-configured EC2 instances** (3 Linux & 3 Windows). These instances have an **IAM role** allowing management via Systems Manager.
![image](https://github.com/user-attachments/assets/78db5940-efdd-4b8e-96f2-551be6028b7a)

### **Step 2: Apply Patching**
1. Select `Linux-1`, then open the **Node ID**.
![image](https://github.com/user-attachments/assets/dff0c2d8-569d-415f-aa6a-0796cbc557ea)

Here we can view details about the specific instance
![image](https://github.com/user-attachments/assets/859e3475-a491-4009-88f2-19c175b759ad)

3. Return to **Systems Manager → Patch Manager**.
![image](https://github.com/user-attachments/assets/2ec96716-b483-4f2e-99f0-a24173da6dba)

5. Click **Patch now**.
6. Configure the patch operation:
   - **Patching operation:** `Scan and install`
   - **Reboot option:** `Reboot if needed`
   - **Instances to patch:** `Patch only the target instances I specify`
  ![image](https://github.com/user-attachments/assets/ad52ca40-a474-4c0d-8864-70046df3b8a3)

   - **Target selection:** `Specify instance tags`
   - **Tag key:** `Patch Group`
   - **Tag value:** `LinuxProd`
![image](https://github.com/user-attachments/assets/1e4c2b8d-088d-4266-8e88-86c73573dcea)

7. Click **Add** → **Patch now**.
![image](https://github.com/user-attachments/assets/acf1b5a2-1cd2-4556-a904-3ddaedf83086)

9. Monitor the **AWS-PatchNowAssociation panel** to track patch progress.
![image](https://github.com/user-attachments/assets/04d2215a-5ccb-44c1-a89b-7d23dd4704f1)
![image](https://github.com/user-attachments/assets/3c2018f5-6da1-4fd8-80bf-d52a34aa9048)

---

## **2️⃣ Creating a Custom Patch Baseline for Windows Instances**

Windows instances also have default patch baselines, but for this project, I created a **custom baseline** focused on security updates.

### **Step 1: Create a Patch Baseline**
1. Go to **Systems Manager → Patch Manager**.
2. Click **Create patch baseline**.
![image](https://github.com/user-attachments/assets/a732cb46-317b-4ecf-b595-d747da7ad1be)
   
4. Configure the following:
   - **Name:** `WindowsServerSecurityUpdates`
   - **Description:** `Windows security baseline patch`
   - **Operating System:** `Windows`
   - **Default baseline:** Leave **unchecked**.
![image](https://github.com/user-attachments/assets/cb8a59a8-9bb9-49e4-b533-c1854c762359)

### **Step 2: Define Approval Rules**
1. **Add Rule 1:**
   - **Products:** `WindowsServer2019` (uncheck `All`).
   - **Severity:** `Critical`.
   - **Classification:** `SecurityUpdates`.
   - **Auto-approval delay:** `3 days`.
   - **Compliance reporting:** `Critical`.
![image](https://github.com/user-attachments/assets/96cdae16-5ec3-436a-9437-05f50b8ed734)
  
2. **Add Rule 2:**
   - **Products:** `WindowsServer2019`.
   - **Severity:** `Important`.
   - **Classification:** `SecurityUpdates`.
   - **Auto-approval delay:** `3 days`.
   - **Compliance reporting:** `High`.
![image](https://github.com/user-attachments/assets/e594d982-fba6-4452-9bc0-f459bdd6eea6)

3. Click **Create patch baseline**.
![image](https://github.com/user-attachments/assets/da75a88a-2c0c-45fc-8d35-36ac0bbf0424)

### **Step 3: Associate the Patch Baseline with a Patch Group**
1. Select `WindowsServerSecurityUpdates`.
![image](https://github.com/user-attachments/assets/c3396508-03dd-4586-b610-cc776a9600c0)

3. Click **Actions → Modify patch groups**.
![image](https://github.com/user-attachments/assets/708cea42-0511-4582-abbf-20a8739f8873)

5. Under **Patch groups**, enter `WindowsProd`.
6. Click **Add → Close**.
![image](https://github.com/user-attachments/assets/680f6a88-1d42-4b51-b4f8-507bedcb64f5)

---

## **3️⃣ Patching Windows Instances**

### **Step 1: Tag Windows Instances**
1. Go to **EC2 → Instances**.
2. Select `Windows-1`, go to **Tags**, and click **Manage tags**.
![image](https://github.com/user-attachments/assets/d41b6d38-cd3d-4549-83d6-e677c379e359)

4. Click **Add new tag**:
   - **Key:** `Patch Group`
   - **Value:** `WindowsProd`
5. Click **Save**.
![image](https://github.com/user-attachments/assets/cfbee656-61b3-4f65-92bf-9393ba1a9c19)

7. Repeat for `Windows-2` and `Windows-3`.

### **Step 2: Apply Patching**
1. Return to **Systems Manager → Patch Manager**.
2. Click **Patch now**.

![image](https://github.com/user-attachments/assets/f23d13bc-ffa3-4b08-a48c-5b2e589da5b9)

4. Configure:
   - **Patching operation:** `Scan and install`
   - **Reboot option:** `Reboot if needed`
   - **Instances to patch:** `Patch only the target instances I specify`
![image](https://github.com/user-attachments/assets/0ae46499-4404-46dd-83c4-92675accbcf2)

   - **Tag key:** `Patch Group`
   - **Tag value:** `WindowsProd`
5. Click **Add → Patch now**.
   
![image](https://github.com/user-attachments/assets/6fa160cb-acea-40e9-a55a-8cd258882450)

7. Monitor execution status in **AWS-PatchNowAssociation panel**.
![image](https://github.com/user-attachments/assets/a8489bfe-acc8-4fe0-8742-4045cbca0341)
![image](https://github.com/user-attachments/assets/522e1b26-fe24-4c53-97db-3f48dd18e5fa)

---

## **4️⃣ Verifying Patch Compliance**

1. Go to **Systems Manager → Patch Manager**.
2. Click **Dashboard**.
3. Under **Compliance Summary**, verify:
   - ✅ `Compliant: 6` (3 Linux + 3 Windows).
![image](https://github.com/user-attachments/assets/3d0fd5f7-ed33-46d1-972f-ca7a2025fd36)

4. Click **Compliance Reporting** to review detailed instance compliance data.
![image](https://github.com/user-attachments/assets/67282498-c57f-4925-8c7e-7bd9b5d532b7)

6. Select a **Windows instance → Node ID → Patch tab**.
7. Observe installed patches and timestamps.
![image](https://github.com/user-attachments/assets/5207c939-ce48-47c3-bcce-3cba614e1fd3)

---

## **🚀 Conclusion**
Through this project, I successfully: </br>
✅ Patched **Linux instances** using default patch baselines.  
✅ Created a **custom Windows patch baseline** for security updates.  
✅ Used **patch groups** to organize and manage Windows patching.  
✅ Verified patch compliance across all instances.  

This project demonstrates how **AWS Systems Manager Patch Manager** can efficiently handle **patch automation and compliance monitoring** at scale.





