# Jenkins Complete Notes: Beginner to Advanced

> **Purpose**: Interview-ready, structured Jenkins notes for DevOps professionals  
> **Target Audience**: Freshers moving to mid-level DevOps roles  
> **Format**: Concept → Explanation → Example

---

## Table of Contents
1. [Beginner Level](#beginner-level)
2. [Intermediate Level](#intermediate-level)
3. [Advanced Level](#advanced-level)
4. [Hands-On Tasks](#hands-on-tasks)
5. [Interview Questions](#interview-questions)
6. [Learning Checklist](#learning-checklist)
7. [Real-World Tips](#real-world-tips)

---

# Beginner Level

## 1. What is Jenkins

**Concept**: Jenkins is an open-source automation server written in Java.

**Explanation**: Jenkins automates the repetitive tasks in software development—building code, running tests, and deploying applications. Think of it as a robot that watches your code repository and automatically performs actions whenever you make changes. It's the backbone of CI/CD pipelines in most organizations.

**Example**: 
When a developer pushes code to GitHub at 10:00 AM, Jenkins automatically:
1. Pulls the latest code
2. Compiles the Java application using Maven
3. Runs 500 unit tests
4. Sends a Slack notification: "Build #245 passed ✓"

All this happens without any manual intervention.

---

## 2. Why Jenkins is Used

**Concept**: Jenkins eliminates manual, error-prone deployment processes.

**Explanation**: Before Jenkins, developers manually compiled code, ran tests, and deployed to servers—a process taking hours and prone to human error. Jenkins automates this entire workflow, ensuring consistency, speed, and reliability. It integrates with virtually every tool in the DevOps ecosystem.

**Example**:
**Without Jenkins (Manual Process)**:
```
Developer → Manually build → Manually test → Manually deploy → 4 hours, prone to errors
```

**With Jenkins (Automated Process)**:
```
Developer pushes code → Jenkins builds → Jenkins tests → Jenkins deploys → 15 minutes, zero manual steps
```

A company reduced deployment time from 4 hours to 15 minutes and eliminated "it works on my machine" issues.

---

## 3. Jenkins Architecture (Controller / Agent)

**Concept**: Jenkins uses a distributed architecture with a Controller (master) and Agents (workers).

**Explanation**: 
- **Controller (Master)**: The brain of Jenkins. It schedules jobs, manages the UI, stores configurations, and distributes work to agents. It should NOT run heavy builds itself.
- **Agent (Slave/Node)**: Worker machines that execute the actual build jobs. They can be Linux, Windows, or Mac machines with specific tools installed.

**Example**:
```
                    ┌─────────────────┐
                    │   CONTROLLER    │
                    │  (Scheduling,   │
                    │   UI, Config)   │
                    └────────┬────────┘
                             │
           ┌─────────────────┼─────────────────┐
           │                 │                 │
    ┌──────▼──────┐   ┌──────▼──────┐   ┌──────▼──────┐
    │   Agent 1   │   │   Agent 2   │   │   Agent 3   │
    │  (Linux,    │   │  (Windows,  │   │  (Mac,      │
    │   Maven)    │   │   .NET)     │   │   iOS)      │
    └─────────────┘   └─────────────┘   └─────────────┘
```

Real scenario: Controller receives a build request, checks which agent has Maven installed, assigns the job to Agent 1.

---

## 4. Jenkins Installation Overview

**Concept**: Jenkins can be installed on various platforms using different methods.

**Explanation**: Jenkins requires Java (JDK 11 or 17) and can be installed via:
- **Package managers**: apt, yum, brew
- **Docker**: Most popular for quick setup
- **WAR file**: Direct Java execution
- **Kubernetes**: Helm charts for production

**Example**:
```bash
# Docker Installation (Recommended for learning)
docker run -d \
  --name jenkins \
  -p 8080:8080 \
  -p 50000:50000 \
  -v jenkins_home:/var/jenkins_home \
  jenkins/jenkins:lts

# Access Jenkins
# URL: http://localhost:8080
# Get initial password:
docker exec jenkins cat /var/jenkins_home/secrets/initialAdminPassword
```

After installation, you'll see the setup wizard to install suggested plugins and create an admin user.

---

## 5. Jenkins Dashboard & UI

**Concept**: The Jenkins dashboard is the central control panel for managing all CI/CD activities.

**Explanation**: Key UI components:
- **Dashboard**: Shows all jobs with their status (blue = success, red = failed, grey = not built)
- **Build History**: Timeline of all builds with logs
- **Manage Jenkins**: System configuration, plugins, credentials
- **New Item**: Create new jobs/pipelines
- **Build Queue**: Jobs waiting to be executed

**Example**:
```
┌─────────────────────────────────────────────────────────┐
│  JENKINS DASHBOARD                                      │
├─────────────────────────────────────────────────────────┤
│  [New Item]  [People]  [Build History]  [Manage Jenkins]│
├─────────────────────────────────────────────────────────┤
│  Job Name              │ Last Success │ Last Failure    │
│  ─────────────────────────────────────────────────────  │
│  🔵 backend-api-build  │ 2 min ago    │ 3 days ago      │
│  🔴 frontend-deploy    │ 1 day ago    │ 5 min ago       │
│  ⚪ database-migration │ N/A          │ N/A             │
└─────────────────────────────────────────────────────────┘
```

🔵 = Stable (last build passed)  
🔴 = Failed (last build failed)  
⚪ = Not built yet

---

## 6. Freestyle Jobs

**Concept**: Freestyle jobs are GUI-based, simple Jenkins jobs configured through the web interface.

**Explanation**: Freestyle jobs are the simplest way to create automation in Jenkins. You configure everything through dropdown menus and text fields—no coding required. Good for beginners but limited for complex pipelines. They don't support version control of the job configuration itself.

**Example**:
Creating a Freestyle job to build a Java project:

1. **New Item** → Enter name: `my-java-app` → Select **Freestyle project**
2. **Source Code Management** → Git → URL: `https://github.com/company/app.git`
3. **Build Triggers** → Poll SCM: `H/5 * * * *` (check every 5 minutes)
4. **Build Steps** → Execute shell:
```bash
mvn clean package
```
5. **Post-build Actions** → Archive artifacts: `target/*.jar`

Result: Every 5 minutes, Jenkins checks for new commits and builds the JAR file.

---

## 7. Pipeline Jobs

**Concept**: Pipeline jobs define the entire CI/CD workflow as code in a Jenkinsfile.

**Explanation**: Unlike Freestyle jobs, Pipelines are written as code (Groovy-based DSL), stored in version control, and support complex workflows with parallel stages, conditionals, and error handling. This is the modern, recommended approach—"Pipeline as Code."

**Example**:
```groovy
// Jenkinsfile - stored in your Git repository
pipeline {
    agent any
    
    stages {
        stage('Build') {
            steps {
                sh 'mvn clean compile'
            }
        }
        stage('Test') {
            steps {
                sh 'mvn test'
            }
        }
        stage('Deploy') {
            steps {
                sh 'kubectl apply -f deployment.yaml'
            }
        }
    }
}
```

**Key Difference from Freestyle**:
| Freestyle | Pipeline |
|-----------|----------|
| GUI-based configuration | Code-based (Jenkinsfile) |
| Not version controlled | Stored in Git |
| Limited complexity | Supports complex workflows |
| Hard to replicate | Easy to replicate across projects |

---

## 8. What is CI in Jenkins

**Concept**: Continuous Integration (CI) is the practice of automatically building and testing code on every commit.

**Explanation**: CI ensures that code changes from multiple developers are integrated frequently (multiple times a day) and validated automatically. Jenkins acts as the CI server that:
1. Detects code changes
2. Pulls the latest code
3. Builds the application
4. Runs automated tests
5. Reports results immediately

**Example**:
```
Developer A pushes code at 9:00 AM
         ↓
Jenkins detects change via webhook
         ↓
Jenkins pulls code from Git
         ↓
Jenkins runs: mvn clean test
         ↓
Tests pass → Green build notification
Tests fail → Red alert + email to Developer A

Total time: 3 minutes
```

**Real-world impact**: A team of 10 developers pushing 50 commits/day catches bugs within minutes instead of discovering them during manual testing weeks later.

---

## 9. Jenkins Plugins

**Concept**: Plugins extend Jenkins functionality to integrate with external tools and services.

**Explanation**: Jenkins core is minimal by design. Its power comes from 1,800+ plugins that add features like Git integration, Docker support, Slack notifications, AWS deployments, and more. Plugins are installed via **Manage Jenkins → Manage Plugins**.

**Example**:
Essential plugins for a typical DevOps pipeline:

| Plugin | Purpose |
|--------|---------|
| Git | Clone repositories, track branches |
| Pipeline | Enable Jenkinsfile support |
| Docker Pipeline | Build and push Docker images |
| Slack Notification | Send build alerts to Slack |
| Credentials Binding | Securely inject secrets |
| Blue Ocean | Modern UI for pipelines |
| SonarQube Scanner | Code quality analysis |

**Installing a plugin**:
```
Manage Jenkins → Manage Plugins → Available → Search "Docker Pipeline" → Install
```

---

## 10. Introduction to Jenkinsfile

**Concept**: A Jenkinsfile is a text file containing the pipeline definition, stored in your source code repository.

**Explanation**: The Jenkinsfile defines your entire CI/CD pipeline as code. It lives alongside your application code in Git, meaning:
- Pipeline changes are version-controlled
- Code review for pipeline changes
- Same pipeline definition across all branches
- Easy to replicate for new projects

**Example**:
```groovy
// Jenkinsfile in repository root
pipeline {
    agent any
    
    environment {
        APP_NAME = 'my-service'
        DOCKER_REGISTRY = 'docker.io/mycompany'
    }
    
    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/company/app.git'
            }
        }
        
        stage('Build') {
            steps {
                sh 'npm install'
                sh 'npm run build'
            }
        }
        
        stage('Test') {
            steps {
                sh 'npm test'
            }
        }
    }
    
    post {
        success {
            echo 'Pipeline completed successfully!'
        }
        failure {
            echo 'Pipeline failed. Check logs.'
        }
    }
}
```

---

## 11. Basic Declarative Pipeline Structure

**Concept**: Declarative Pipeline is a simplified, structured syntax for defining Jenkins pipelines.

**Explanation**: There are two pipeline syntaxes:
- **Declarative** (Recommended): Structured, easier to read, has built-in error checking
- **Scripted**: More flexible but complex, uses full Groovy

Declarative Pipeline has a strict structure with predefined sections.

**Example**:
```groovy
pipeline {
    // WHERE to run - agent selection
    agent any
    
    // ENVIRONMENT variables
    environment {
        DEPLOY_ENV = 'staging'
    }
    
    // OPTIONAL: Build parameters
    parameters {
        string(name: 'BRANCH', defaultValue: 'main', description: 'Branch to build')
    }
    
    // MAIN WORKFLOW - stages and steps
    stages {
        stage('Stage 1: Build') {
            steps {
                echo 'Building...'
                sh 'make build'
            }
        }
        
        stage('Stage 2: Test') {
            steps {
                echo 'Testing...'
                sh 'make test'
            }
        }
        
        stage('Stage 3: Deploy') {
            when {
                branch 'main'  // Only deploy from main branch
            }
            steps {
                echo "Deploying to ${DEPLOY_ENV}..."
                sh 'make deploy'
            }
        }
    }
    
    // POST-BUILD actions
    post {
        always {
            cleanWs()  // Clean workspace
        }
        success {
            slackSend message: "Build #${BUILD_NUMBER} passed ✓"
        }
        failure {
            slackSend message: "Build #${BUILD_NUMBER} failed ✗"
        }
    }
}
```

**Structure Breakdown**:
```
pipeline {
    agent { }       → Where to run
    environment { } → Variables
    parameters { }  → User inputs
    stages {        → Main workflow
        stage { }
        stage { }
    }
    post { }        → Cleanup & notifications
}
```

---

# Intermediate Level

## 1. Jenkinsfile Deep Dive (stages, steps, environment, post)

**Concept**: Understanding the core building blocks of a Jenkinsfile for production pipelines.

**Explanation**:
- **stages**: Logical divisions of your pipeline (Build, Test, Deploy)
- **steps**: Individual commands within a stage
- **environment**: Variables available throughout the pipeline
- **post**: Actions that run after stages complete (cleanup, notifications)

**Example**:
```groovy
pipeline {
    agent any
    
    // Global environment variables
    environment {
        DOCKER_IMAGE = "myapp:${BUILD_NUMBER}"
        SONAR_TOKEN = credentials('sonar-token')  // Inject secret
    }
    
    stages {
        stage('Build') {
            // Stage-specific environment
            environment {
                MAVEN_OPTS = '-Xmx1024m'
            }
            steps {
                sh 'mvn clean package -DskipTests'
                // Archive the artifact
                archiveArtifacts artifacts: 'target/*.jar'
            }
        }
        
        stage('Unit Tests') {
            steps {
                sh 'mvn test'
                // Publish test results
                junit 'target/surefire-reports/*.xml'
            }
        }
        
        stage('Code Quality') {
            steps {
                withSonarQubeEnv('SonarQube') {
                    sh 'mvn sonar:sonar'
                }
            }
        }
        
        stage('Build Docker Image') {
            steps {
                sh "docker build -t ${DOCKER_IMAGE} ."
            }
        }
        
        stage('Deploy to Staging') {
            when {
                branch 'develop'
            }
            steps {
                sh 'kubectl apply -f k8s/staging/'
            }
        }
        
        stage('Deploy to Production') {
            when {
                branch 'main'
            }
            input {
                message "Deploy to production?"
                ok "Yes, deploy it!"
            }
            steps {
                sh 'kubectl apply -f k8s/production/'
            }
        }
    }
    
    post {
        always {
            // Always runs - cleanup
            sh 'docker system prune -f'
            cleanWs()
        }
        success {
            slackSend channel: '#deployments',
                      color: 'good',
                      message: "✅ ${JOB_NAME} #${BUILD_NUMBER} succeeded"
        }
        failure {
            slackSend channel: '#deployments',
                      color: 'danger',
                      message: "❌ ${JOB_NAME} #${BUILD_NUMBER} failed"
            // Send email on failure
            emailext subject: "Build Failed: ${JOB_NAME}",
                     body: "Check console output: ${BUILD_URL}",
                     to: 'team@company.com'
        }
        unstable {
            // When tests fail but build succeeds
            slackSend message: "⚠️ ${JOB_NAME} is unstable - tests failing"
        }
    }
}
```

---

## 2. Git Integration with Jenkins

**Concept**: Connecting Jenkins to Git repositories for automatic code checkout and build triggers.

**Explanation**: Jenkins integrates with Git through:
1. **Git Plugin**: Clones repositories, checks out branches
2. **Credentials**: SSH keys or tokens for private repos
3. **Webhooks**: Instant triggers on push events
4. **Branch tracking**: Build specific branches or all branches

**Example**:
```groovy
pipeline {
    agent any
    
    stages {
        stage('Checkout') {
            steps {
                // Method 1: Simple checkout
                git branch: 'main',
                    url: 'https://github.com/company/backend-api.git',
                    credentialsId: 'github-token'
                
                // Method 2: Advanced checkout with options
                checkout([
                    $class: 'GitSCM',
                    branches: [[name: '*/main']],
                    extensions: [
                        [$class: 'CloneOption', depth: 1, shallow: true],  // Shallow clone
                        [$class: 'CleanBeforeCheckout']  // Clean workspace
                    ],
                    userRemoteConfigs: [[
                        url: 'git@github.com:company/backend-api.git',
                        credentialsId: 'github-ssh-key'
                    ]]
                ])
            }
        }
        
        stage('Show Git Info') {
            steps {
                script {
                    // Access Git environment variables
                    echo "Branch: ${env.GIT_BRANCH}"
                    echo "Commit: ${env.GIT_COMMIT}"
                    echo "Author: ${env.GIT_AUTHOR_NAME}"
                    
                    // Get commit message
                    def commitMsg = sh(script: 'git log -1 --pretty=%B', returnStdout: true).trim()
                    echo "Commit Message: ${commitMsg}"
                }
            }
        }
    }
}
```

**Setting up Git credentials in Jenkins**:
```
Manage Jenkins → Manage Credentials → Global → Add Credentials

For HTTPS:
- Kind: Username with password
- Username: your-github-username
- Password: your-personal-access-token
- ID: github-token

For SSH:
- Kind: SSH Username with private key
- Username: git
- Private Key: Enter directly (paste your private key)
- ID: github-ssh-key
```

---

## 3. Webhooks and Pipeline Triggers

**Concept**: Webhooks enable instant pipeline triggers when code changes occur in Git.

**Explanation**: Instead of Jenkins polling Git every few minutes (wasteful), webhooks push notifications to Jenkins immediately when events occur (push, PR, tag). This provides:
- Instant builds (no polling delay)
- Reduced load on Git servers
- Event-specific triggers (push, PR opened, PR merged)

**Example**:

**Step 1: Configure Jenkins job for webhook**
```groovy
pipeline {
    agent any
    
    triggers {
        // GitHub webhook trigger
        githubPush()
        
        // Or for GitLab
        // gitlab(triggerOnPush: true, triggerOnMergeRequest: true)
        
        // Or for Bitbucket
        // bitbucketPush()
    }
    
    stages {
        stage('Build') {
            steps {
                echo "Triggered by webhook!"
                echo "Branch: ${env.GIT_BRANCH}"
            }
        }
    }
}
```

**Step 2: Configure GitHub webhook**
```
GitHub Repository → Settings → Webhooks → Add webhook

Payload URL: http://jenkins.company.com/github-webhook/
Content type: application/json
Secret: your-webhook-secret
Events: Just the push event (or select specific events)
```

**Step 3: For Jenkins behind firewall (use ngrok for testing)**
```bash
# Expose local Jenkins to internet
ngrok http 8080

# Use the ngrok URL in GitHub webhook
# https://abc123.ngrok.io/github-webhook/
```

**Other trigger types**:
```groovy
triggers {
    // Poll SCM (fallback if webhooks fail)
    pollSCM('H/5 * * * *')  // Every 5 minutes
    
    // Cron schedule (nightly builds)
    cron('H 2 * * *')  // Daily at 2 AM
    
    // Trigger from another job
    upstream(upstreamProjects: 'build-job', threshold: hudson.model.Result.SUCCESS)
}
```

---

## 4. Multibranch Pipelines

**Concept**: Multibranch Pipelines automatically create jobs for each branch in a repository.

**Explanation**: Instead of creating separate jobs for `main`, `develop`, `feature/*` branches, a Multibranch Pipeline:
1. Scans the repository for branches
2. Creates a job for each branch with a Jenkinsfile
3. Automatically removes jobs when branches are deleted
4. Perfect for GitFlow or trunk-based development

**Example**:

**Creating a Multibranch Pipeline**:
```
New Item → Enter name: "backend-api" → Select "Multibranch Pipeline"

Branch Sources:
- GitHub → Repository URL: https://github.com/company/backend-api
- Credentials: github-token
- Behaviors: 
  - Discover branches: All branches
  - Discover pull requests: Merging with current branch revision

Build Configuration:
- Mode: by Jenkinsfile
- Script Path: Jenkinsfile
```

**Jenkinsfile with branch-specific behavior**:
```groovy
pipeline {
    agent any
    
    stages {
        stage('Build') {
            steps {
                sh 'npm install'
                sh 'npm run build'
            }
        }
        
        stage('Test') {
            steps {
                sh 'npm test'
            }
        }
        
        stage('Deploy to Dev') {
            when {
                branch 'develop'
            }
            steps {
                echo 'Deploying to development environment...'
                sh 'kubectl apply -f k8s/dev/'
            }
        }
        
        stage('Deploy to Staging') {
            when {
                branch 'release/*'
            }
            steps {
                echo 'Deploying to staging environment...'
                sh 'kubectl apply -f k8s/staging/'
            }
        }
        
        stage('Deploy to Production') {
            when {
                branch 'main'
            }
            steps {
                input message: 'Deploy to production?'
                echo 'Deploying to production environment...'
                sh 'kubectl apply -f k8s/production/'
            }
        }
    }
}
```

**Result in Jenkins UI**:
```
backend-api (Multibranch Pipeline)
├── main           → Deploys to production
├── develop        → Deploys to dev
├── feature/login  → Build & test only
├── feature/payment → Build & test only
└── release/v2.0   → Deploys to staging
```

---

## 5. Build Tools Integration (Maven / npm / Python)

**Concept**: Jenkins integrates with build tools to compile, package, and test applications.

**Explanation**: Each programming language has its build tools:
- **Java**: Maven, Gradle
- **JavaScript/Node.js**: npm, yarn
- **Python**: pip, poetry

Jenkins can use these tools directly via shell commands or through dedicated plugins.

**Example**:

**Maven (Java)**:
```groovy
pipeline {
    agent any
    
    tools {
        maven 'Maven-3.8.6'  // Configured in Global Tool Configuration
        jdk 'JDK-17'
    }
    
    stages {
        stage('Build') {
            steps {
                sh 'mvn clean compile'
            }
        }
        stage('Test') {
            steps {
                sh 'mvn test'
            }
            post {
                always {
                    junit 'target/surefire-reports/*.xml'
                }
            }
        }
        stage('Package') {
            steps {
                sh 'mvn package -DskipTests'
                archiveArtifacts artifacts: 'target/*.jar'
            }
        }
    }
}
```

**npm (Node.js)**:
```groovy
pipeline {
    agent {
        docker { image 'node:18-alpine' }
    }
    
    stages {
        stage('Install') {
            steps {
                sh 'npm ci'  // Clean install from lock file
            }
        }
        stage('Lint') {
            steps {
                sh 'npm run lint'
            }
        }
        stage('Test') {
            steps {
                sh 'npm test -- --coverage'
            }
            post {
                always {
                    publishHTML([
                        reportDir: 'coverage/lcov-report',
                        reportFiles: 'index.html',
                        reportName: 'Coverage Report'
                    ])
                }
            }
        }
        stage('Build') {
            steps {
                sh 'npm run build'
                archiveArtifacts artifacts: 'dist/**/*'
            }
        }
    }
}
```

**Python**:
```groovy
pipeline {
    agent {
        docker { image 'python:3.11-slim' }
    }
    
    stages {
        stage('Setup') {
            steps {
                sh '''
                    python -m venv venv
                    . venv/bin/activate
                    pip install -r requirements.txt
                '''
            }
        }
        stage('Lint') {
            steps {
                sh '''
                    . venv/bin/activate
                    flake8 src/
                    black --check src/
                '''
            }
        }
        stage('Test') {
            steps {
                sh '''
                    . venv/bin/activate
                    pytest tests/ --junitxml=test-results.xml --cov=src/
                '''
            }
            post {
                always {
                    junit 'test-results.xml'
                }
            }
        }
    }
}
```

---

## 6. Automated Testing in Jenkins

**Concept**: Jenkins runs automated tests and reports results as part of the CI pipeline.

**Explanation**: Testing in Jenkins includes:
- **Unit Tests**: Fast, isolated tests (JUnit, Jest, pytest)
- **Integration Tests**: Test component interactions
- **E2E Tests**: Full user workflow tests (Selenium, Cypress)
- **Test Reports**: Visual reports in Jenkins UI

**Example**:
```groovy
pipeline {
    agent any
    
    stages {
        stage('Unit Tests') {
            steps {
                sh 'npm run test:unit'
            }
            post {
                always {
                    // Publish JUnit-format results
                    junit 'test-results/unit/*.xml'
                }
            }
        }
        
        stage('Integration Tests') {
            steps {
                // Start dependencies
                sh 'docker-compose up -d postgres redis'
                sh 'sleep 10'  // Wait for services
                
                sh 'npm run test:integration'
            }
            post {
                always {
                    junit 'test-results/integration/*.xml'
                    sh 'docker-compose down'
                }
            }
        }
        
        stage('E2E Tests') {
            agent {
                docker {
                    image 'cypress/included:12.0.0'
                    args '-v /tmp/.X11-unix:/tmp/.X11-unix'
                }
            }
            steps {
                sh 'npm run test:e2e'
            }
            post {
                always {
                    // Publish Cypress results
                    junit 'cypress/results/*.xml'
                    
                    // Archive screenshots on failure
                    archiveArtifacts artifacts: 'cypress/screenshots/**/*', allowEmptyArchive: true
                    
                    // Archive videos
                    archiveArtifacts artifacts: 'cypress/videos/**/*', allowEmptyArchive: true
                }
            }
        }
        
        stage('Code Coverage') {
            steps {
                sh 'npm run test:coverage'
            }
            post {
                always {
                    // Publish coverage report
                    publishHTML([
                        reportDir: 'coverage/lcov-report',
                        reportFiles: 'index.html',
                        reportName: 'Code Coverage'
                    ])
                    
                    // Fail if coverage below threshold
                    script {
                        def coverage = sh(script: "cat coverage/coverage-summary.json | jq '.total.lines.pct'", returnStdout: true).trim()
                        if (coverage.toFloat() < 80) {
                            error "Code coverage ${coverage}% is below 80% threshold"
                        }
                    }
                }
            }
        }
    }
}
```

---

## 7. Jenkins Credentials & Secrets

**Concept**: Jenkins Credentials store sensitive data (passwords, API keys, SSH keys) securely.

**Explanation**: Never hardcode secrets in Jenkinsfiles. Instead:
1. Store secrets in Jenkins Credentials Manager
2. Reference them by ID in pipelines
3. Jenkins injects them at runtime (masked in logs)

**Example**:

**Adding credentials in Jenkins**:
```
Manage Jenkins → Manage Credentials → Global → Add Credentials

Types:
1. Username with password (for Git, Docker registries)
2. Secret text (API keys, tokens)
3. Secret file (kubeconfig, certificates)
4. SSH Username with private key
5. Certificate
```

**Using credentials in Jenkinsfile**:
```groovy
pipeline {
    agent any
    
    environment {
        // Method 1: Inject as environment variable
        DOCKER_CREDS = credentials('docker-hub-credentials')  // Creates DOCKER_CREDS_USR and DOCKER_CREDS_PSW
        AWS_ACCESS_KEY = credentials('aws-access-key')  // Secret text
        KUBECONFIG = credentials('kubeconfig-file')  // Secret file
    }
    
    stages {
        stage('Docker Login') {
            steps {
                // Using username/password credentials
                sh 'echo $DOCKER_CREDS_PSW | docker login -u $DOCKER_CREDS_USR --password-stdin'
            }
        }
        
        stage('Deploy to K8s') {
            steps {
                // Method 2: withCredentials block (recommended for scope control)
                withCredentials([
                    usernamePassword(
                        credentialsId: 'docker-hub-credentials',
                        usernameVariable: 'DOCKER_USER',
                        passwordVariable: 'DOCKER_PASS'
                    ),
                    string(credentialsId: 'slack-webhook', variable: 'SLACK_URL'),
                    file(credentialsId: 'kubeconfig-prod', variable: 'KUBECONFIG')
                ]) {
                    sh '''
                        kubectl --kubeconfig=$KUBECONFIG apply -f deployment.yaml
                        curl -X POST $SLACK_URL -d '{"text":"Deployed!"}'
                    '''
                }
            }
        }
        
        stage('AWS Operations') {
            steps {
                withCredentials([
                    [$class: 'AmazonWebServicesCredentialsBinding',
                     credentialsId: 'aws-credentials',
                     accessKeyVariable: 'AWS_ACCESS_KEY_ID',
                     secretKeyVariable: 'AWS_SECRET_ACCESS_KEY']
                ]) {
                    sh 'aws s3 ls'
                }
            }
        }
    }
}
```

**Security note**: Credentials are masked in console output:
```
+ echo ****
+ docker login -u myuser --password-stdin
```

---

## 8. Docker Integration with Jenkins

**Concept**: Jenkins can build Docker images, run containers, and push to registries.

**Explanation**: Docker integration enables:
1. **Docker as build environment**: Run builds in containers (consistent environment)
2. **Build Docker images**: Create images as part of CI
3. **Push to registries**: Deploy images to Docker Hub, ECR, GCR
4. **Docker Compose**: Spin up test dependencies

**Example**:
```groovy
pipeline {
    agent any
    
    environment {
        DOCKER_REGISTRY = 'docker.io'
        DOCKER_IMAGE = 'mycompany/backend-api'
        DOCKER_TAG = "${BUILD_NUMBER}"
    }
    
    stages {
        stage('Build in Docker') {
            agent {
                docker {
                    image 'maven:3.8-openjdk-17'
                    args '-v $HOME/.m2:/root/.m2'  // Cache Maven dependencies
                }
            }
            steps {
                sh 'mvn clean package -DskipTests'
                stash includes: 'target/*.jar', name: 'app-jar'
            }
        }
        
        stage('Build Docker Image') {
            steps {
                unstash 'app-jar'
                
                script {
                    // Build image
                    docker.build("${DOCKER_IMAGE}:${DOCKER_TAG}")
                    
                    // Also tag as latest
                    docker.build("${DOCKER_IMAGE}:latest")
                }
            }
        }
        
        stage('Test Docker Image') {
            steps {
                script {
                    // Run container and test
                    docker.image("${DOCKER_IMAGE}:${DOCKER_TAG}").inside {
                        sh 'java -version'
                        sh 'curl -f http://localhost:8080/health || exit 1'
                    }
                }
            }
        }
        
        stage('Push to Registry') {
            steps {
                script {
                    docker.withRegistry("https://${DOCKER_REGISTRY}", 'docker-hub-credentials') {
                        docker.image("${DOCKER_IMAGE}:${DOCKER_TAG}").push()
                        docker.image("${DOCKER_IMAGE}:latest").push()
                    }
                }
            }
        }
        
        stage('Cleanup') {
            steps {
                sh "docker rmi ${DOCKER_IMAGE}:${DOCKER_TAG} || true"
                sh "docker rmi ${DOCKER_IMAGE}:latest || true"
            }
        }
    }
}
```

**Dockerfile example**:
```dockerfile
FROM openjdk:17-slim
WORKDIR /app
COPY target/*.jar app.jar
EXPOSE 8080
HEALTHCHECK CMD curl -f http://localhost:8080/health || exit 1
ENTRYPOINT ["java", "-jar", "app.jar"]
```

---

## 9. Difference Between CI and CD in Jenkins

**Concept**: CI focuses on integration and testing; CD extends to automated deployment.

**Explanation**:
- **CI (Continuous Integration)**: Automatically build and test on every commit
- **CD (Continuous Delivery)**: Automatically deploy to staging; manual approval for production
- **CD (Continuous Deployment)**: Automatically deploy to production (no manual approval)

**Example**:
```groovy
pipeline {
    agent any
    
    stages {
        // ============ CI STAGES ============
        stage('Build') {
            steps {
                sh 'mvn clean compile'
            }
        }
        
        stage('Unit Test') {
            steps {
                sh 'mvn test'
            }
        }
        
        stage('Integration Test') {
            steps {
                sh 'mvn verify'
            }
        }
        
        stage('Code Quality') {
            steps {
                sh 'mvn sonar:sonar'
            }
        }
        
        stage('Build Artifact') {
            steps {
                sh 'mvn package -DskipTests'
                sh 'docker build -t myapp:${BUILD_NUMBER} .'
            }
        }
        // ============ END CI ============
        
        // ============ CD STAGES ============
        stage('Deploy to Dev') {
            // Automatic - no approval needed
            steps {
                sh 'kubectl apply -f k8s/dev/'
            }
        }
        
        stage('Deploy to Staging') {
            // Automatic - no approval needed
            steps {
                sh 'kubectl apply -f k8s/staging/'
            }
        }
        
        stage('Performance Tests') {
            steps {
                sh 'k6 run load-test.js'
            }
        }
        
        // CONTINUOUS DELIVERY: Manual approval for production
        stage('Deploy to Production') {
            when {
                branch 'main'
            }
            steps {
                // Manual gate - this makes it "Delivery" not "Deployment"
                input message: 'Deploy to production?', ok: 'Deploy'
                
                sh 'kubectl apply -f k8s/production/'
            }
        }
        
        // CONTINUOUS DEPLOYMENT alternative: No approval
        // stage('Deploy to Production') {
        //     when {
        //         branch 'main'
        //     }
        //     steps {
        //         // No input step - fully automated
        //         sh 'kubectl apply -f k8s/production/'
        //     }
        // }
        // ============ END CD ============
    }
}
```

**Visual comparison**:
```
CI Only:
Code → Build → Test → Artifact ✓ (Manual deployment)

Continuous Delivery:
Code → Build → Test → Artifact → Dev → Staging → [Approval] → Production

Continuous Deployment:
Code → Build → Test → Artifact → Dev → Staging → Production (Fully automated)
```

---

# Advanced Level

## 1. Jenkins Controller–Agent Scaling

**Concept**: Scaling Jenkins by adding agents to handle increased build load.

**Explanation**: As your organization grows, a single Jenkins server becomes a bottleneck. Scaling strategies:
1. **Vertical scaling**: More CPU/RAM for controller (limited)
2. **Horizontal scaling**: Add more agents (recommended)
3. **Controller offloading**: Controller only schedules; agents do all builds

**Example**:

**Adding a permanent agent**:
```
Manage Jenkins → Manage Nodes → New Node

Name: build-agent-01
Type: Permanent Agent
Remote root directory: /home/jenkins
Labels: linux docker maven
Launch method: Launch agent via SSH
  Host: 192.168.1.100
  Credentials: jenkins-ssh-key
  Host Key Verification: Known hosts file
```

**Using labels in pipeline**:
```groovy
pipeline {
    agent none  // Don't use controller
    
    stages {
        stage('Build Java') {
            agent { label 'maven' }  // Run on agent with 'maven' label
            steps {
                sh 'mvn clean package'
            }
        }
        
        stage('Build Node') {
            agent { label 'nodejs' }  // Run on agent with 'nodejs' label
            steps {
                sh 'npm run build'
            }
        }
        
        stage('Build Docker') {
            agent { label 'docker' }  // Run on agent with Docker installed
            steps {
                sh 'docker build -t myapp .'
            }
        }
        
        stage('Deploy') {
            agent { label 'kubectl' }  // Run on agent with kubectl
            steps {
                sh 'kubectl apply -f deployment.yaml'
            }
        }
    }
}
```

**Scaling architecture**:
```
                        ┌─────────────────┐
                        │   CONTROLLER    │
                        │  (Scheduling    │
                        │   only, no      │
                        │   builds)       │
                        └────────┬────────┘
                                 │
        ┌────────────────────────┼────────────────────────┐
        │                        │                        │
┌───────▼───────┐       ┌───────▼───────┐       ┌───────▼───────┐
│  Agent Pool   │       │  Agent Pool   │       │  Agent Pool   │
│  (Maven/Java) │       │  (Node.js)    │       │  (Docker)     │
│  3 agents     │       │  2 agents     │       │  4 agents     │
└───────────────┘       └───────────────┘       └───────────────┘
```

---

## 2. Static vs Dynamic Agents

**Concept**: Static agents are always running; dynamic agents are created on-demand.

**Explanation**:
- **Static Agents**: Pre-provisioned VMs, always available, fixed cost
- **Dynamic Agents**: Created when needed, destroyed after use, pay-per-use

| Aspect | Static | Dynamic |
|--------|--------|---------|
| Availability | Always ready | Spin-up time (30s-2min) |
| Cost | Fixed (24/7) | Pay per build |
| Maintenance | Manual updates | Image-based updates |
| Scaling | Manual | Automatic |

**Example**:

**Static agent configuration** (always running):
```groovy
pipeline {
    agent { label 'static-linux-agent' }
    
    stages {
        stage('Build') {
            steps {
                sh 'mvn clean package'
            }
        }
    }
}
```

**Dynamic agent with Docker** (created per build):
```groovy
pipeline {
    agent {
        docker {
            image 'maven:3.8-openjdk-17'
            label 'docker-host'  // Run on a host with Docker
            args '-v /tmp:/tmp'
        }
    }
    
    stages {
        stage('Build') {
            steps {
                sh 'mvn clean package'
            }
        }
    }
}
// Container is destroyed after pipeline completes
```

**Dynamic agent with Kubernetes** (created per build):
```groovy
pipeline {
    agent {
        kubernetes {
            yaml '''
apiVersion: v1
kind: Pod
spec:
  containers:
  - name: maven
    image: maven:3.8-openjdk-17
    command: ['sleep', 'infinity']
  - name: docker
    image: docker:dind
    securityContext:
      privileged: true
'''
        }
    }
    
    stages {
        stage('Build') {
            container('maven') {
                steps {
                    sh 'mvn clean package'
                }
            }
        }
        stage('Docker Build') {
            container('docker') {
                steps {
                    sh 'docker build -t myapp .'
                }
            }
        }
    }
}
// Pod is deleted after pipeline completes
```

---

## 3. Jenkins Agents Using Docker

**Concept**: Using Docker containers as ephemeral Jenkins agents.

**Explanation**: Docker agents provide:
1. **Consistency**: Same environment every time
2. **Isolation**: No conflicts between builds
3. **Speed**: Pre-built images, fast startup
4. **Cleanup**: Automatic cleanup after build

**Example**:

**Method 1: Docker agent per stage**
```groovy
pipeline {
    agent none
    
    stages {
        stage('Build Frontend') {
            agent {
                docker {
                    image 'node:18-alpine'
                    args '-v $HOME/.npm:/root/.npm'  // Cache npm
                }
            }
            steps {
                sh 'npm ci && npm run build'
                stash includes: 'dist/**', name: 'frontend'
            }
        }
        
        stage('Build Backend') {
            agent {
                docker {
                    image 'golang:1.21-alpine'
                    args '-v $HOME/go:/go'  // Cache Go modules
                }
            }
            steps {
                sh 'go build -o app .'
                stash includes: 'app', name: 'backend'
            }
        }
        
        stage('Build Docker Image') {
            agent {
                docker {
                    image 'docker:24-cli'
                    args '-v /var/run/docker.sock:/var/run/docker.sock'
                }
            }
            steps {
                unstash 'frontend'
                unstash 'backend'
                sh 'docker build -t myapp:${BUILD_NUMBER} .'
            }
        }
    }
}
```

**Method 2: Custom Dockerfile as agent**
```groovy
pipeline {
    agent {
        dockerfile {
            filename 'Dockerfile.build'
            dir 'ci'
            args '-v /tmp:/tmp'
            additionalBuildArgs '--build-arg VERSION=1.0'
        }
    }
    
    stages {
        stage('Build') {
            steps {
                sh 'make build'
            }
        }
    }
}
```

**ci/Dockerfile.build**:
```dockerfile
FROM ubuntu:22.04
RUN apt-get update && apt-get install -y \
    openjdk-17-jdk \
    maven \
    nodejs \
    npm \
    docker.io
ARG VERSION
ENV APP_VERSION=$VERSION
```

---

## 4. Jenkins with Kubernetes (High-Level Flow)

**Concept**: Running Jenkins on Kubernetes with dynamic pod-based agents.

**Explanation**: Jenkins on Kubernetes provides:
1. **Controller on K8s**: Jenkins controller runs as a Deployment
2. **Dynamic agents**: Each build spins up a pod, destroyed after
3. **Auto-scaling**: Kubernetes handles resource allocation
4. **High availability**: Multiple controller replicas possible

**Example**:

**High-level architecture**:
```
┌─────────────────────────────────────────────────────────────┐
│                    KUBERNETES CLUSTER                        │
│                                                              │
│  ┌──────────────────────────────────────────────────────┐   │
│  │              Jenkins Namespace                        │   │
│  │                                                       │   │
│  │  ┌─────────────────┐    ┌─────────────────────────┐  │   │
│  │  │    Jenkins      │    │   Dynamic Agent Pods    │  │   │
│  │  │   Controller    │───▶│  ┌─────┐ ┌─────┐       │  │   │
│  │  │   (Deployment)  │    │  │Pod 1│ │Pod 2│ ...   │  │   │
│  │  └─────────────────┘    │  └─────┘ └─────┘       │  │   │
│  │          │              └─────────────────────────┘  │   │
│  │          ▼                                           │   │
│  │  ┌─────────────────┐                                 │   │
│  │  │   PVC (Jenkins  │                                 │   │
│  │  │   Home Data)    │                                 │   │
│  │  └─────────────────┘                                 │   │
│  └──────────────────────────────────────────────────────┘   │
│                                                              │
└─────────────────────────────────────────────────────────────┘
```

**Jenkinsfile for Kubernetes agents**:
```groovy
pipeline {
    agent {
        kubernetes {
            yaml '''
apiVersion: v1
kind: Pod
metadata:
  labels:
    app: jenkins-agent
spec:
  serviceAccountName: jenkins-agent
  containers:
  - name: jnlp
    image: jenkins/inbound-agent:latest
    resources:
      requests:
        memory: "256Mi"
        cpu: "100m"
  - name: maven
    image: maven:3.8-openjdk-17
    command: ['sleep', 'infinity']
    resources:
      requests:
        memory: "1Gi"
        cpu: "500m"
    volumeMounts:
    - name: maven-cache
      mountPath: /root/.m2
  - name: docker
    image: docker:24-dind
    securityContext:
      privileged: true
    resources:
      requests:
        memory: "512Mi"
        cpu: "250m"
  - name: kubectl
    image: bitnami/kubectl:latest
    command: ['sleep', 'infinity']
  volumes:
  - name: maven-cache
    persistentVolumeClaim:
      claimName: maven-cache-pvc
'''
        }
    }
    
    stages {
        stage('Build') {
            steps {
                container('maven') {
                    sh 'mvn clean package'
                }
            }
        }
        
        stage('Build Image') {
            steps {
                container('docker') {
                    sh 'docker build -t myapp:${BUILD_NUMBER} .'
                    sh 'docker push myregistry/myapp:${BUILD_NUMBER}'
                }
            }
        }
        
        stage('Deploy') {
            steps {
                container('kubectl') {
                    sh 'kubectl set image deployment/myapp myapp=myregistry/myapp:${BUILD_NUMBER}'
                }
            }
        }
    }
}
```

---

## 5. Jenkins Shared Libraries

**Concept**: Reusable pipeline code shared across multiple Jenkinsfiles.

**Explanation**: Shared Libraries allow you to:
1. **DRY principle**: Write common functions once
2. **Standardization**: Enforce pipeline standards across teams
3. **Versioning**: Library is a Git repo with versions
4. **Abstraction**: Hide complexity from developers

**Example**:

**Shared Library structure** (Git repo: `jenkins-shared-library`):
```
jenkins-shared-library/
├── vars/
│   ├── buildMaven.groovy      # Global variables/functions
│   ├── deployToK8s.groovy
│   ├── notifySlack.groovy
│   └── standardPipeline.groovy
├── src/
│   └── com/
│       └── company/
│           └── Utils.groovy   # Classes
└── resources/
    └── templates/
        └── deployment.yaml    # Resource files
```

**vars/buildMaven.groovy**:
```groovy
def call(Map config = [:]) {
    def mavenVersion = config.mavenVersion ?: '3.8'
    def jdkVersion = config.jdkVersion ?: '17'
    def skipTests = config.skipTests ?: false
    
    pipeline {
        agent {
            docker {
                image "maven:${mavenVersion}-openjdk-${jdkVersion}"
            }
        }
        
        stages {
            stage('Build') {
                steps {
                    sh "mvn clean compile"
                }
            }
            stage('Test') {
                when { expression { !skipTests } }
                steps {
                    sh "mvn test"
                }
            }
            stage('Package') {
                steps {
                    sh "mvn package -DskipTests"
                }
            }
        }
    }
}
```

**vars/notifySlack.groovy**:
```groovy
def call(String status, String channel = '#builds') {
    def color = status == 'SUCCESS' ? 'good' : 'danger'
    def emoji = status == 'SUCCESS' ? '✅' : '❌'
    
    slackSend(
        channel: channel,
        color: color,
        message: "${emoji} *${env.JOB_NAME}* #${env.BUILD_NUMBER} - ${status}\n${env.BUILD_URL}"
    )
}
```

**vars/standardPipeline.groovy**:
```groovy
def call(Map config) {
    pipeline {
        agent any
        
        stages {
            stage('Checkout') {
                steps {
                    checkout scm
                }
            }
            stage('Build') {
                steps {
                    script {
                        if (config.buildTool == 'maven') {
                            sh 'mvn clean package'
                        } else if (config.buildTool == 'npm') {
                            sh 'npm ci && npm run build'
                        }
                    }
                }
            }
            stage('Test') {
                steps {
                    script {
                        if (config.buildTool == 'maven') {
                            sh 'mvn test'
                        } else if (config.buildTool == 'npm') {
                            sh 'npm test'
                        }
                    }
                }
            }
            stage('Deploy') {
                when { branch 'main' }
                steps {
                    deployToK8s(
                        namespace: config.namespace,
                        deployment: config.deployment
                    )
                }
            }
        }
        
        post {
            success { notifySlack('SUCCESS') }
            failure { notifySlack('FAILURE') }
        }
    }
}
```

**Using the shared library in Jenkinsfile**:
```groovy
@Library('jenkins-shared-library@main') _

// Method 1: Use pre-built pipeline
standardPipeline(
    buildTool: 'maven',
    namespace: 'production',
    deployment: 'backend-api'
)

// Method 2: Use individual functions
// @Library('jenkins-shared-library@main') _
// pipeline {
//     agent any
//     stages {
//         stage('Build') {
//             steps {
//                 buildMaven(mavenVersion: '3.9', skipTests: true)
//             }
//         }
//     }
//     post {
//         always { notifySlack(currentBuild.result) }
//     }
// }
```

**Configure shared library in Jenkins**:
```
Manage Jenkins → Configure System → Global Pipeline Libraries

Name: jenkins-shared-library
Default version: main
Retrieval method: Modern SCM
  - Git
  - Project Repository: https://github.com/company/jenkins-shared-library.git
  - Credentials: github-token
```

---

## 6. Jenkins Security Best Practices

**Concept**: Securing Jenkins to prevent unauthorized access and data breaches.

**Explanation**: Jenkins security involves:
1. **Authentication**: Who can access Jenkins
2. **Authorization**: What they can do
3. **Secrets management**: Protecting credentials
4. **Network security**: Limiting exposure

**Example**:

**1. Enable security and configure authentication**:
```
Manage Jenkins → Configure Global Security

Security Realm: Jenkins' own user database
  ☑ Allow users to sign up (disable in production)

OR

Security Realm: LDAP
  Server: ldap://ldap.company.com
  Root DN: dc=company,dc=com
```

**2. Configure authorization (Role-Based Access)**:
```
Install "Role-based Authorization Strategy" plugin

Manage Jenkins → Configure Global Security
Authorization: Role-Based Strategy

Manage Jenkins → Manage and Assign Roles → Manage Roles

Global Roles:
| Role    | Overall/Read | Job/Build | Job/Configure | Agent/Build |
|---------|--------------|-----------|---------------|-------------|
| admin   | ✓            | ✓         | ✓             | ✓           |
| dev     | ✓            | ✓         | ✗             | ✓           |
| viewer  | ✓            | ✗         | ✗             | ✗           |

Project Roles:
| Role         | Pattern          | Job/Build | Job/Configure |
|--------------|------------------|-----------|---------------|
| frontend-dev | frontend-.*      | ✓         | ✓             |
| backend-dev  | backend-.*       | ✓         | ✓             |
```

**3. Secure Jenkinsfile practices**:
```groovy
pipeline {
    agent any
    
    stages {
        stage('Secure Build') {
            steps {
                // ✅ GOOD: Use credentials binding
                withCredentials([
                    string(credentialsId: 'api-key', variable: 'API_KEY')
                ]) {
                    sh 'curl -H "Authorization: $API_KEY" https://api.example.com'
                }
                
                // ❌ BAD: Hardcoded secrets
                // sh 'curl -H "Authorization: sk-12345" https://api.example.com'
                
                // ✅ GOOD: Use approved scripts
                script {
                    def result = sh(script: 'echo "safe"', returnStdout: true)
                }
                
                // ❌ BAD: Arbitrary Groovy execution
                // evaluate("dangerous code")
            }
        }
    }
}
```

**4. Network security configuration**:
```groovy
// Jenkins Configuration as Code (JCasC)
// jenkins.yaml
jenkins:
  securityRealm:
    ldap:
      configurations:
        - server: ldap://ldap.company.com
          rootDN: dc=company,dc=com
  
  authorizationStrategy:
    roleBased:
      roles:
        global:
          - name: admin
            permissions:
              - Overall/Administer
            assignments:
              - admin-group
  
  remotingSecurity:
    enabled: true
  
  crumbIssuer:
    standard:
      excludeClientIPFromCrumb: false

security:
  scriptApproval:
    approvedSignatures: []
  
  globalJobDslSecurityConfiguration:
    useScriptSecurity: true
```

**5. Audit logging**:
```
Install "Audit Trail" plugin

Manage Jenkins → Configure System → Audit Trail
Log Location: /var/log/jenkins/audit.log
Log Pattern: .* (log everything)
```

---

## 7. Jenkins Performance Optimization

**Concept**: Tuning Jenkins for faster builds and better resource utilization.

**Explanation**: Performance optimization areas:
1. **Controller optimization**: JVM tuning, job cleanup
2. **Build optimization**: Parallel stages, caching
3. **Agent optimization**: Right-sizing, dynamic scaling
4. **Storage optimization**: Artifact cleanup, log rotation

**Example**:

**1. JVM tuning for Jenkins controller**:
```bash
# /etc/default/jenkins or JAVA_OPTS
JAVA_OPTS="-Xms4g -Xmx8g \
  -XX:+UseG1GC \
  -XX:+ParallelRefProcEnabled \
  -XX:+UseStringDeduplication \
  -XX:MaxGCPauseMillis=200 \
  -Djava.awt.headless=true \
  -Djenkins.install.runSetupWizard=false"
```

**2. Parallel stages in pipeline**:
```groovy
pipeline {
    agent none
    
    stages {
        stage('Build') {
            agent { label 'builder' }
            steps {
                sh 'mvn clean package -DskipTests'
                stash includes: 'target/*.jar', name: 'app'
            }
        }
        
        stage('Parallel Tests') {
            parallel {
                stage('Unit Tests') {
                    agent { label 'test' }
                    steps {
                        unstash 'app'
                        sh 'mvn test -Dtest=*UnitTest'
                    }
                }
                stage('Integration Tests') {
                    agent { label 'test' }
                    steps {
                        unstash 'app'
                        sh 'mvn test -Dtest=*IntegrationTest'
                    }
                }
                stage('Security Scan') {
                    agent { label 'security' }
                    steps {
                        sh 'trivy image myapp:latest'
                    }
                }
                stage('Code Quality') {
                    agent { label 'sonar' }
                    steps {
                        sh 'mvn sonar:sonar'
                    }
                }
            }
        }
    }
}
```

**3. Caching dependencies**:
```groovy
pipeline {
    agent {
        docker {
            image 'maven:3.8-openjdk-17'
            // Mount host cache directory
            args '-v /var/jenkins_cache/.m2:/root/.m2'
        }
    }
    
    stages {
        stage('Build') {
            steps {
                // Uses cached dependencies
                sh 'mvn clean package'
            }
        }
    }
}
```

**4. Cleanup old builds**:
```groovy
pipeline {
    agent any
    
    options {
        // Keep only last 10 builds
        buildDiscarder(logRotator(numToKeepStr: '10', artifactNumToKeepStr: '5'))
        // Timeout after 30 minutes
        timeout(time: 30, unit: 'MINUTES')
        // Don't run concurrent builds
        disableConcurrentBuilds()
    }
    
    stages {
        stage('Build') {
            steps {
                sh 'mvn clean package'
            }
        }
    }
}
```

**5. Workspace cleanup**:
```groovy
pipeline {
    agent any
    
    stages {
        stage('Build') {
            steps {
                sh 'mvn clean package'
            }
        }
    }
    
    post {
        always {
            // Clean workspace after build
            cleanWs(
                cleanWhenNotBuilt: false,
                deleteDirs: true,
                disableDeferredWipeout: true,
                notFailBuild: true
            )
        }
    }
}
```

---

## 8. Jenkins in AWS (EC2, ECR, EKS Overview)

**Concept**: Deploying and integrating Jenkins with AWS services.

**Explanation**: Common AWS + Jenkins patterns:
1. **Jenkins on EC2**: Controller and agents on EC2 instances
2. **Jenkins with ECR**: Push Docker images to AWS container registry
3. **Jenkins with EKS**: Deploy to Kubernetes on AWS
4. **Jenkins with CodePipeline**: Hybrid CI/CD

**Example**:

**1. Jenkins on EC2 with dynamic agents**:
```groovy
// Install "Amazon EC2" plugin
// Configure EC2 cloud in Manage Jenkins → Manage Nodes → Configure Clouds

pipeline {
    agent {
        label 'ec2-linux'  // Matches EC2 plugin label
    }
    
    stages {
        stage('Build') {
            steps {
                sh 'mvn clean package'
            }
        }
    }
}
// EC2 instance is launched on-demand and terminated after build
```

**2. Push to ECR (Elastic Container Registry)**:
```groovy
pipeline {
    agent any
    
    environment {
        AWS_REGION = 'us-east-1'
        ECR_REGISTRY = '123456789012.dkr.ecr.us-east-1.amazonaws.com'
        ECR_REPO = 'myapp'
        IMAGE_TAG = "${BUILD_NUMBER}"
    }
    
    stages {
        stage('Build Image') {
            steps {
                sh "docker build -t ${ECR_REPO}:${IMAGE_TAG} ."
            }
        }
        
        stage('Push to ECR') {
            steps {
                withCredentials([[
                    $class: 'AmazonWebServicesCredentialsBinding',
                    credentialsId: 'aws-credentials',
                    accessKeyVariable: 'AWS_ACCESS_KEY_ID',
                    secretKeyVariable: 'AWS_SECRET_ACCESS_KEY'
                ]]) {
                    sh '''
                        # Login to ECR
                        aws ecr get-login-password --region $AWS_REGION | \
                            docker login --username AWS --password-stdin $ECR_REGISTRY
                        
                        # Tag and push
                        docker tag ${ECR_REPO}:${IMAGE_TAG} ${ECR_REGISTRY}/${ECR_REPO}:${IMAGE_TAG}
                        docker push ${ECR_REGISTRY}/${ECR_REPO}:${IMAGE_TAG}
                        
                        # Also push as latest
                        docker tag ${ECR_REPO}:${IMAGE_TAG} ${ECR_REGISTRY}/${ECR_REPO}:latest
                        docker push ${ECR_REGISTRY}/${ECR_REPO}:latest
                    '''
                }
            }
        }
    }
}
```

**3. Deploy to EKS (Elastic Kubernetes Service)**:
```groovy
pipeline {
    agent any
    
    environment {
        AWS_REGION = 'us-east-1'
        EKS_CLUSTER = 'production-cluster'
        ECR_REGISTRY = '123456789012.dkr.ecr.us-east-1.amazonaws.com'
    }
    
    stages {
        stage('Build & Push') {
            steps {
                // ... build and push to ECR
            }
        }
        
        stage('Deploy to EKS') {
            steps {
                withCredentials([[
                    $class: 'AmazonWebServicesCredentialsBinding',
                    credentialsId: 'aws-credentials'
                ]]) {
                    sh '''
                        # Update kubeconfig for EKS
                        aws eks update-kubeconfig --region $AWS_REGION --name $EKS_CLUSTER
                        
                        # Deploy using kubectl
                        kubectl set image deployment/myapp \
                            myapp=${ECR_REGISTRY}/myapp:${BUILD_NUMBER} \
                            --namespace production
                        
                        # Wait for rollout
                        kubectl rollout status deployment/myapp --namespace production --timeout=300s
                    '''
                }
            }
        }
    }
}
```

**AWS Architecture diagram**:
```
┌─────────────────────────────────────────────────────────────────┐
│                         AWS CLOUD                                │
│                                                                  │
│  ┌─────────────┐    ┌─────────────┐    ┌─────────────────────┐  │
│  │   Jenkins   │    │     ECR     │    │        EKS          │  │
│  │  (EC2/EKS)  │───▶│  (Docker    │───▶│  (Kubernetes        │  │
│  │             │    │   Images)   │    │   Cluster)          │  │
│  └─────────────┘    └─────────────┘    └─────────────────────┘  │
│         │                                        │               │
│         ▼                                        ▼               │
│  ┌─────────────┐                        ┌─────────────────────┐  │
│  │  S3 Bucket  │                        │   Application       │  │
│  │  (Artifacts)│                        │   Load Balancer     │  │
│  └─────────────┘                        └─────────────────────┘  │
│                                                  │               │
└──────────────────────────────────────────────────│───────────────┘
                                                   ▼
                                              End Users
```

---

# Hands-On Tasks

## Beginner Tasks

### Task 1: Create Your First Freestyle Job

**Objective**: Create a Freestyle job that clones a Git repository and runs a shell script.

**Jenkins Concepts Involved**:
- Freestyle job creation
- Git integration
- Build steps (Execute shell)
- Build triggers

**Tools Required**:
- Jenkins (local Docker or cloud instance)
- GitHub account
- Sample repository (create one with a simple script)

**Steps**:
1. Create a GitHub repo with a `build.sh` file:
```bash
#!/bin/bash
echo "Build started at $(date)"
echo "Hello from Jenkins!"
echo "Build completed successfully"
```

2. In Jenkins:
   - New Item → "my-first-job" → Freestyle project
   - Source Code Management → Git → Your repo URL
   - Build Steps → Execute shell → `chmod +x build.sh && ./build.sh`
   - Save and Build Now

**Expected Output**:
```
Started by user admin
Building in workspace /var/jenkins_home/workspace/my-first-job
Cloning repository https://github.com/youruser/sample-repo.git
Build started at Mon Jan 15 10:30:00 UTC 2024
Hello from Jenkins!
Build completed successfully
Finished: SUCCESS
```

---

### Task 2: Create a Declarative Pipeline

**Objective**: Write a Jenkinsfile with multiple stages and run it as a Pipeline job.

**Jenkins Concepts Involved**:
- Declarative Pipeline syntax
- Stages and steps
- Post actions
- Environment variables

**Tools Required**:
- Jenkins with Pipeline plugin
- Text editor

**Steps**:
1. Create a new Pipeline job in Jenkins
2. Select "Pipeline script" and enter:

```groovy
pipeline {
    agent any
    
    environment {
        APP_NAME = 'my-application'
        VERSION = '1.0.0'
    }
    
    stages {
        stage('Initialize') {
            steps {
                echo "Building ${APP_NAME} version ${VERSION}"
                echo "Build Number: ${BUILD_NUMBER}"
                echo "Workspace: ${WORKSPACE}"
            }
        }
        
        stage('Build') {
            steps {
                echo 'Compiling source code...'
                sh 'sleep 2'
                echo 'Compilation complete!'
            }
        }
        
        stage('Test') {
            steps {
                echo 'Running unit tests...'
                sh 'sleep 3'
                echo 'All tests passed!'
            }
        }
        
        stage('Deploy') {
            steps {
                echo 'Deploying application...'
                sh 'sleep 1'
                echo 'Deployment successful!'
            }
        }
    }
    
    post {
        success {
            echo '✅ Pipeline completed successfully!'
        }
        failure {
            echo '❌ Pipeline failed!'
        }
        always {
            echo 'Cleaning up...'
        }
    }
}
```

**Expected Output**:
```
[Pipeline] stage (Initialize)
Building my-application version 1.0.0
Build Number: 1
Workspace: /var/jenkins_home/workspace/my-pipeline

[Pipeline] stage (Build)
Compiling source code...
Compilation complete!

[Pipeline] stage (Test)
Running unit tests...
All tests passed!

[Pipeline] stage (Deploy)
Deploying application...
Deployment successful!

[Pipeline] stage (Declarative: Post Actions)
✅ Pipeline completed successfully!
Cleaning up...

Finished: SUCCESS
```

---

## Intermediate Tasks

### Task 3: Build and Test a Node.js Application

**Objective**: Create a pipeline that builds, tests, and reports results for a Node.js application.

**Jenkins Concepts Involved**:
- Docker agents
- npm build tools
- Test reporting (JUnit)
- Artifact archiving

**Tools Required**:
- Jenkins with Docker Pipeline plugin
- Sample Node.js project with tests

**Steps**:

1. Create a sample Node.js project:

**package.json**:
```json
{
  "name": "sample-app",
  "version": "1.0.0",
  "scripts": {
    "test": "jest --ci --reporters=default --reporters=jest-junit",
    "build": "echo 'Building...' && mkdir -p dist && echo 'Built!' > dist/app.js"
  },
  "devDependencies": {
    "jest": "^29.0.0",
    "jest-junit": "^15.0.0"
  }
}
```

**sum.js**:
```javascript
function sum(a, b) {
  return a + b;
}
module.exports = sum;
```

**sum.test.js**:
```javascript
const sum = require('./sum');
test('adds 1 + 2 to equal 3', () => {
  expect(sum(1, 2)).toBe(3);
});
```

2. Create Jenkinsfile:

```groovy
pipeline {
    agent {
        docker {
            image 'node:18-alpine'
        }
    }
    
    stages {
        stage('Install Dependencies') {
            steps {
                sh 'npm install'
            }
        }
        
        stage('Run Tests') {
            steps {
                sh 'npm test'
            }
            post {
                always {
                    junit 'junit.xml'
                }
            }
        }
        
        stage('Build') {
            steps {
                sh 'npm run build'
            }
        }
        
        stage('Archive Artifacts') {
            steps {
                archiveArtifacts artifacts: 'dist/**/*', fingerprint: true
            }
        }
    }
    
    post {
        success {
            echo 'Build and tests passed!'
        }
        failure {
            echo 'Build or tests failed!'
        }
    }
}
```

**Expected Output**:
- Build succeeds with all stages green
- Test results visible in Jenkins UI (1 test passed)
- Artifacts archived (dist/app.js)

---

### Task 4: Implement a Multibranch Pipeline with Webhooks

**Objective**: Set up a Multibranch Pipeline that automatically builds different branches.

**Jenkins Concepts Involved**:
- Multibranch Pipeline
- Branch-specific behavior
- GitHub webhooks
- Conditional stages

**Tools Required**:
- Jenkins with GitHub plugin
- GitHub repository with multiple branches
- ngrok (for local Jenkins webhook testing)

**Steps**:

1. Create a repository with branches: `main`, `develop`, `feature/new-feature`

2. Add Jenkinsfile to repository root:

```groovy
pipeline {
    agent any
    
    stages {
        stage('Build') {
            steps {
                echo "Building branch: ${env.BRANCH_NAME}"
                sh 'echo "Build artifacts" > build.txt'
            }
        }
        
        stage('Test') {
            steps {
                echo 'Running tests...'
            }
        }
        
        stage('Deploy to Dev') {
            when {
                branch 'develop'
            }
            steps {
                echo '🚀 Deploying to DEVELOPMENT environment'
            }
        }
        
        stage('Deploy to Staging') {
            when {
                branch pattern: "release/.*", comparator: "REGEXP"
            }
            steps {
                echo '🚀 Deploying to STAGING environment'
            }
        }
        
        stage('Deploy to Production') {
            when {
                branch 'main'
            }
            steps {
                input message: 'Deploy to production?', ok: 'Deploy'
                echo '🚀 Deploying to PRODUCTION environment'
            }
        }
    }
    
    post {
        success {
            echo "✅ ${env.BRANCH_NAME} build #${env.BUILD_NUMBER} succeeded"
        }
    }
}
```

3. Create Multibranch Pipeline in Jenkins:
   - New Item → "my-multibranch" → Multibranch Pipeline
   - Branch Sources → GitHub → Add repository
   - Build Configuration → Jenkinsfile

4. Set up webhook in GitHub:
   - Settings → Webhooks → Add webhook
   - Payload URL: `http://your-jenkins/github-webhook/`

**Expected Output**:
- Jenkins creates jobs for each branch automatically
- `main` branch shows "Deploy to Production" stage
- `develop` branch shows "Deploy to Dev" stage
- Feature branches only show Build and Test stages

---

## Advanced Tasks

### Task 5: Create a Jenkins Shared Library

**Objective**: Build a shared library with reusable pipeline functions.

**Jenkins Concepts Involved**:
- Shared Libraries
- Global variables (vars/)
- Library versioning
- DRY principles

**Tools Required**:
- Jenkins with Pipeline plugin
- Git repository for shared library
- Multiple projects to use the library

**Steps**:

1. Create shared library repository structure:

```
jenkins-shared-library/
├── vars/
│   ├── standardBuild.groovy
│   ├── notifyTeam.groovy
│   └── dockerBuild.groovy
└── README.md
```

2. **vars/standardBuild.groovy**:
```groovy
def call(Map config = [:]) {
    def buildTool = config.buildTool ?: 'maven'
    def skipTests = config.skipTests ?: false
    
    pipeline {
        agent any
        
        stages {
            stage('Checkout') {
                steps {
                    checkout scm
                }
            }
            
            stage('Build') {
                steps {
                    script {
                        switch(buildTool) {
                            case 'maven':
                                sh 'mvn clean compile'
                                break
                            case 'npm':
                                sh 'npm ci'
                                sh 'npm run build'
                                break
                            case 'gradle':
                                sh './gradlew build'
                                break
                        }
                    }
                }
            }
            
            stage('Test') {
                when { expression { !skipTests } }
                steps {
                    script {
                        switch(buildTool) {
                            case 'maven':
                                sh 'mvn test'
                                break
                            case 'npm':
                                sh 'npm test'
                                break
                            case 'gradle':
                                sh './gradlew test'
                                break
                        }
                    }
                }
            }
        }
        
        post {
            success {
                notifyTeam('SUCCESS')
            }
            failure {
                notifyTeam('FAILURE')
            }
        }
    }
}
```

3. **vars/notifyTeam.groovy**:
```groovy
def call(String status) {
    def color = status == 'SUCCESS' ? 'good' : 'danger'
    def emoji = status == 'SUCCESS' ? '✅' : '❌'
    
    echo "${emoji} Build ${status}: ${env.JOB_NAME} #${env.BUILD_NUMBER}"
    
    // Uncomment for Slack integration
    // slackSend(color: color, message: "${emoji} ${env.JOB_NAME} #${env.BUILD_NUMBER} - ${status}")
}
```

4. **vars/dockerBuild.groovy**:
```groovy
def call(Map config) {
    def imageName = config.imageName
    def tag = config.tag ?: env.BUILD_NUMBER
    def dockerfile = config.dockerfile ?: 'Dockerfile'
    
    sh "docker build -t ${imageName}:${tag} -f ${dockerfile} ."
    
    if (config.push) {
        sh "docker push ${imageName}:${tag}"
    }
    
    return "${imageName}:${tag}"
}
```

5. Configure library in Jenkins:
   - Manage Jenkins → Configure System → Global Pipeline Libraries
   - Name: `my-shared-library`
   - Default version: `main`
   - Source: Git repository URL

6. Use in Jenkinsfile:
```groovy
@Library('my-shared-library@main') _

standardBuild(buildTool: 'maven', skipTests: false)
```

**Expected Output**:
- Library functions available across all pipelines
- Consistent build process across projects
- Easy updates by modifying library code

---

### Task 6: Jenkins on Kubernetes with Dynamic Agents

**Objective**: Deploy Jenkins on Kubernetes and configure dynamic pod agents.

**Jenkins Concepts Involved**:
- Jenkins on Kubernetes
- Kubernetes plugin
- Dynamic pod agents
- Pod templates

**Tools Required**:
- Kubernetes cluster (minikube, kind, or cloud)
- kubectl configured
- Helm (optional)

**Steps**:

1. Deploy Jenkins on Kubernetes:

```yaml
# jenkins-deployment.yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: jenkins
  namespace: jenkins
spec:
  replicas: 1
  selector:
    matchLabels:
      app: jenkins
  template:
    metadata:
      labels:
        app: jenkins
    spec:
      serviceAccountName: jenkins
      containers:
      - name: jenkins
        image: jenkins/jenkins:lts
        ports:
        - containerPort: 8080
        - containerPort: 50000
        volumeMounts:
        - name: jenkins-home
          mountPath: /var/jenkins_home
      volumes:
      - name: jenkins-home
        persistentVolumeClaim:
          claimName: jenkins-pvc
---
apiVersion: v1
kind: Service
metadata:
  name: jenkins
  namespace: jenkins
spec:
  type: LoadBalancer
  ports:
  - port: 8080
    targetPort: 8080
    name: http
  - port: 50000
    targetPort: 50000
    name: agent
  selector:
    app: jenkins
```

2. Create ServiceAccount with permissions:

```yaml
# jenkins-rbac.yaml
apiVersion: v1
kind: ServiceAccount
metadata:
  name: jenkins
  namespace: jenkins
---
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRoleBinding
metadata:
  name: jenkins
roleRef:
  apiGroup: rbac.authorization.k8s.io
  kind: ClusterRole
  name: cluster-admin
subjects:
- kind: ServiceAccount
  name: jenkins
  namespace: jenkins
```

3. Configure Kubernetes cloud in Jenkins:
   - Manage Jenkins → Manage Nodes → Configure Clouds → Add Kubernetes
   - Kubernetes URL: `https://kubernetes.default`
   - Jenkins URL: `http://jenkins.jenkins.svc.cluster.local:8080`

4. Create pipeline with Kubernetes agent:

```groovy
pipeline {
    agent {
        kubernetes {
            yaml '''
apiVersion: v1
kind: Pod
spec:
  containers:
  - name: maven
    image: maven:3.8-openjdk-17
    command: ['sleep', 'infinity']
    resources:
      requests:
        memory: "512Mi"
        cpu: "250m"
  - name: docker
    image: docker:24-dind
    securityContext:
      privileged: true
'''
        }
    }
    
    stages {
        stage('Build') {
            steps {
                container('maven') {
                    sh 'mvn --version'
                    sh 'echo "Building in Kubernetes pod!"'
                }
            }
        }
        
        stage('Docker') {
            steps {
                container('docker') {
                    sh 'docker --version'
                    sh 'echo "Docker available in pod!"'
                }
            }
        }
    }
}
```

**Expected Output**:
- Jenkins running as a pod in Kubernetes
- Build creates a new pod with specified containers
- Pod is automatically deleted after build completes
- Resources are dynamically allocated

---

# Interview Questions

## Beginner Level Questions

**Q1: What is Jenkins and why is it used?**
> Jenkins is an open-source automation server that automates building, testing, and deploying software. It's used to implement CI/CD pipelines, reducing manual work and catching bugs early through automated testing.

**Q2: What is the difference between Freestyle and Pipeline jobs?**
> Freestyle jobs are configured via GUI and limited in complexity. Pipeline jobs are defined as code (Jenkinsfile), version-controlled, and support complex workflows with parallel stages and conditionals.

**Q3: What is a Jenkinsfile?**
> A Jenkinsfile is a text file containing the pipeline definition written in Groovy DSL. It's stored in the source code repository, enabling "Pipeline as Code" with version control and code review.

**Q4: Explain Jenkins Controller and Agent architecture.**
> The Controller (master) manages the UI, schedules jobs, and stores configurations. Agents (slaves) are worker machines that execute builds. This distributed architecture allows scaling and running builds on different environments.

**Q5: What are Jenkins plugins?**
> Plugins extend Jenkins functionality. Examples: Git plugin for repository integration, Docker plugin for container builds, Slack plugin for notifications. Jenkins has 1,800+ plugins available.

## Intermediate Level Questions

**Q6: How do you secure credentials in Jenkins?**
> Use Jenkins Credentials Manager to store secrets (passwords, API keys, SSH keys). Reference them by ID in pipelines using `credentials()` or `withCredentials()` blocks. Never hardcode secrets in Jenkinsfiles.

**Q7: What is a Multibranch Pipeline?**
> A Multibranch Pipeline automatically creates jobs for each branch in a repository that contains a Jenkinsfile. It's ideal for GitFlow workflows, automatically managing branch-specific builds.

**Q8: Explain the difference between CI and CD.**
> CI (Continuous Integration) automatically builds and tests code on every commit. CD can mean Continuous Delivery (automated deployment to staging, manual approval for production) or Continuous Deployment (fully automated deployment to production).

**Q9: How do you integrate Docker with Jenkins?**
> Use Docker Pipeline plugin. Docker can serve as build environments (agent { docker { image 'node:18' } }), or Jenkins can build/push Docker images as part of the pipeline.

**Q10: What are webhooks and why are they better than polling?**
> Webhooks are HTTP callbacks that notify Jenkins immediately when code changes occur. They're better than polling because they provide instant triggers, reduce load on Git servers, and eliminate polling delays.

## Advanced Level Questions

**Q11: What are Jenkins Shared Libraries?**
> Shared Libraries are reusable pipeline code stored in a separate Git repository. They contain global variables (vars/) and classes (src/) that can be used across multiple Jenkinsfiles, promoting DRY principles and standardization.

**Q12: Explain static vs dynamic agents.**
> Static agents are always running (fixed cost, instant availability). Dynamic agents are created on-demand (pay-per-use, spin-up time). Dynamic agents using Docker or Kubernetes provide better resource utilization.

**Q13: How do you scale Jenkins for large organizations?**
> Scale by adding agents (horizontal scaling), using dynamic agents (Docker/Kubernetes), offloading builds from controller, implementing folder-based organization, using distributed builds, and optimizing JVM settings.

**Q14: How would you implement Jenkins on Kubernetes?**
> Deploy Jenkins controller as a Deployment with PVC for persistence. Use Kubernetes plugin for dynamic pod agents. Each build spins up a pod with required containers, which is destroyed after completion.

**Q15: What security best practices do you follow in Jenkins?**
> Enable authentication (LDAP/SSO), implement RBAC, use credentials binding for secrets, enable CSRF protection, audit logging, run agents with minimal permissions, keep Jenkins and plugins updated, and use script approval for untrusted code.

---

# Learning Checklist

## Beginner Level ✅
- [ ] Install Jenkins (Docker recommended)
- [ ] Navigate Jenkins UI and dashboard
- [ ] Create a Freestyle job with Git integration
- [ ] Understand build triggers (manual, poll SCM)
- [ ] Create a basic Declarative Pipeline
- [ ] Understand stages, steps, and post sections
- [ ] Install and use essential plugins
- [ ] View build logs and troubleshoot failures

## Intermediate Level ✅
- [ ] Write complex Jenkinsfiles with conditions
- [ ] Set up Git webhooks for instant triggers
- [ ] Create Multibranch Pipelines
- [ ] Integrate build tools (Maven/npm/Python)
- [ ] Configure and use Jenkins credentials
- [ ] Run tests and publish reports
- [ ] Use Docker as build agents
- [ ] Implement parallel stages
- [ ] Archive artifacts and fingerprinting

## Advanced Level ✅
- [ ] Create and use Shared Libraries
- [ ] Set up Jenkins Controller-Agent architecture
- [ ] Configure dynamic agents (Docker/Kubernetes)
- [ ] Deploy Jenkins on Kubernetes
- [ ] Implement security best practices (RBAC, audit)
- [ ] Optimize Jenkins performance
- [ ] Integrate with AWS services (ECR, EKS)
- [ ] Implement GitOps workflows
- [ ] Set up disaster recovery and backup

---

# Real-World DevOps Tips

## Pipeline Best Practices

1. **Keep pipelines fast**: Aim for <10 minutes. Use parallel stages, caching, and incremental builds.

2. **Fail fast**: Put quick checks (lint, unit tests) before slow ones (integration tests, deployments).

3. **Use declarative syntax**: Easier to read, maintain, and has built-in validation.

4. **Version your Jenkinsfile**: Store it in the repository root, review changes like code.

5. **Use meaningful stage names**: `Build Java Application` is better than `Stage 1`.

## Security Tips

6. **Never hardcode secrets**: Always use Jenkins Credentials Manager.

7. **Limit permissions**: Use RBAC, give minimum required access.

8. **Audit everything**: Enable audit logging, review access regularly.

9. **Keep Jenkins updated**: Security patches are critical.

10. **Use approved scripts only**: Enable script security, review approvals.

## Operational Tips

11. **Monitor Jenkins health**: CPU, memory, disk space, build queue length.

12. **Clean up regularly**: Delete old builds, workspaces, and unused jobs.

13. **Backup Jenkins home**: Regular backups of `/var/jenkins_home`.

14. **Use Configuration as Code**: JCasC plugin for reproducible setup.

15. **Document your pipelines**: README files, inline comments, runbooks.

## Scaling Tips

16. **Offload builds from controller**: Controller should only schedule, not build.

17. **Use dynamic agents**: Docker or Kubernetes for cost-effective scaling.

18. **Implement caching**: Maven/npm caches, Docker layer caching.

19. **Distribute load**: Multiple agents with different labels for different workloads.

20. **Consider alternatives**: For very large scale, evaluate Jenkins X, Tekton, or GitHub Actions.

## Troubleshooting Tips

21. **Check console output first**: Most errors are visible in build logs.

22. **Verify agent connectivity**: Network issues are common with distributed builds.

23. **Check disk space**: Full disks cause mysterious failures.

24. **Review plugin compatibility**: Plugin updates can break pipelines.

25. **Use Blue Ocean**: Better visualization for debugging complex pipelines.

---

## Quick Reference Card

```
┌─────────────────────────────────────────────────────────────────┐
│                    JENKINS QUICK REFERENCE                       │
├─────────────────────────────────────────────────────────────────┤
│ PIPELINE STRUCTURE                                               │
│ pipeline {                                                       │
│   agent any                    // Where to run                   │
│   environment { }              // Variables                      │
│   stages {                     // Main workflow                  │
│     stage('Name') {                                              │
│       steps { sh 'command' }                                     │
│     }                                                            │
│   }                                                              │
│   post { always/success/failure { } }  // Cleanup               │
│ }                                                                │
├─────────────────────────────────────────────────────────────────┤
│ COMMON COMMANDS                                                  │
│ sh 'command'                   // Run shell command              │
│ echo 'message'                 // Print message                  │
│ checkout scm                   // Checkout source code           │
│ git url: 'repo', branch: 'main' // Clone specific branch        │
│ withCredentials([...]) { }     // Use secrets                    │
│ archiveArtifacts 'path/**'     // Save build outputs            │
│ junit 'reports/*.xml'          // Publish test results          │
├─────────────────────────────────────────────────────────────────┤
│ AGENT TYPES                                                      │
│ agent any                      // Any available agent            │
│ agent { label 'linux' }        // Specific label                 │
│ agent { docker { image 'x' } } // Docker container              │
│ agent { kubernetes { yaml '' } } // Kubernetes pod              │
├─────────────────────────────────────────────────────────────────┤
│ TRIGGERS                                                         │
│ triggers { pollSCM('H/5 * * * *') }  // Poll every 5 min        │
│ triggers { cron('H 2 * * *') }       // Daily at 2 AM           │
│ triggers { githubPush() }            // GitHub webhook          │
├─────────────────────────────────────────────────────────────────┤
│ CONDITIONS                                                       │
│ when { branch 'main' }         // Only on main branch            │
│ when { expression { x == y } } // Custom condition               │
│ when { environment name: 'X', value: 'Y' }                      │
└─────────────────────────────────────────────────────────────────┘
```

---

*Last Updated: 2024 | Author: DevOps Engineer Notes*
*For the latest Jenkins documentation, visit: https://www.jenkins.io/doc/*
