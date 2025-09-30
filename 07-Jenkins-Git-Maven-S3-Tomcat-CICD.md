# 🚀 Jenkins + Git + Maven + S3 + Tomcat CI/CD Project  

A fully automated **CI/CD pipeline** using **Jenkins Webhook triggers**, integrating **GitHub, Maven, Amazon S3, and Tomcat** for seamless deployments of Java web applications.  
No manual intervention required after setup! 🎯  

---

## 🎯 Objective  
- 🔄 Automate build, test, and deployment of Java applications.  
- 📦 Store artifacts securely in **Amazon S3**.  
- 🌐 Deploy WAR files automatically to **Tomcat Server**.  
- ⚡ Achieve continuous integration & continuous deployment using **GitHub Webhooks**.  

---

## 🏗 Architecture Overview  
1. 🧑‍💻 Developer commits code → GitHub Repo.  
2. 🔔 GitHub Webhook triggers Jenkins job.  
3. 🛠 Jenkins pulls code, builds using **Maven**.  
4. 📤 Artifact (`.war`) uploaded to **S3 bucket**.  
5. 🚢 Jenkins deploys `.war` to **Tomcat server**.  
6. 🌍 Application accessible via browser.  

---

## 🔄 Workflow Summary  
1. Code Push → GitHub ✅  
2. Webhook → Jenkins Trigger 🔔  
3. Maven Build → `.war` created ⚒️  
4. Upload → Amazon S3 📦  
5. Deploy → Tomcat Server 🚀  
6. App Live → Browser 🌐  

---

## ⚙️ Jenkins Freestyle Job Configuration  

1. **Source Code Management**  
   - Git → Add GitHub repository URL  
   - Branch → `main`  

2. **Build Triggers**  
   - Select **GitHub hook trigger for GITScm polling**  

3. **Build Steps**  
   - Execute Maven → `clean package`  

4. **Post-Build Actions**  
   - Upload `.war` to Amazon S3 (via S3 plugin or AWS CLI)  
   - Deploy `.war` to Tomcat (via *Deploy to container* plugin)  

---

## ⚠️ Issues Faced  
- ❌ **S3 Bucket Not Found** → Fixed by creating bucket before job run.  
- 🔑 **Encryption Error** → Disabled SSE-C in advanced settings.  
- 🔔 **Webhook Not Triggering** → Fixed inbound rules (opened 8080) & reconfigured GitHub webhook.  

---

## ✅ Outcome  
- ✅ Fully automated **CI/CD pipeline**.  
- ✅ WAR artifacts stored in **Amazon S3**.  
- ✅ Deployment to **Tomcat** within seconds after GitHub commit.  
- ✅ Zero manual intervention after initial setup.  

---

## 📚 Key Learnings & Observations  
- 🔄 **Webhooks** make CI/CD real-time.  
- 📦 Always pre-create S3 bucket before configuring Jenkins.  
- 🛡️ Secure credentials using Jenkins **Credentials Manager**.  
- ⚡ Debugging **Jenkins Console Output** helps resolve 90% of errors.  

---

## 🌍 Real-World Use Case  
- 📦 Enterprises hosting **Java-based web apps** can automate deployment.  
- 🔄 Teams working on **microservices** can replicate this for each service.  
- ☁️ Enables **hybrid cloud deployments** by combining S3 storage & Tomcat hosting.  

---

## 💡 Tips  
- 📝 Always **test pipeline** with a sample Java app before real project.  
- 🔒 Never hardcode credentials → use **Jenkins Credentials Plugin**.  
- 📡 Use **GitHub → Jenkins Webhook** instead of Poll SCM for faster builds.  
- 🚀 Add **SonarQube** or **JUnit** tests for production-ready pipelines.

---
## 🛠 Steps Performed (with Screenshots)  

### 🔧 Jenkins Setup  
1. **Jenkins Installation Commands** – Installed Jenkins and started service.  
    <img width="1920" height="1080" alt="1" src="https://github.com/user-attachments/assets/304508df-9fe9-46ba-ab2b-60b926c037f0" />

### 🔌 Plugin Installation 
2. **Default Jenkins Plugins View** – Displayed available default plugins.  
    <img width="1920" height="1031" alt="2" src="https://github.com/user-attachments/assets/382c90b1-976d-4f77-8ce2-1d0b90f6b435" />

