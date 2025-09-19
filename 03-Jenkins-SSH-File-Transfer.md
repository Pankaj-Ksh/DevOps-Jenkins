# 🧩 Jenkins Remote File Transfer & Automation

This project focuses on setting up a CI/CD pipeline to securely transfer files from a Jenkins server to a remote server.  
It involves configuring SSH connections, managing user permissions, and automating the file transfer process within a Jenkins job.  
The goal is to streamline deployment workflows and enhance system security through passwordless authentication.

---

## 🎯 Objective
- Securely connect a Jenkins server to a remote Amazon Linux 2 instance.  
- Automate the process of copying project files to a designated remote directory.  
- Implement passwordless SSH authentication to improve security and efficiency.  

---

## 🔄 Workflow Summary
The workflow began with generating an SSH key pair on the Jenkins server.  
The public key was then copied to the remote server, establishing a trusted, passwordless connection.  
A Jenkins freestyle job was configured using the **Publish Over SSH** plugin to define the source files and the destination on the remote machine.  
Finally, a build was triggered to execute the file transfer.  

---

## 🛠 Steps Performed
1. **SSH Key Generation**  
   - Ran `ssh-keygen` on the Jenkins server to create a key pair.  

2. **SSH Agent Setup**  
   - Used `eval $(ssh-agent -s)` and `ssh-add MyKey.pem` to add the private key to the SSH agent for secure use.  

3. **Key Transfer**  
   - Transferred the public key to the remote server using `ssh-copy-id`.  

4. **Jenkins Plugin Installation**  
   - Installed the **Publish Over SSH** plugin via *Manage Jenkins → Plugins*.  

5. **Jenkins SSH Configuration**  
   - Configured the Jenkins SSH server settings by adding the remote server’s details and pasting the private key data (`cat id_rsa`) from the Jenkins server.  

6. **Job Creation**  
   - Created a new freestyle Jenkins job and added a build step to **Send files or execute commands over SSH**, specifying the source and remote directories.  

7. **Build Execution**  
   - Executed the job to perform the automated file transfer.  

---

## ✅ Outcome
The project successfully established a **robust and secure method** for transferring files from Jenkins to a remote server.  
The automated process eliminates manual intervention, reduces the risk of human error, and ensures consistency in deployments.  
The configuration of passwordless SSH login and the use of variables demonstrated a solid understanding of modern DevOps practices.  

---

## 📚 Key Learnings & Observations
- **Security First**: Using passwordless SSH is crucial for automating tasks in a secure manner.  
- **Plugins are Powerful**: The *Publish Over SSH* plugin simplifies complex tasks and provides a GUI for a typically command-line-driven process.  
- **Automation Saves Time**: Automating file transfers is a fundamental step in building efficient CI/CD pipelines.  

---

## 🌍 Real-World Use Case
This workflow is a **cornerstone of continuous deployment (CD)**, where new application builds are automatically deployed to a test or production environment after successful build and testing.  
It's used by companies of all sizes to push code updates to servers, ensuring that the latest features and bug fixes are always available to users.  

---

# Jenkins SSH File Transfer – Mistakes & Fixes Log ✨

| ❌ Mistake / Error | ✅ Fix / Solution |
|-------------------|------------------|
| Confused whether `.pem` is public or private key 🔑 | Understood that `.pem` is the **private key** from AWS, and the public key is placed in `~/.ssh/authorized_keys` |
| Used wrong `ssh-copy-id` format (wrong username/hostname) 🌐 | Corrected to proper username and hostname (e.g., `ec2-user@<ip>`) |
| Jenkins job finished with “Transferred 0 file(s)” 📂 | Changed build step to **Send files or execute commands over SSH** |
| Assumed files were inside `pankajTest` directory but they were not 📁 | Created files in the correct directory and matched the path properly |
| Tried checking `/var/lib/jenkins/workspace/...` on the remote server 🖥️ | Understood this path exists only on Jenkins server; correct location is the user’s home or remote directory configured in Jenkins |
| Thought `.pem` must always be used for Jenkins automation 🔐 | Learned that `.pem` is only for **first login**; Jenkins uses `id_rsa` and `id_rsa.pub` for automation |

