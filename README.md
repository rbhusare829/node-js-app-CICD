# node-js-app-CICD
# Jenkins CI/CD Pipeline for Node.js Application on AWS EC2

**This project demonstrates a complete Jenkins CI/CD pipeline deployed on AWS EC2 for automating the deployment of a Node.js application using PM2.**

## Architecture Overview



## Pipeline Stages

### 1. Environment Setup `(01-setup-env)`

**EC2 Preparation & Tool Installation:**

```bash
sudo apt update
sudo apt install nodejs -y
sudo apt install npm -y
sudo npm install -g pm2
```



### 2. Source Code Management `(02-Pull repository)`

**Git Repository Configuration:**

Repository URL: `https://github.com/rbhusare829/node-js-app-OCD.git`

Branch: `main`

Workspace: `/var/lib/jenkins/workspace/02-Pull repository`


### 3. Dependency Installation (03-install dependencies)

**Application Dependencies:**
```bash
cd "/var/lib/jenkins/workspace/02-Pull repository"
npm install
```

### 4. Application Deployment (04-Deploy application)

**PM2 Deployment Strategy:**

```bash
cd "/var/lib/jenkins/workspace/02-Pull repository"
pm2 start app.js --name node-app || pm2 restart node-app
```


## AWS EC2 Setup Instructions

### Step 1: Launch EC2 Instance

1.AMI Selection: Ubuntu Server 22.04 LTS

2.Instance Type: t2.medium (recommended for Jenkins)

3.Security Group Configuration:

```bash
Type        Protocol    Port    Source
SSH         TCP         22      Your IP
HTTP        TCP         80      0.0.0.0/0
Custom TCP  TCP         8080    0.0.0.0/0 (Jenkins)
Custom TCP  TCP         3000    0.0.0.0/0 (Node.js App)

```
### Step 2: EC2 Instance Preparation

```bash
# Connect to your EC2 instance
ssh -i "your-key.pem" ubuntu@your-ec2-ip

# Update system and install Java (Jenkins dependency)
sudo apt update
sudo apt install openjdk-11-jdk -y

# Install Jenkins
wget -q -O - https://pkg.jenkins.io/debian/jenkins.io.key | sudo apt-key add -
sudo sh -c 'echo deb http://pkg.jenkins.io/debian-stable binary/ > /etc/apt/sources.list.d/jenkins.list'
sudo apt update
sudo apt install jenkins -y

# Start Jenkins service
sudo systemctl start jenkins
sudo systemctl enable jenkins
```
### Step 3: Access Jenkins

1.Get initial admin password:

```bash
sudo cat /var/lib/jenkins/secrets/initialAdminPassword
```
2.Access Jenkins: `http://your-ec2-ip:8080`

3.Complete initial setup and install suggested plugins

4.Access Node :`http://your-ec2-ip:3000`

## Jenkins Pipeline Configuration

### Creating Pipeline Jobs

1.Navigate to Jenkins Dashboard

2.Create New Item for each stage:

  Freestyle project for each of the 4 stages

  Configure build steps as shell commands

### External Access

**Access your application from browser:**

```bash
http://your-ec2-ip:3000
```


## Cost Optimization

### EC2 Cost Saving Tips

1.Use Spot Instances for development

2.Implement auto-scaling based on load

3.Use smaller instance types for development

4.Schedule instance shutdown during off-hours


## Success Metrics

Deployment Time: Reduced from manual to automated (2 minutes)

Reliability: PM2 ensures application availability

Rollback Capability: PM2 ecosystem management

Monitoring: Built-in logging and process management

**This setup provides a robust, scalable CI/CD pipeline on AWS EC2, automating the entire deployment process from code commit to production deployment.**
