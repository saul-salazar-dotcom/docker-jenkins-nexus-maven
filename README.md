# docker-jenkins-nexus-maven

## Overview

### Nexus
- Setup & Access Nexus
- Create Repository
- Create User (credentials for usage on Jenkins)

### Jenkins
- Setup & Access Jenkins
- Install Plugins
- Create Project (Pipeline or Multibranch)

#### Pipeline Stages
- Build
- Test
- Publish to Nexus

---

## How To Details

### Nexus
- Create Repository named `maven-mixed`
    - with `Mixed Version Policy`
    - with reuploads allowed
- Create a rol/permission only for jenkins usage
- Create an User with those permissions

### Jenkins
- Install Tools
    - Maven 3.3.9 (or any version just update the Jenkinsfile)
        - Open Jenkins
        - Click "Manage Jenkins" > "Global Tool Configuration" > "Add Maven"
        - Select the version you want to use (must match with the one used on the Jenkinsfile)
    - Java
        - Open Jenkins
        - Click "Manage Jenkins" > "Global Tool Configuration" > "Add JDK" (near JDK installations)
        - Delete the java.sun.com installer.
        - Just click "Add Installer" below and choose "Extract .zip/.tar.gz". Enter following:
            - Label: `openjdk-11`
            - Download URL: `https://download.java.net/java/GA/jdk11/13/GPL/openjdk-11.0.1_linux-x64_bin.tar.gz`
            - Subdirectory of extracted archive: `jdk-11.0.1`
                - (Optional subdirectory of the downloaded and unpacked archive to use as the tool's home directory.)
- Install Plugins (already added by default)
    - Nexus Artifact Upload
    - Pipeline Utilities (makes pom.xml reading easy)
- Add Credentials for Git
- Add Credentials for Nexus
- Create Project (Pipeline or Multibranch)
    - Setup correct git source

### Jenkins trigger from git push
- On Jenkins (only if Jenkins project is not multibranch)
	1. Go to Project Settings
	1. Go to Build Triggers
	1. Mark "Build when a change is pushed to BitBucket"
- On BitBucket
	1. Go to Repository Settings
	1. Go to Webhooks (under Integrations)
	1. Add a new webhook
		- URL should be the jenkins instance BUT you have to append `/bitbucket-hook/`
		- Default for the rest fields
- To Validate
	1. Create a commit and push the changes to bitbucket
	1. Go to Jenkins and you should see a new build
	1. Click on the new build and it should say something like triggered by git webhook