---

## ⚠️ Common Errors in Logs & Meaning

| 📝 Error Message | 🧐 Meaning | 🔧 Fix |
|------------------|-----------|--------|
| `Transferred 0 file(s)` | Jenkins didn’t find files at the given path | Double-check **Source files** path in job config |
| `No such file or directory` | Wrong path on remote server | Verify target directory (default is `/home/ec2-user/`) |
| `Could not resolve hostname` | Wrong username or typo in IP/hostname | Use correct format `ec2-user@<Private-IP>` |
| `Permission denied (publickey)` | Remote server doesn’t have Jenkins public key installed | Run `ssh-copy-id` from Jenkins to remote server |
| `Build step changed build result to SUCCESS` but no files | Jenkins job step misconfigured | Use **Send files or execute commands over SSH** step |

---
## 🔑 Key Takeaways
- `.pem` = AWS private key, only needed for first login.  
- `id_rsa` & `id_rsa.pub` = used for Jenkins automation.  
- Always check the correct username (`ec2-user`) and private IP.  
- Jenkins remote copy default path = `/home/<remote-user>/`.  
- Match file paths correctly between Jenkins workspace and remote server.  

---

## 🏷️ Tags
#Jenkins #DevOps #CICD #Automation #SSH #Deployment #TransferFiles  

---

## MY Hands-On with 📸 Screenshots 
1. **Cloud Server Instances** : Overview of the AWS EC2 console showing Jenkins and Test servers running.  
   <img width="1920" height="980" alt="Screenshot (1648)" src="https://github.com/user-attachments/assets/53fddc37-77c4-4f26-a3a3-3480c409b2e8" />

2. **Jenkins Service Check** : Terminal output confirming Jenkins service is active and running.  
   <img width="1920" height="1080" alt="Screenshot (1649)" src="https://github.com/user-attachments/assets/06094ca6-92af-4b6e-ade3-b0f838f56376" />

3. **SSH Key Pair Creation** : CLI view of generating RSA public and private key pair.
4. **SSH Agent Configuration** : Terminal showing SSH agent started and private key added.
   <img width="1920" height="1080" alt="Screenshot (1661)" src="https://github.com/user-attachments/assets/96377d51-471c-4e38-a4f3-438c1627590b" />

6. **Remote Key Installation** : Successful `ssh-copy-id` execution installing key on remote server.  
   <img width="1920" height="1080" alt="Screenshot (1662)" src="https://github.com/user-attachments/assets/d8e3f540-4913-4a80-8881-0fd6e1899e98" />

7. **Remote Server Validation** : Verified passwordless SSH with `authorized_keys` file on remote server.  
   <img width="1920" height="1080" alt="Screenshot (1663)" src="https://github.com/user-attachments/assets/266dd77f-7aa2-42e2-a67e-39fd6c134757" />

8. **Jenkins Global Management** : Central Manage Jenkins dashboard for admin tasks.  
   <img width="1920" height="1032" alt="Screenshot (1664)" src="https://github.com/user-attachments/assets/9263ef1b-3fed-4d0e-bd66-7e5511bcea76" />

9. **Plugin Search and Discovery** : Available plugins page highlighting Publish Over SSH plugin.  
   <img width="1920" height="1034" alt="Screenshot (1665)" src="https://github.com/user-attachments/assets/9b5e9269-90d2-4e54-8779-0debaf559e78" />

10. **Plugin Installation Progress** : Plugin installation page showing successful installations.  
    <img width="1920" height="1033" alt="Screenshot (1666)" src="https://github.com/user-attachments/assets/d705fcc2-d612-4d85-b9d4-aed854bf1809" />

11. **Jenkins Service Restart** : Jenkins UI displaying restart process to apply changes.  
    <img width="1920" height="1032" alt="Screenshot (1667)" src="https://github.com/user-attachments/assets/d917dd61-7c1f-401e-b154-bc25f02096b1" />

