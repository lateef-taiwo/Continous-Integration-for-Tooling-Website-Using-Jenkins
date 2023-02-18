# TOOLING-WEBSITE-DEPLOYMENT-AUTOMATION-WITH-CONTINUOUS-INTEGRATION-USING-JENKINS
In this project, we are going to start automating part of our routine tasks with a free and open-source automation server – Jenkins. It is one of the most popular CI/CD tools.
According to Circle CI, Continuous integration (CI) is a software development strategy that increases the speed of development while ensuring the quality of the code that teams deploy. Developers continually commit code in small increments (at least daily, or even several times a day), which is then automatically built and tested before it is merged with the shared repository.
In our project we are going to utilize Jenkins CI capabilities to make sure that every change made to the source code in GitHub `https://github.com/<yourname>/tooling` will be automatically updated to the Tooling Website.

---------
__________

## Task
Enhance the architecture prepared in [the last project](https://github.com/lateef-taiwo/LOAD-BALANCER-SOLUTION-WITH-APACHE) by adding a Jenkins server, and configuring a job to automatically deploy source code changes from Git to the NFS server.

## Architecture Diagram
Here is what your updated architecture will look like upon completion of this project:

![diagram](./images/Architecture.png)

----------
__________

### INSTALL AND CONFIGURE JENKINS SERVER
### Step 1 – Install the Jenkins server
* Create an AWS EC2 server based on Ubuntu Server 20.04 LTS and name it "Jenkins server"

  ![ec2](./images/ec2.png)

* Install JDK (since Jenkins is a Java-based application)

  `sudo apt update`
   
   ![update](./images/update.png)

   `sudo apt install default-jdk-headless`

   ![jdk](./images/jdk.png)

* Install Jenkins
  
        wget -q -O - https://pkg.jenkins.io/debian-stable/jenkins.io.key | sudo apt-key add -
        sudo sh -c 'echo deb https://pkg.jenkins.io/debian-stable binary/ > \
            /etc/apt/sources.list.d/jenkins.list'
        sudo apt update
        sudo apt-get install jenkins

   ![install](./images/installs.png)

   ![install](./images/install%20jenkins.png)

* Check if Jenkins is up and running. `sudo systemctl status jenkins`
  ![systemctl](./images/systemctl.png)

* By default Jenkins server uses TCP port 8080 – open it by creating a new Inbound Rule in your EC2 Security Group.

   ![sec group](./images/sec%20groups.png)

* Perform initial Jenkins setup.
From your browser access `http://<Jenkins-Server-Public-IP-Address-or-Public-DNS-Name>:8080`.
You will be prompted to provide a default admin password.

 ![jenkins](./images/jenkins%20installed.png)
  
 ![pass](./images/pass.png)

* Then you will be asked which plugings to install – choose suggested plugins.
   
   ![plugin](./images/plugin%20install.png)

* Once plugin installation is done – create an admin user and you will get your Jenkins server address (url), then click save and finish. The installation is completed!

![user](./images/user.png)

![installed](./images/installed.png)

![complete](./images/complete.png)

--------
________

### Step 2 – Configure Jenkins to retrieve source codes from GitHub using Webhooks
Here, I will configure a simple Jenkins job/project (these two terms can be used interchangeably). This job will be triggered by GitHub webhooks and will execute a ‘build’ task to retrieve codes from GitHub and store it locally on Jenkins server.

* Enable webhooks in your GitHub repository settings.

  ![web hook](./images/webhook.png)

* Go to Jenkins web console, click "New Item" and create a "Freestyle project"

  ![free style](./images/free%20style.png)

* To connect your GitHub repository, you will need to provide its URL, you can copy it from the repository itself.

  ![copy](./images/git%20hub%20copy.png)

* In the configuration of your Jenkins freestyle project choose Git repository, and provide the link to your Tooling GitHub repository and credentials (user/password) so Jenkins could access files in the repository.

  ![config](./images/jenkins%20config.png)

* Save the configuration and let us try to run the build. For now, we can only do it manually.
Click the "Build Now" button, if you have configured everything correctly, the build will be successful and you will see it under `#1`

  ![build](./images/build%20now.png)

* You can open the build and check in "Console Output" if it has run successfully.
  
  ![console](./images/console%20output.png)

