# SE4A1-Projects
OS Lab End Semester GitHub Actions (Building and testing Java with Maven) project

## Java Maven

Maven is a build automation tool used primarily for Java projects. It is a powerful project management tool that is based on POM (project object model). It is used for projects build, dependency and documentation.
> Maven makes a project easy to build also provides uniform build process (maven project can be shared by all the maven projects)


The common features of Maven are following:

1. Dependency management including automatic updating
2. Consistent usage across all projects
3. Instant access to new features with little or no extra configuration

## Prerequisites

- Java


### Installing Java

    sudo apt update
    sudo apt install openjdk-11-jdk
    
### Verifying Installation

    java -version

As we can see Java 11 was installed successfully: 

    ali@pop-os:~$ java -version
    openjdk version "11.0.15" 2022-04-19
    OpenJDK Runtime Environment (build 11.0.15+10-Ubuntu-0ubuntu0.22.04.1)
    OpenJDK 64-Bit Server VM (build 11.0.15+10-Ubuntu-0ubuntu0.22.04.1, mixed mode, sharing)

    
## Installing Jenkins

The version of Jenkins included with the default Ubuntu packages is often outdated. To ensure we have the latest fixes and features, we will use the project-maintained packages to install Jenkins.

First, we will add the repository key to the system:

    wget -q -O - https://pkg.jenkins.io/debian-stable/jenkins.io.key | sudo apt-key add -

After the key is added the system will return with OK.

Next, we will add the Debian package repository address to sources.list which maintains all repositories:

    sudo sh -c 'echo deb http://pkg.jenkins.io/debian-stable binary/ > /etc/apt/sources.list.d/jenkins.list'
    
After both commands have been entered, we’ll run update so that apt will use the newly added repository:

    sudo apt update

Finally, we’ll install Jenkins and its dependencies:

    sudo apt install jenkins
    
### Starting Jenkins

Let’s start Jenkins by using **systemctl**:

    sudo systemctl start jenkins

Since **systemctl** doesn’t display status output, we’ll use the status command to verify that Jenkins started successfully:

    sudo systemctl status jenkins

If everything went well, the beginning of the status output shows that the service is active and configured to start at boot:

    ● jenkins.service - Jenkins Continuous Integration Server
     Loaded: loaded (/lib/systemd/system/jenkins.service; enabled; vendor preset: enabled)
     Active: active (running) since Fri 2022-07-08 17:41:05 PKT; 1h 31min ago
    Main PID: 1034 (java)
      Tasks: 51 (limit: 18976)
     Memory: 614.0M
        CPU: 36.170s

Now that Jenkins is up and running, we need to adjust our firewall rules so that we can access it from a web browser

### Opening Firewall

By default, Jenkins runs on port 8080. We’ll open this port using **ufw**:

    sudo ufw allow 8080
    
To check ufw’s status to confirm the new rules:

    sudo ufw status
    
We can see that port **8080** is allowed:

    ali@pop-os:~$ sudo ufw status
    Status: active

    To                         Action      From
    --                         ------      ----
    8080                       ALLOW       Anywhere                  
    8080 (v6)                  ALLOW       Anywhere (v6)            


## Setting up Jenkins


To set up  installation, we will visit Jenkins on its default port i.e. 8080, using server IP address (in our case it is localhost): http://localhost:8080

We will receive the Unlock Jenkins screen, which displays the location of the initial password:

![Jenkins unlock screen](/steps/11.png)

In the terminal window, we will use the **cat** command to display the password:

    sudo cat /var/lib/jenkins/secrets/initialAdminPassword
    
We will copy the password from the terminal and paste it into the Administrator password field, then click Continue.

The next screen show us the option of installing suggested plugins or selecting specific plugins:

![installation method selection](/steps/13.png)

We’ll click the Install suggested plugins option, which will immediately begin the installation process.

![installation begin](/steps/14.png)

When the installation is complete, we'll be prompted to set up the first administrative user. Or we can use the default admin user

![user selection](/steps/15.png)

We will setup our user i.e. we will enter the name and password for our user:

![user selection](/steps/16.png)

We’ll receive an Instance Configuration page that will ask us to confirm the preferred URL for our Jenkins instance. We will proceed with this as this is the URL we want.

![Instance cofiguration](/steps/17.png)

After confirming, click Save and Finish. We’ll receive a confirmation page confirming that **“Jenkins is Ready!”**:

![Jenkins ready](/steps/18.png)

Click Start using Jenkins to visit the main Jenkins dashboard:

![Jenkins Dashboard](/steps/19.png)

At this point, we have completed installation of Jenkins.


## Defining Jenkins Pipeline


To create a Jenkins job and create a pipeline we will click on **"Create a job"**

![Jenkins Dashboard](/steps/21.png)

Next we will enter the name of the Job. We will call it **"Node Calculator”**. 
Then we will select Freestyle project and finally click **Ok**.

![Jenkins Freestyle project](/steps/22.png)

Next we will enter the description of our Job and enter the github url of our project.

![Job details](/steps/24.png)

Then under Source Code Management tab, We will enter our repository URL i.e. https://github.com/alifaroo-q/SE4A1-Projects.git <br>
And enter our Branches to build i.e. **/JenkinProject**

![Repo details](/steps/25.png)

Next under Build Trigger tab, We will check the **Github hook trigger for GITSCM polling** box

![github hook](/steps/26.png)

Next under Build tab, we will click on **“Add build step”** <br>
Then click on “Execute shell” and add the following commands (we are using npm because this is a nodejs project)

    npm install
    npm run build
    npm test

![build details](/steps/27.png)

Then click on **"Apply"** and then **"Save"**


Now that we have created the pipeline as well connected it to our github repository and defined building & testing steps, we now will trigger the build process.

## Jenkins Build Process

In the main dashboard screen, Click the **Build Now** button on the left-hand side to build the source code from github repository.

![Build Now](/steps/29.png)

After clicking on Build now, we can see the status of the build we run under **Build History**.

![Build status](/steps/30.png)

Click on the build number i.e. **#1** to view the status of build that we ran.

![Build status for #1](/steps/31.png)

Click on **Console Output** on the left-hand side to see the status of the build we run.

![Console output](/steps/32.png)

Finaly we can see **Finished: SUCCESS** <br> 
It means that our pipeline ran successfully as it builded and tested our github project without any error

![Finished](/steps/34.png)

In sum, we have executed our project hosted on GitHub. Jenkin pulls the code from the remote repository and builds continuously each time we commit on our repository.


## Conclusion

We have successfully installed Jenkins and all of it's dependencies and successfully configured it. After that we created a simple calculator that was written on JavaScript with NodeJS and created Testcases for it. Finally we created CI/CD pipeline on Jenkins for our calculator and successfully builded and tested it.

<br>

> This pipeline will only run when we explicitly click on **Build Now** in our pipeline. To make it run automatically on every commit that is made on our github repository, we will have to host our Jenkins server on dedicated machine or some cloud hosted service like **Amazon Web Services (AWS)** and add IP Address of that server in our Github reposity as WebHook, so that Github can notify our Jenkins server everytime a new commit is made.














