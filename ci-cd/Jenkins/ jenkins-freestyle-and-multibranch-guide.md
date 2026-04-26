# 🚀 Jenkins Freestyle and Multibranch Guide

## 🎯 Objective

Build, configure, and understand two of the most common Jenkins project types used in real environments: **Freestyle Projects** and **Multibranch Pipelines** integrated with **Gitea source control**. This guide serves as a reusable runbook for future job use, lab refreshers, and helping others learn Jenkins fundamentals.


## 🏗️ Environment Build Choices

### 🖥️ Lab Platform

- KodeKloud Jenkins Playground
- Browser-hosted temporary training sandbox
- Included Jenkins UI, Linux terminal, and Gitea repository hosting
- Fast environment for CI/CD hands-on practice

### ⚙️ Jenkins Platform

- Jenkins LTS interface
- Supports Freestyle, Pipeline, and Multibranch jobs
- Console output and build history enabled
- Plugin-based CI/CD automation platform

### 🗃️ Source Control Platform

- Gitea lightweight self-hosted Git platform
- Used as SCM repository for Jenkins integration
- Git-based branch testing workflow

### 📦 Repository Used

```text
max/jenkins-lab
```

### 🌿 Branches Used

```text
main
dev
```


# 🧩 Gitea Repository Configuration Guide

## 🎯 Purpose

Used Gitea to host source-controlled Jenkins files and simulate real enterprise Git workflows for Jenkins integration.


## 🚀 Repository Creation Steps

### Navigation

- Open Gitea dashboard
- Select **New Repository**

### Settings Used

### 👤 Owner

```text
max
```

### 📁 Repository Name

```text
jenkins-lab
```

### 🌐 Visibility

- Public repository used for sandbox testing

### 📝 Description

Example:

```text
Jenkins CI/CD lab repo for scripts, Jenkinsfile, and automation testing.
```

### 📄 Initialize Repository

Enabled:

- README file created automatically

### 🌿 Default Branch

```text
main
```

### 🔐 Object Format

```text
sha1
```


## 🌿 Branch Workflow Used

### Main Branch

- Base production-style branch
- Stable Jenkinsfile stored here

### Dev Branch

Created later for testing:

```text
dev
```

Used for:

- Jenkinsfile changes
- Additional DEV ONLY stage
- Separate branch pipeline execution


## 💻 Git Actions Performed

### Created New Branch

```bash
git checkout -b dev
```

### Modified Jenkinsfile

Added custom stage:

```groovy
stage('DEV ONLY') {
  steps {
    sh 'echo Running DEV branch pipeline'
    sh 'date'
  }
}
```

### Commit Changes

```bash
git add .
git commit -m "Update Jenkinsfile"
git push origin dev
```


## ✅ Validation

- Repository created successfully
- Jenkins connected to Gitea URL
- Branches visible in repo
- Jenkinsfile stored in source control
- `main` and `dev` detected by Jenkins
- Dev branch changes triggered separate build


## 🧠 Lessons Learned

- Gitea is lightweight and fast for labs
- Great alternative to GitHub/GitLab
- Ideal for private internal repos
- Branching model mirrors enterprise workflows
- Excellent tool for Jenkins SCM practice


## 🔮 Future Improvements

- Enable private repo + credentials auth
- Add webhooks to trigger Jenkins instantly
- Add pull request workflow
- Add branch protection rules
- Add issue tracking integration


## 🧹 Final Hygiene Cleanup

- Delete unused test repos
- Remove stale branches
- Rotate credentials if used
- Keep Jenkinsfile versioned
- Clean old commits if repo becomes cluttered


# 1️⃣ Jenkins Freestyle Project Guide

## 🎯 Purpose

Used to run shell-based administrative jobs and system checks through Jenkins UI.


## 🚀 Setup Steps

### Navigation

- Jenkins Dashboard
- New Item
- Enter Name
- Select **Freestyle Project**

### Build Steps Used

Selected:

- Execute Shell

Commands entered:

```bash
hostname
date
whoami
df -h
free -m
systemctl --failed
docker ps
kubectl get nodes
```


## 💻 Commands Used

- `hostname`
- `date`
- `whoami`
- `df -h`
- `free -m`
- `systemctl --failed`
- `docker ps`
- `kubectl get nodes`

---

## ✅ Validation

- Job saved successfully
- Manual build successful
- Commands executed in console output
- Jenkins runtime user confirmed


## 🧠 Lessons Learned

- Great for admin scripts
- Easy troubleshooting entry point
- Common in legacy environments


## 🔮 Future Improvements

- Add scheduled triggers
- Add email alerts
- Add artifact archiving


## 🧹 Final Hygiene Cleanup

- Delete unused jobs
- Clean build history
- Review shell permissions


# 2️⃣ Jenkins Multibranch Pipeline Guide

## 🎯 Purpose

Automatically detect repository branches and run Jenkinsfile from each branch.

## 🚀 Setup Steps

### Navigation

- Jenkins Dashboard
- New Item
- Select **Multibranch Pipeline**

### Source Control

Repository URL:

```text
https://3000-port-xxxxx.labs.kodekloud.com/max/jenkins-lab.git
```

### Script Path

```text
Jenkinsfile
```

### Scan Trigger

- Periodic scan enabled


## 📄 Jenkinsfile Used

```groovy
pipeline {
  agent any

  stages {

    stage('Info') {
      steps {
        sh 'hostname'
        sh 'date'
        sh 'whoami'
      }
    }

    stage('Health') {
      steps {
        sh 'df -h'
        sh 'free -m'
      }
    }

  }
}
```


## 💻 Commands Used

- `hostname`
- `date`
- `whoami`
- `df -h`
- `free -m`


## ✅ Validation

- Jenkins indexed branches
- `main` detected
- `dev` detected
- Separate branch jobs created
- DEV branch executed custom stage


## 🧠 Lessons Learned

- Strong modern CI/CD design
- Reduces manual job creation
- Great for teams using Git branches


## 🔮 Future Improvements

- Replace polling with webhook
- Add Docker build stages
- Add Terraform deployment stage
- Add testing gates


## 🧹 Final Hygiene Cleanup

- Remove stale branches
- Reduce scan intervals
- Clean build history
- Backup Jenkins config


# 🏁 Final Validation

- Gitea repository configured
- Jenkins SCM integration working
- Freestyle project working
- Multibranch pipeline working
- Branch automation validated
- CI/CD knowledge expanded
```
