# 📒 Jenkins Build Triggers & Scheduling Notes

## 1. 🛠 Normal Build (Manual)
- Developer/admin manually triggers the build from Jenkins dashboard.  
- ❌ No automation for time or code change.  

---

## 2. ⏰ Build Periodically (CRON)
- Use CRON expression to run builds at scheduled times.  
- Example: `H/15 * * * *` → Runs every 15 mins.  
- ❌ Limitation:  
  - Does **not check for source code changes**.  
  - Build will run even if code is unchanged.  

---

## 3. 🔄 Poll SCM
- Similar to CRON, but **adds a code-change check**.  
- Example: `20 14 8 7 1` (UTC → 14:20 on 8th July, Monday)  
  - Poll SCM **waits for the schedule to hit**.  
  - It builds **only if changes are found at that time**, not immediately at commit.  
- ✔ Build will only run if **code has changed** in SCM.  
- ❌ Limitation:  
  - Cannot run immediately; must wait for scheduled poll.  

---

## 4. 🚀 Webhook (Recommended)
- Trigger build **immediately** when code is pushed to SCM.  
- **Setup (GitHub → Jenkins):**  
  1. GitHub → Repo → Settings → Webhook → Add Webhook  
  2. **Payload URL:** `http://<JENKINS_SERVER>:8080/github-webhook/`  
  3. Content Type → `application/json`  
- **Jenkins job configuration:**  
  - Source Code Mgmt → Git repo (branch: main/master)  
  - Build Trigger → GitHub hook trigger for GITScm polling  
  - Build Step → Maven `clean package`  
- ✅ Ensure AWS EC2 SG → **All traffic allowed** (GitHub → Jenkins connection).  

---

## 5. 🛑 Throttle Build
- Restrict **number of builds per interval**.  
- Example:  
  - Max builds → 3  
  - Time period → 1 hour  
- Prevents **resource overuse** when frequent triggers occur.  

---

## 6. 🌐 Remote Triggering
- Trigger Jenkins job from **external request** (browser, API, etc.).  
- **Setup:**  
  1. Job → Configure → Build Triggers → Enable *Trigger build remotely*  
  2. Add **Authentication Token** (e.g., `pankajksh`)  
  3. Use URL format:  
     ```
     http://<JENKINS_SERVER>:8080/job/<JOB_NAME>/build?token=<TOKEN>
     ```  
- Example:  
  `http://13.233.214.180:8080/job/firstjob/build?token=pankajksh`  

---

## ✅ Summary
| Trigger Type | Behavior |
|--------------|---------|
| Manual 🛠 | Triggered manually |
| CRON ⏰ | Scheduled, ignores code changes |
| Poll SCM 🔄 | Scheduled + checks for code changes |
| Webhook 🚀 | Instant trigger on push (best for CI/CD) |
| Throttle 🛑 | Limit concurrent/frequent builds |
| Remote Trigger 🌐 | Trigger via API/URL |
