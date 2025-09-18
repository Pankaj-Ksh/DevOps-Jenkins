# 🖥️ Jenkins Node Disk Space Issue and Fix

## ⚠️ Problem
Jenkins automatically marked the built-in node as **offline** during builds because the available disk space on `/tmp` dropped below the default threshold of **1.00 GiB**.

Error message observed:
> Disk space is below threshold of 1.00 GiB. Only 470.19 MiB out of 474.69 MiB left on /tmp.


### ❓ Why did this happen?
- Jenkins has a **Node Monitor → Disk Space Threshold** setting (default = **1 GB**).  
- It continuously checks free space on critical directories like `/tmp`.  
- If free space is **less than 1 GB**, Jenkins marks the node as offline to prevent potential build failures.  
- In this case, `/tmp` had only **470 MB free**, so the node was marked offline.  

---

## 💡 Solution 1: Adjust Jenkins Threshold (Quick Fix)
Instead of requiring 1 GB free space, lower the threshold or disable it.

### 📝 Steps:
1. Go to **Jenkins UI** → `Manage Jenkins` → `Nodes`.  
2. Select **Built-in Node** → `Configure`.  
3. Scroll down to **Disk Space Threshold** (under Node Monitors).  
4. Options:  
   - **Uncheck** it → disables disk monitoring.  
   - Or **lower the threshold** (e.g., from `1.0 GB` → `100 MB`).  
5. Click **Save** and bring the node online.

✅ **Effect:** Jenkins will not mark the node offline even if less than 1 GB is free.  
⚠️ **Risk:** If disk space actually runs out during a build, the build may fail.

---

## 💡 Solution 2: Free Up or Increase Disk Space (Best Practice)
The real issue is lack of space on `/tmp`. Free up or expand disk space to keep Jenkins stable.

### 🧹 Option A: Clean `/tmp`
Many temporary files can be safely removed:
```bash
sudo rm -rf /tmp/*
```

### 🔄 Option B: Restart the Server
Some OS configurations clear `/tmp` on reboot.

---

### 💽 Option C: Expand Disk
- **On AWS EC2:** Increase EBS volume size and extend the partition.  
- **On local VM/Server:** Attach or expand a disk.

---

### 📂 Option D: Move Jenkins Workspace
Configure Jenkins builds to run in a directory with more space, e.g., `/var/lib/jenkins/workspace`.

> 💻 **Demo Note:** For this hands-on demo, I implemented **Option 1** (adjusting Jenkins disk space threshold) to quickly bring the node online.


## Screenshots:
### 1.
<img width="1920" height="1080" alt="Screenshot (1649)" src="https://github.com/user-attachments/assets/7b2062ad-5112-493c-8cf2-d9d690defdbd" />

### 2.
<img width="1920" height="1033" alt="Screenshot (1650)" src="https://github.com/user-attachments/assets/6d0fe3dd-aafe-4eec-b439-b0f2be4f7eff" />

### 3.
<img width="1920" height="1034" alt="Screenshot (1651)" src="https://github.com/user-attachments/assets/50d41f11-e4d2-4db7-b836-c338d8ed8527" />

### 4.
<img width="1920" height="1034" alt="Screenshot (1652)" src="https://github.com/user-attachments/assets/923d4766-6c01-4765-8dcc-b32be7506efb" />

### 5.
<img width="1920" height="1031" alt="Screenshot (1653)" src="https://github.com/user-attachments/assets/3e02b8c6-02de-4229-9163-f64fdf89c2e5" />

### 6.
<img width="1920" height="1032" alt="Screenshot (1654)" src="https://github.com/user-attachments/assets/d718d020-78e2-494d-b9e1-92dfc3752a5a" />

### 7.
<img width="1920" height="1032" alt="Screenshot (1655)" src="https://github.com/user-attachments/assets/7c3f4385-a117-42d8-a754-2b7c8e0b4c80" />

### 8.
<img width="1920" height="1032" alt="Screenshot (1656)" src="https://github.com/user-attachments/assets/e1ee4b7d-7d05-47d4-ab43-44d9b7597d7e" />

### 9.
<img width="1920" height="1032" alt="Screenshot (1657)" src="https://github.com/user-attachments/assets/c230244b-1060-4e38-8c27-70652690875b" />

### 10.
<img width="1920" height="1033" alt="Screenshot (1658)" src="https://github.com/user-attachments/assets/24509f33-efbb-4eff-a0dd-9a353d4e24ee" />
