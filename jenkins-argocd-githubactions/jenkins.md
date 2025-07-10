Jenkins is a popular automation tool which is used for CI and CD let us consider typical SDLC process as a devops engineer your role is to automate build and release process with jenkins you can setup some job which can help you to autmate build and release
It automates entire cycle that's why it is called CI/CD tool


Continuous Integration
----------------------
- Fetching latest code[checkout the code]
- unit tests
- Source Code Analysis
- Dependency analysis
- Docker build
- Image Scanning
- Image push to ECR

Continuous Deployment
---------------------
- automate deploy the software in lower environemnts

Continuous Delivary
-------------------
- Manually Release the application to the enduser

- Generally CI/CD pipelines are decoupled in any organization


# Jenkins
- Jenkins Installation
- Jenkins Master-Slave Architecture
- UI overview
- FreeStyle Jobs
- Jobs
- Upstream & DownStream Jobs
- Buiding on the built-in node can be a security issue . You should set the number of executors on the built-in node to 0

upstreamjob
-----------
```bash
export PATH=/usr/local/bin:/usr/bin:$PATH

echo "Git Version:"
git --version
```

downstreamjob
-------------
```bash
export JAVA_HOME=/usr/lib/jvm/java-1.8.0-amazon-corretto.x86_64
export PATH=$JAVA_HOME/bin:$PATH
java -version
```

```jenkins
pipeline {
  agent any
  stages {
    stage('Build') {
      steps {
        echo "Building branch: ${env.BRANCH_NAME}"
      }
    }
  }
}
```
