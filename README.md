# Automated Build and Test with Jenkins on EC2 Instance

This project aims to set up Jenkins to automatically build and test code changes from a GitHub repository using a Jenkinsfile. The Git Flow branching model is applied to manage the development process.

## Project Setup

### Tools Needed
- EC2 instance on AWS
- Jenkins
- GitHub account
- Git installed on the Jenkins server
- GitFlow for later tasks

## Installation and Configuration Guide
### 1. **Prepare GitHub Repository:**
   - Create or select a GitHub repository.
   - Initialize the repository with a README.md.
   - Ensure the repository contains at least two branches:
     - `develop` for ongoing development.
     - `master` (or `main`) for stable releases.
   - Apply the Git Flow model.

### 2. **Installing Git & Git Flow on EC2:**
```bash
#Installing git
$ sudo apt install git

$ git --version
```
```bash
#Installing Git Flow
$ git clone -b feature/https-remote https://github.com/jgonggrijp/gitflow.git

$ curl -OL https://raw.github.com/jgonggrijp/gitflow/develop/contrib/gitflow-installer.sh

$ ls

$ chmod +x gitflow-installer.sh

$ sudo ./gitflow-installer.sh

$ sudo apt update
```
### 3. **Cloning Github Repo:**
```bash
# clone the repo
$ git clone <repo_url>
$ ls
$ cd <cloned repo folder>

#initialize git & git flow
$ git init
$ git flow init  # Let the default branch 'develop'
$ git branch  # to check the assigned branch, 

# If the branch didn't change to 'develop'
$ git checkout develop
```
### Creating Jenkins file example and push it to the repo:
```bash
$ vim Jenkinsfile

# press 'i'

# paste these lines in the file
pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                echo 'Building..'
            }
        }
        stage('Test') {
            steps {
                echo 'Testing..'
            }
        }
        stage('Deploy') {
            steps {
                echo 'Deploying....'
            }
        }
    }
}


# Save and Exit
# press 'ESC'
# then write ':wq'
```
```bash
# Push the file to GitHub Repo
$ git add Jenkinsfile
$ git commit -m "Add Jenkinsfile"
$ git push <repo_url> develop

# check your develop branch on github repo
```

### 4. **Installing Jenkins on EC2:**

```bash
$ cd
$ sudo apt update
```
```bash
#Install Java
$ sudo apt install fontconfig openjdk-17-jre
$ java --version
```

```bash
$ sudo wget -O /usr/share/keyrings/jenkins-keyring.asc   https://pkg.jenkins.io/debian-stable/jenkins.io-2023.key
```
```bash
$ echo "deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc]"   https://pkg.jenkins.io/debian-stable binary/ | sudo tee   /etc/apt/sources.list.d/jenkins.list > /dev/null
```
```bash
$ sudo apt-get update 
```
```bash
# Install Jenkins
$ sudo apt-get install jenkins -y
```
```bash
#Setting up a firewall rule using Uncomplicated Firewall (UFW) to allow incoming connections on port 8080
$ sudo ufw allow 8080
```
### Note :
**Add Inbound rule in the security group of the EC2 instance:**
- Type: Custom TCP
- Port Range: 8080
- source: 0.0.0.0/0

### Accessing Jenkins:
- Go to web browser and write : http://<your-jenkins-url>:8080
- run the following command on your terminal to get Jenkins password
```bash
$ sudo cat /var/lib/jenkins/secrets/initialAdminPassword
```
- Copy password to Jenkins tab and sign in


### 5. **Configure Jenkins:**
   - **Install necessary plugins in Jenkins such as Git and Pipeline.**
   - **Connect Jenkins to GitHub.**
        - Go to "Manage Jenkins" > "Manage Plugins" > "Available" and install "GitHub Integration Plugin".
     - Set up credentials in Jenkins for GitHub (username and token).
   - **Create a new pipeline job.**
     - Select "New Item", name your pipeline (e.g., "GitHub Pipeline"), and choose "Pipeline" as the type.
     - In the pipeline configuration, select "Pipeline script from SCM" and choose "Git" as the SCM.
     - Enter the repository URL and credentials.
     - Specify the branch to build (e.g., */develop).

### 6. **Implement Webhooks for Continuous Integration:**
**Set up webhooks in GitHub to trigger Jenkins builds on push events.**
- In GitHub, go to your repository settings and select **"Webhooks"**.
- **Add a new webhook:**
   - Payload URL: http://<your-jenkins-url>:8080/github-webhook/
   - Content type: **application/json**
   - Select **"Just the push event"**.
   - Ensure the webhook is active
### **With the webhook, Jenkins will trigger a new build every time changes are pushed to the connected branch.**

### 7. **Testing and Validation:**
   - Push a change to the `develop` branch and verify Jenkins triggers a build.
   
- Check the Jenkins dashboard for build status and output.
## Usage

1. Clone the GitHub repository to your local machine or EC2.
2. Make changes to the codebase and push them to the `develop` branch.
3. Jenkins will automatically trigger a build and execute the defined pipeline stages.
4. Check the Jenkins dashboard for build status and output.

## Contributors

- [Maximous Elkess Ayoub](https://github.com/maxiayoub)

  - LinkedIn: **Maximous ElKess Ayoub**
  - Github: **https://github.com/maxiayoub**