3. **S3 Publisher Plugin Search** – Installed S3 plugin for publishing artifacts.  
    <img width="1920" height="1028" alt="3" src="https://github.com/user-attachments/assets/f462d375-2376-40a9-b34a-0259d03d9fac" />

### 🔑 Credential Management  
4. **Jenkins Credential Setup** – Added credentials (pankajCred).  
    <img width="1920" height="1031" alt="4" src="https://github.com/user-attachments/assets/c88a7bfe-bd19-4d44-879b-d869721f24ad" />

5. **Amazon S3 Profile Configuration (Empty)** – Initial empty configuration.  
    <img width="1920" height="1032" alt="5" src="https://github.com/user-attachments/assets/f1edd6c1-9299-4a23-b605-5859fdd8cd84" />

6. **AWS IAM User Security Credentials** – IAM user credentials page.  
    <img width="1920" height="1032" alt="6" src="https://github.com/user-attachments/assets/981ab47e-b510-42ee-80db-57c2cc1d904c" />

7. **AWS Access Key Creation - Use Case** – Selected CLI as use case.  
    <img width="1920" height="1031" alt="7" src="https://github.com/user-attachments/assets/72f4bd63-e16b-42dc-ab20-be75b741bca6" />

8. **AWS Access Key Creation - Key Retrieval** – Generated keys displayed.  
    <img width="1920" height="1034" alt="8" src="https://github.com/user-attachments/assets/f712e576-af09-455a-8d7d-3ba3e68f456e" />

### ☁️ AWS S3 Configuration  
9. **AWS S3 Console - Region Selection** – Selected Mumbai (ap-south-1).  
    <img width="1920" height="1031" alt="9" src="https://github.com/user-attachments/assets/58548ece-3a02-44e5-af5c-12f1d05d098b" />

10. **Amazon S3 Profile Configuration (Tested)** – Configured successfully in Jenkins.  
    <img width="1920" height="1031" alt="10" src="https://github.com/user-attachments/assets/164347d7-955c-4f20-93bf-cabcf5b33ea0" />

### ⚙️ Jenkins Job Setup  
11. **Jenkins New Item Configuration** – Created job `Tomcat-Deployment-Job`.  
    <img width="1920" height="1032" alt="11" src="https://github.com/user-attachments/assets/44059f2e-6984-4a4e-94eb-9a43e95ecc38" />

12. **Jenkins Job SCM Configuration** – Linked GitHub repository.  
    <img width="1920" height="1031" alt="12" src="https://github.com/user-attachments/assets/e2af7c69-69c1-4d74-9f32-3e35ba7841ca" />

13. **GitHub Repository and URL** – Repository clone URL.  
    <img width="1920" height="1032" alt="13" src="https://github.com/user-attachments/assets/fa988fd9-c0e9-49f1-85d4-048f7a3d8912" />

14. **Jenkins Job Triggers and Environment** – Configured GitHub webhook trigger.  
    <img width="1920" height="1032" alt="14" src="https://github.com/user-attachments/assets/259acd5c-9214-44dc-9752-fa85ea1f34f8" />

15. **GitHub Webhook Configuration** – Payload URL to Jenkins.  
    <img width="1920" height="1032" alt="15" src="https://github.com/user-attachments/assets/59ce4eb5-2cfd-40cd-898b-59c0a9327545" />

### 🔒 Security & Networking  
16. **AWS Security Group Inbound Rules** – Opened ports (80, 8080).  
    <img width="1920" height="978" alt="16" src="https://github.com/user-attachments/assets/c026fa66-e02d-4575-a728-9647d19c79c0" />

### ⚒️ Build & Deployment  
17. **Jenkins Job Build Steps Configuration** – Added Maven clean package.  
    <img width="1920" height="1032" alt="17" src="https://github.com/user-attachments/assets/7a980b11-55f7-43a5-a5cb-6399f3abb649" />

18. **Jenkins Job Artifact Publishing to S3** – Configured S3 upload.  
    <img width="1920" height="1032" alt="18" src="https://github.com/user-attachments/assets/d3cd43f5-0798-43ef-b104-a4e74e830a6f" />

19. **Jenkins Job WAR File Deployment to Tomcat** – Configured Tomcat deployment.  
    <img width="1920" height="1032" alt="19" src="https://github.com/user-attachments/assets/2ee8f656-6dc3-4cda-b7b1-bd94625d09a6" />

