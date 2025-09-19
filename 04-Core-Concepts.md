# Jenkins Administration and Core Concepts

This guide covers essential Jenkins administration activities, including managing variables, handling job executors, restoring deleted jobs, and configuring passwordless login.

---

## **Variables in Jenkins** 📝

Variables in Jenkins are used to store values that can be reused and changed dynamically. There are two main types: **user-defined variables** and **Jenkins environment variables**.

### User-Defined Variables

These are variables that you create yourself. They can be either **local** or **global**.

* **Local Variables:** These are defined within a specific job and only work for that single job.
    * **How to use:**
        1.  In a Free-style project, go to the **Build Steps** section.
        2.  Add an **"Execute shell"** build step.
        3.  Declare the variable and use it. For example:
            ```shell
            COURSE="DevOps"
            echo "This is the $COURSE Course."
            ```
        4.  When you build the job, Jenkins will use the value assigned to `COURSE`. You can change the value in the job configuration to see a different result.

* **Global Variables:** These are available to all jobs across your Jenkins instance. This is useful for values that are common to many jobs, such as repository URLs or API keys.
    * **How to set:**
        1.  Navigate to **Dashboard** -> **Manage Jenkins** -> **System**.
        2.  Under **"Global properties"**, check the **"Environment variables"** box.
        3.  Click **"Add"** and provide a `Name` (e.g., `COURSE`) and `Value` (e.g., `DevOps`).
        4.  Save the changes.
    * **How to use:**
        * In any job's shell script, you can now access the variable directly using `$COURSE`.

---

### Jenkins Environment Variables

These are pre-defined variables automatically provided by Jenkins for each build. They contain useful information about the current job, such as the build number, job name, and workspace path. You cannot define or change these variables.

* **How to use:**
    1.  Create a new Free-style job.
    2.  Add an **"Execute shell"** build step.
    3.  Access the variables directly. For example:
        ```shell
        echo "The current build number is $BUILD_NUMBER."
        echo "The job name is $JOB_NAME."
        ```
* **To see all available variables:** Run the `printenv` command in a shell build step.

---

## **Managing Build Executors** 🚀

Build executors are the number of jobs that Jenkins can run concurrently on a node. By default, Jenkins runs jobs sequentially.

* **To change the number of executors:**
    1.  Go to **Dashboard** -> **Manage Jenkins** -> **System**.
    2.  Locate the **"Number of executors"** field and change the value. The default is typically 2.
    3.  Save the changes. This will allow Jenkins to run that many jobs in parallel on the main node.

* **Enabling concurrent builds for a specific job:**
    * For a single job, you can override the sequential behavior.
    1.  Go to the specific job's configuration.
    2.  Under **"General"**, check the box for **"Execute concurrent builds if necessary"**.
    3.  This allows multiple instances of the same job to run at the same time.

---

## **Restoring a Deleted Job** ⏪

Accidentally deleting a job is a common mistake, but it can be restored with the **Job Configuration History Plugin**.

1.  **Install the plugin:**
    * Go to **Manage Jenkins** -> **Manage Plugins**.
    * In the **"Available plugins"** tab, search for **"Job Configuration History"**.
    * Install the plugin.

2.  **Restore a job:**
    * The plugin only tracks changes after it is installed. Any jobs created or deleted before the plugin's installation cannot be restored.
    * After a job has been deleted, go to the Jenkins dashboard.
    * Click on **"Job Config History"** in the left-hand navigation. 
    * You will see a list of recent changes, including deleted jobs.
    * Find the deleted job and click the **"Restore"** button to bring it back.

---

## **Configuring Passwordless Login** 🔑

For development or testing environments, you might want to disable the security login to access Jenkins directly without a username and password.

⚠️ **Warning:** This is not recommended for production environments as it poses a significant security risk.

* **How to set up:**
    1.  Locate the `config.xml` file on your Jenkins server.
    2.  The file is typically found at `/var/lib/jenkins/config.xml`. You can use `find / -name config.xml` to locate it.
    3.  Open the file with a text editor (e.g., `vi` or `nano`).
    4.  Find the `<useSecurity>` tag and change its value from `true` to `false`.
        ```xml
        <useSecurity>false</useSecurity>
        ```
    5.  Save the file.
    6.  Restart the Jenkins service: `sudo systemctl restart jenkins.service`.
    7.  You can now access the Jenkins dashboard directly from your browser without a login prompt.

---

## **Changing the Jenkins Port** ⚙️

By default, Jenkins runs on port 8080. You may need to change this if another service is using the same port.

* **How to change:**
    1.  Locate the Jenkins service file. It's often at `/usr/lib/systemd/system/jenkins.service` or `/etc/default/jenkins`. You can use the `find` command to locate it.
    2.  Open the file with a text editor.
    3.  Look for a line that defines the Jenkins port (e.g., `Environment="JENKINS_PORT=8080"` or a command-line argument like `--httpPort=8080`).
    4.  Change the port number to your desired value (e.g., `8069`).
    5.  Reload the systemd daemon to recognize the changes: `sudo systemctl daemon-reload`.
    6.  Restart the Jenkins service: `sudo systemctl restart jenkins.service`.
    7.  Jenkins will now be accessible on the new port.
