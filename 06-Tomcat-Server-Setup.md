# ⚙️ Tomcat Server Setup and Configuration

This file outlines the steps for installing, configuring, and starting an **Apache Tomcat 9** server, including setting up user credentials for the **Tomcat Manager** for remote deployment.

---

## 1. Installation of Java and Tomcat

1.  **Install Java:** Install the required Java runtime environment (Amazon Corretto 17).
    ```bash
    dnf install -y fontconfig java-17-amazon-corretto
    ```

2.  **Download Tomcat 9:** Download the specified version of the Apache Tomcat 9 archive.
    ```bash
    wget https://downloads.apache.org/tomcat/tomcat-9/v9.0.109/bin/apache-tomcat-9.0.109.tar.gz
    ```

3.  **Extract the Archive:** Extract the Tomcat files.
    ```bash
    tar -xvzf apache-tomcat-9.0.109.tar.gz
    ```

---

## 2. Configure Tomcat Manager Access

1.  **Edit `tomcat-users.xml`:** Navigate to the configuration directory and open the user file to define roles and users.
    ```bash
    cd apache-tomcat-9.0.109/conf/
    vi tomcat-users.xml
    ```

2.  **Add User and Roles:** Inside the `<tomcat-users>` block, add the following lines to create a user named **`pankaj`** with the specified password and roles needed for the Manager GUI and script access.
    ```xml
    <role rolename="manager-gui"/> 
    <role rolename="manager-script"/> 
    <user username="pankaj" password="pankaj123" roles="manager-gui,manager-script"/>
    ```

---

## 3. Enable Remote Manager Access

To allow the Jenkins server to remotely deploy applications, you must remove the IP address restriction from the **Manager** application's `context.xml`.

1.  **Edit `context.xml` for Manager:** Navigate to the Manager application's metadata directory and open its configuration file.
    ```bash
    cd apache-tomcat-9.0.109/webapps/manager/META-INF/
    vi context.xml
    ```

2.  **Remove IP Restrictions:** Locate and **delete or comment out** the lines that contain the `RemoteAddrValve` restriction. These are typically lines 21 and 22, which look similar to the following (delete these lines):
    ```xml
    <Valve className="org.apache.catalina.valves.RemoteAddrValve"
           allow="127\.\d+\.\d+\.\d+|::1|0:0:0:0:0:0:0:1" />
    ```

---

## 4. Start Tomcat and Verify

1.  **Start the Server:** Navigate to the `bin` directory and execute the startup script.
    ```bash
    cd apache-tomcat-9.0.109/bin/
    sh startup.sh
    ```

2.  **Verify Access: ( Use in Jenkins-Server )** Use `curl` to confirm the **Tomcat Manager** is accessible with the new credentials. Replace `<private-ip>` with the server's actual private IP address.
    ```bash
    curl -u pankaj:pankaj123 http://<private-ip>:8080/manager/text/list
    ```

3. **Verify Web Access via Browser:** Open a web browser and navigate to the Tomcat server's default page using its Public IP address and port 8080.
    ```
    http://<public-ip>:8080
    ```


## Ready to use Script 
```
dnf install java-17-amazon-corretto -y

wget https://downloads.apache.org/tomcat/tomcat-9/v9.0.109/bin/apache-tomcat-9.0.109.tar.gz

tar -zxvf apache-tomcat-9.0.109.tar.gz

sed -i '55  a\<role rolename="manager-gui"/>' apache-tomcat-9.0.109/conf/tomcat-users.xml
sed -i '56  a\<role rolename="manager-script"/>' apache-tomcat-9.0.109/conf/tomcat-users.xml
sed -i '57  a\<user username="pankaj" password="pankaj123" roles="manager-gui, manager-script"/>' apache-tomcat-9.0.109/conf/tomcat-users.xml

sed -i '21d' apache-tomcat-9.0.109/webapps/manager/META-INF/context.xml
sed -i '21d' apache-tomcat-9.0.109/webapps/manager/META-INF/context.xml

sh apache-tomcat-9.0.109/bin/startup.sh

```
