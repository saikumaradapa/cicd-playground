# Jenkins Setup on Ubuntu 24.04

1.  **Create a new Ubuntu 24.04 instance**\
    Launch a fresh Ubuntu 24.04 EC2 instance (or VM).

2.  **Allow HTTP traffic**\
    Edit the instance's Security Groups to allow inbound HTTP (port 80)
    and Jenkins port 8080 if needed.

3.  **Install Java 21.0.3**

    ``` bash
    sudo apt update
    sudo apt install -y fontconfig openjdk-21-jre
    java -version
    ```

4.  **Install Jenkins**

    ``` bash
    sudo wget -O /etc/apt/keyrings/jenkins-keyring.asc https://pkg.jenkins.io/debian-stable/jenkins.io-2023.key
    echo "deb [signed-by=/etc/apt/keyrings/jenkins-keyring.asc] https://pkg.jenkins.io/debian-stable binary/" | sudo tee /etc/apt/sources.list.d/jenkins.list > /dev/null
    sudo apt-get update
    sudo apt-get install -y jenkins
    ```

5.  **Access Jenkins UI**\
    Open `http://<public_ip>:8080` in your browser to reach the Jenkins
    console.

6.  **Get one-time admin password**

    ``` bash
    sudo cat /var/lib/jenkins/secrets/initialAdminPassword
    # example: 19a61d0bc52e4ed19800fbcadacdba10
    ```

7. **Install Suggested Plugins**  
   Typical suggested plugins include:

   | Category                 | Example Plugins                                                                 |
   |--------------------------|---------------------------------------------------------------------------------|
   | **Pipelines & Build**    | **Pipeline** (workflow-aggregator, includes *Pipeline: Groovy*, *Pipeline: Stage View*) |
   | **SCM (Source Control)** | **Git**, **GitHub**, **GitHub Branch Source**, **Subversion**                   |
   | **UI / Management**      | **Folders**, **Build Timeout**, **Timestamper**                                  |
   | **Agents / Nodes**       | **SSH Build Agents** (a.k.a. SSH Slaves)                                        |
   | **Triggers & Integrations** | **Email Extension**, **Mailer**, **Gradle**, **Credentials Binding**         |
   | **Utilities**            | **Matrix Authorization Strategy**, **Command Agent Launcher**, **JUnit**, **Workspace Cleanup** |



8.  **Install Docker**

    ``` bash
    sudo apt update
    sudo apt install -y docker.io
    ```

9.  **Grant Docker access to Jenkins & Ubuntu users, then restart Docker
    daemon**

    ``` bash
    sudo usermod -aG docker jenkins
    sudo usermod -aG docker ubuntu
    sudo systemctl restart docker
    ```

10. **Verify Docker access**

    ``` bash
    sudo su - jenkins   # switch to jenkins user
    docker run hello-world
    ```

11. **Restart Jenkins** Visit:

    ```html
    http://<public_ip>:8080/restart
    ```

12. **Install Docker Pipeline plugin**\
    Go to *Manage Jenkins → Manage Plugins → Available* and install
    **Docker Pipeline**.