20. **Tomcat Manager Console Access** – Verified Tomcat Manager presence.  
    <img width="1920" height="1080" alt="20" src="https://github.com/user-attachments/assets/76544c30-74eb-49fb-8ffe-ad9135344b8f" />

21. **EC2 Instance Java/Maven Environment Check and Tomcat Access** – Verified environment. 
    <img width="1920" height="1080" alt="22" src="https://github.com/user-attachments/assets/8dd03b85-77f6-4578-b770-103fe0ec9a72" />

22. **EC2 Instances and Tomcat Server Details** – Instance details view.  
    <img width="1920" height="977" alt="21" src="https://github.com/user-attachments/assets/1a9c59e6-2b4b-4aaf-a4f9-a82eac8a71d6" />

23. **Jenkins Job Status After Trigger** – Build in progress.  
    <img width="1920" height="1031" alt="23" src="https://github.com/user-attachments/assets/00869bd2-6ba4-4f9a-842c-f70971f043e8" />

### ⚠️ Errors Faced  
24. **Jenkins Build Failure - No Such Bucket Error** – Occurred when bucket missing.  
    <img width="1920" height="1031" alt="24" src="https://github.com/user-attachments/assets/3bbaa965-2788-4163-9981-d4ebd63f522c" />

25. **AWS S3 Bucket Creation Success** – Confirmed bucket creation.  
    <img width="1920" height="977" alt="25" src="https://github.com/user-attachments/assets/5b3cbbc0-8bab-443c-8265-9444ab6ef206" />

26. **Jenkins Build Failure - Encryption Error** – Fixed by disabling SSE-C.  
    <img width="1920" height="1032" alt="26" src="https://github.com/user-attachments/assets/2fcfa299-0d57-4afb-9309-2961d4fa23e6" />

27. **Jenkins S3 Publish Advanced Settings** – Set advanced S3 options.  
    <img width="1920" height="1032" alt="27" src="https://github.com/user-attachments/assets/56661e3a-32a9-4a83-bcac-34e5b2704464" />

28. **Successful Jenkins Build and Deployment Console Output** – Build SUCCESS.  
    <img width="1920" height="1031" alt="28" src="https://github.com/user-attachments/assets/f8c6220e-3f24-4037-a80b-a36b7b5b3f61" />

29. **Successful Artifact Upload to S3 Folder** – Confirmed upload in S3.  
    <img width="1920" height="979" alt="29" src="https://github.com/user-attachments/assets/d8d6ef47-4955-4a08-9e16-58cc737d46d1" />

30. **Artifact in S3 Bucket target Folder** – WAR uploaded to target folder.  
    <img width="1920" height="978" alt="30" src="https://github.com/user-attachments/assets/970981f9-0ea0-4c23-8178-cd264c674d36" />

### 🌍 Application Deployment
31. **Tomcat Manager After Initial Deployment** – WAR deployed to Tomcat.  
    <img width="1920" height="1032" alt="31" src="https://github.com/user-attachments/assets/e9f58b13-3ea8-4fec-926a-391219aa8138" />

32. **Initial Application Access** – App running in browser.  
    <img width="1920" height="1031" alt="32" src="https://github.com/user-attachments/assets/ae78f730-111c-4daf-b9cd-d13d37ee41a7" />

### 🔄 Pipeline Execution
33. **GitHub Code Change - Index File Update** – Modified index.jsp.  
    <img width="1920" height="1031" alt="33" src="https://github.com/user-attachments/assets/71dd4c15-6779-4efa-aabe-ef6c5f9e67ff" />

34. **Jenkins Build Triggered by Webhook** – Webhook started build automatically.  
    <img width="1920" height="1030" alt="34" src="https://github.com/user-attachments/assets/fb9a6dc4-4ad8-4d35-b9c6-eb3501181ea6" />

35. **Application Access After Webhook Deployment** – App updated in browser.  
      <img width="1920" height="1034" alt="35" src="https://github.com/user-attachments/assets/ec2b7e5e-b884-404d-b13d-b92955171ca8" />

---

## 🏷️ Tags  
`#Jenkins` `#GitHub` `#Maven` `#AWS` `#S3` `#Tomcat` `#CI/CD` `#DevOps`  