12. **Jenkins Login** : Login page requiring username and password for secure Jenkins access.  
    <img width="1920" height="1032" alt="Screenshot (1668)" src="https://github.com/user-attachments/assets/5fb6dc1c-a2e8-404c-9df9-6a9e7abc5dd4" />

13. **New Job Creation** : Interface for creating a new Jenkins job like Freestyle or Pipeline.  
    <img width="1920" height="1033" alt="Screenshot (1671)" src="https://github.com/user-attachments/assets/2319e14d-fd6b-4244-b940-eeff8345adfe" />

14. **⚠️🛑 Post-Build Actions** : Job configuration section for tasks after a build completes.  
    <img width="1920" height="1032" alt="Screenshot (1672)" src="https://github.com/user-attachments/assets/b78a0edd-3af0-490e-af9a-ae265be2cb98" />

15. **Jenkins Management Console** : Dashboard for configuring system settings, plugins, and security.  
    <img width="1920" height="1031" alt="Screenshot (1674)" src="https://github.com/user-attachments/assets/e5bb9334-2aef-46e8-8bc7-2534afabed3a" />

16. **Secure Key Display** : CLI output showing contents of the SSH private key.  
    <img width="1920" height="1080" alt="Screenshot (1675)" src="https://github.com/user-attachments/assets/4f395a6e-fa99-4505-a5d8-6719d294cb53" />

17. **SSH Server Configuration** : Jenkins Publish Over SSH plugin settings for remote connection.  
    <img width="1920" height="1032" alt="Screenshot (1676)" src="https://github.com/user-attachments/assets/5e547acc-6079-41a9-9797-db6db0381e6d" />

18. **File Transfer Setup** : Build step defining source files, remote directory, and commands.  
    <img width="1920" height="1034" alt="Screenshot (1677)" src="https://github.com/user-attachments/assets/643d78ee-9042-4077-acf5-db9783caba73" />

19. **Job Status Page** : Jenkins job overview showing build history and current status.  
    <img width="1920" height="1032" alt="Screenshot (1678)" src="https://github.com/user-attachments/assets/2e1e7a6a-84dc-4952-8388-8ee3b7e4272b" />

20. **Workspace Directory** : CLI commands for creating files and directories in Jenkins workspace.  
    <img width="1920" height="1080" alt="Screenshot (1680)" src="https://github.com/user-attachments/assets/fd39afd9-3564-4c83-bbb6-9648f73567f1" />

21. **Build Step Selection** : Menu to add new build steps like executing scripts or SSH transfers.  
    <img width="1920" height="1032" alt="Screenshot (1684)" src="https://github.com/user-attachments/assets/f6822638-30b9-4a3e-86db-7eb252078d12" />

22. **File Transfer Configuration** : Build step setup to transfer all Python files from `pankajTest/py/*`.  
    <img width="1920" height="1032" alt="Screenshot (1685)" src="https://github.com/user-attachments/assets/1dd8edc8-9f8c-4b0a-8680-6907ffedd240" />

23. **Modified File Transfer Path** : Source path updated to `py/*` for transferring files from workspace.  
    <img width="1920" height="1033" alt="Screenshot (1686)" src="https://github.com/user-attachments/assets/f0159b82-281a-431f-a15f-b21339311376" />

24. **Successful Build Output** : Jenkins console output showing `Finished: SUCCESS` after job execution.  
    <img width="1920" height="1080" alt="Screenshot (1687)" src="https://github.com/user-attachments/assets/43f0dedd-eb5a-416e-8a66-dfbed3b1bf61" />

25. **Remote Server File Listing** : Remote server `ls` output confirming transferred Python files exist.  
    <img width="1920" height="1080" alt="Screenshot (1690)" src="https://github.com/user-attachments/assets/8acfb3d4-ffe3-48a7-ba5e-6da1f18ca4e6" />
