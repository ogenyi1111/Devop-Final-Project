Jenkins Server Installation and Configuration Guide
===============================================

1. Server Requirements
---------------------
A. Hardware Requirements:
   - CPU: 2+ cores recommended
   - RAM: 4GB minimum (8GB+ recommended)
   - Storage: 50GB+ free space
   - Network: Stable internet connection

B. Software Requirements:
   - Operating System: Ubuntu 20.04 LTS or later
   - Java: OpenJDK 11 or later
   - Git: Latest stable version
   - Firewall: UFW or similar

2. Server Preparation
--------------------
A. System Update:
   ```bash
   sudo apt update
   sudo apt upgrade -y
   ```

B. Install Dependencies:
   ```bash
   sudo apt install -y \
       openjdk-11-jdk \
       git \
       curl \
       wget \
       gnupg2 \
       software-properties-common \
       apt-transport-https \
       ca-certificates
   ```

C. Configure Firewall:
   ```bash
   sudo ufw allow 8080/tcp  # Jenkins default port
   sudo ufw allow 22/tcp    # SSH
   sudo ufw enable
   ```

3. Jenkins Installation
----------------------
A. Add Jenkins Repository:
   ```bash
   curl -fsSL https://pkg.jenkins.io/debian-stable/jenkins.io-2023.key | sudo tee \
     /usr/share/keyrings/jenkins-keyring.asc > /dev/null
   echo deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc] \
     https://pkg.jenkins.io/debian-stable binary/ | sudo tee \
     /etc/apt/sources.list.d/jenkins.list > /dev/null
   ```

B. Install Jenkins:
   ```bash
   sudo apt update
   sudo apt install jenkins
   ```

C. Start and Enable Jenkins:
   ```bash
   sudo systemctl start jenkins
   sudo systemctl enable jenkins
   sudo systemctl status jenkins
   ```

4. Initial Server Configuration
-----------------------------
A. Get Initial Admin Password:
   ```bash
   sudo cat /var/lib/jenkins/secrets/initialAdminPassword
   ```

B. Access Jenkins:
   - Open browser and navigate to: http://your-server-ip:8080
   - Enter the initial admin password
   - Choose "Install suggested plugins"

C. Create Admin User:
   - Username: admin
   - Password: (use a strong password)
   - Full name: Jenkins Administrator
   - Email: admin@yourdomain.com

5. Security Configuration
------------------------
A. Configure SSL/TLS:
   1. Generate SSL Certificate:
      ```bash
      sudo mkdir -p /etc/jenkins/ssl
      sudo openssl req -x509 -nodes -days 365 -newkey rsa:2048 \
        -keyout /etc/jenkins/ssl/jenkins.key \
        -out /etc/jenkins/ssl/jenkins.crt
      ```

   2. Configure Jenkins for HTTPS:
      ```bash
      sudo nano /etc/default/jenkins
      ```
      Add these lines:
      ```
      JENKINS_ARGS="--httpPort=-1 --httpsPort=8443 --httpsKeyStore=/etc/jenkins/ssl/jenkins.jks --httpsKeyStorePassword=your_password"
      ```

B. Configure Authentication:
   1. Go to Manage Jenkins > Configure Global Security
   2. Enable security
   3. Select "Jenkins' own user database"
   4. Configure authorization as "Matrix-based security"

6. System Configuration
----------------------
A. Configure System Resources:
   1. Edit Jenkins configuration:
      ```bash
      sudo nano /etc/default/jenkins
      ```
   2. Set JVM options:
      ```
      JAVA_OPTS="-Xmx4g -Xms2g -XX:MaxPermSize=512m -XX:+UseG1GC"
      ```

B. Configure Build Tools:
   1. Install build tools:
      ```bash
      sudo apt install -y maven gradle
      ```
   2. Configure in Jenkins:
      - Go to Manage Jenkins > Global Tool Configuration
      - Add JDK, Maven, and Gradle installations

7. Backup Configuration
----------------------
A. Install Backup Plugin:
   1. Go to Manage Jenkins > Manage Plugins
   2. Install "ThinBackup" plugin

B. Configure Backup:
   1. Go to Manage Jenkins > ThinBackup
   2. Set backup directory: /var/lib/jenkins/backup
   3. Configure backup schedule:
      - Full backup: Weekly
      - Differential backup: Daily
   4. Enable backup compression

8. Monitoring Setup
------------------
A. Install Monitoring Plugins:
   1. Go to Manage Jenkins > Manage Plugins
   2. Install:
      - Monitoring plugin
      - Metrics plugin
      - Prometheus plugin

B. Configure System Monitoring:
   1. Set up Prometheus metrics endpoint
   2. Configure alert thresholds
   3. Set up email notifications

9. Maintenance Tasks
-------------------
A. Regular Updates:
   ```bash
   sudo apt update
   sudo apt upgrade jenkins
   ```

B. Log Rotation:
   ```bash
   sudo nano /etc/logrotate.d/jenkins
   ```
   Add:
   ```
   /var/log/jenkins/jenkins.log {
       daily
       rotate 7
       compress
       delaycompress
       missingok
       notifempty
       create 0640 jenkins jenkins
   }
   ```

10. Troubleshooting
------------------
A. Common Issues:
   1. Port conflicts:
      ```bash
      sudo netstat -tulpn | grep 8080
      ```
   2. Permission issues:
      ```bash
      sudo chown -R jenkins:jenkins /var/lib/jenkins
      ```
   3. Memory issues:
      - Check logs: /var/log/jenkins/jenkins.log
      - Monitor system: htop

B. Useful Commands:
   ```bash
   # Check Jenkins status
   sudo systemctl status jenkins

   # View Jenkins logs
   sudo tail -f /var/log/jenkins/jenkins.log

   # Restart Jenkins
   sudo systemctl restart jenkins

   # Check disk space
   df -h
   ```

11. Security Best Practices
--------------------------
A. Regular Security Updates:
   ```bash
   sudo apt update
   sudo apt upgrade
   ```

B. Firewall Rules:
   ```bash
   sudo ufw status
   sudo ufw allow from trusted-ip to any port 8080
   ```

C. File Permissions:
   ```bash
   sudo chmod 600 /etc/jenkins/ssl/jenkins.key
   sudo chmod 644 /etc/jenkins/ssl/jenkins.crt
   ```

12. Performance Optimization
---------------------------
A. JVM Tuning:
   1. Edit Jenkins configuration:
      ```bash
      sudo nano /etc/default/jenkins
      ```
   2. Add/update JVM options:
      ```
      JAVA_OPTS="-Xmx4g -Xms2g -XX:MaxPermSize=512m -XX:+UseG1GC -XX:+ExplicitGCInvokesConcurrent -XX:+UseStringDeduplication"
      ```

B. Build Optimization:
   1. Configure build retention
   2. Set up workspace cleanup
   3. Configure artifact rotation

Remember to:
- Regularly backup your Jenkins configuration
- Monitor system resources
- Keep Jenkins and plugins updated
- Review security logs
- Test restore procedures periodically 

pipeline {
    agent any

    environment {
        // Docker registry configuration
        DOCKER_REGISTRY = 'your-registry.com'
        
        // Application versions
        JAVA_APP_VERSION = '1.0.0'
        NODE_APP_VERSION = '1.0.0'
        PYTHON_APP_VERSION = '1.0.0'
        REACT_APP_VERSION = '1.0.0'
        
        // Application names
        JAVA_APP_NAME = 'java-app'
        NODE_APP_NAME = 'node-app'
        PYTHON_APP_NAME = 'python-app'
        REACT_APP_NAME = 'react-app'
    }

    parameters {
        choice(
            name: 'ENVIRONMENT',
            choices: ['development', 'staging', 'production'],
            description: 'Select deployment environment'
        )
        string(
            name: 'VERSION',
            defaultValue: '1.0.0',
            description: 'Application version to deploy'
        )
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build Applications') {
            parallel {
                stage('Build Java App') {
                    steps {
                        dir('java-app') {
                            sh 'mvn clean package -DskipTests'
                        }
                    }
                }
                stage('Build Node.js App') {
                    steps {
                        dir('node-app') {
                            sh 'npm install'
                        }
                    }
                }
                stage('Build Python App') {
                    steps {
                        dir('python-app') {
                            sh 'pip install -r requirements.txt'
                        }
                    }
                }
                stage('Build React App') {
                    steps {
                        dir('react-app') {
                            sh 'npm install'
                            sh 'npm run build'
                        }
                    }
                }
            }
        }

        stage('Test Applications') {
            parallel {
                stage('Test Java App') {
                    steps {
                        dir('java-app') {
                            sh 'mvn test'
                        }
                    }
                }
                stage('Test Node.js App') {
                    steps {
                        dir('node-app') {
                            sh 'npm test'
                        }
                    }
                }
                stage('Test Python App') {
                    steps {
                        dir('python-app') {
                            sh 'python -m pytest'
                        }
                    }
                }
                stage('Test React App') {
                    steps {
                        dir('react-app') {
                            sh 'npm test'
                        }
                    }
                }
            }
        }

        stage('Build Docker Images') {
            steps {
                script {
                    // Build and tag images for all environments
                    sh """
                        docker build -t ${DOCKER_REGISTRY}/${JAVA_APP_NAME}:${params.VERSION} ./java-app
                        docker build -t ${DOCKER_REGISTRY}/${NODE_APP_NAME}:${params.VERSION} ./node-app
                        docker build -t ${DOCKER_REGISTRY}/${PYTHON_APP_NAME}:${params.VERSION} ./python-app
                        docker build -t ${DOCKER_REGISTRY}/${REACT_APP_NAME}:${params.VERSION} ./react-app
                    """
                }
            }
        }

        stage('Push Docker Images') {
            steps {
                script {
                    withCredentials([usernamePassword(credentialsId: 'docker-registry-credentials', 
                                                    passwordVariable: 'DOCKER_PASSWORD', 
                                                    usernameVariable: 'DOCKER_USERNAME')]) {
                        sh "docker login ${DOCKER_REGISTRY} -u ${DOCKER_USERNAME} -p ${DOCKER_PASSWORD}"
                        
                        sh """
                            docker push ${DOCKER_REGISTRY}/${JAVA_APP_NAME}:${params.VERSION}
                            docker push ${DOCKER_REGISTRY}/${NODE_APP_NAME}:${params.VERSION}
                            docker push ${DOCKER_REGISTRY}/${PYTHON_APP_NAME}:${params.VERSION}
                            docker push ${DOCKER_REGISTRY}/${REACT_APP_NAME}:${params.VERSION}
                        """
                    }
                }
            }
        }

        stage('Deploy to Environment') {
            steps {
                script {
                    // Load environment-specific configuration
                    def envConfig = loadEnvironmentConfig(params.ENVIRONMENT)
                    
                    // Deploy to selected environment
                    switch(params.ENVIRONMENT) {
                        case 'development':
                            deployToDevelopment(envConfig)
                            break
                        case 'staging':
                            deployToStaging(envConfig)
                            break
                        case 'production':
                            deployToProduction(envConfig)
                            break
                    }
                }
            }
        }
    }

    post {
        always {
            cleanWs()
        }
        success {
            echo "Pipeline completed successfully for environment: ${params.ENVIRONMENT}"
            emailext (
                subject: "Deployment Successful: ${currentBuild.fullDisplayName}",
                body: "Deployment to ${params.ENVIRONMENT} completed successfully. Version: ${params.VERSION}",
                to: 'team@example.com'
            )
        }
        failure {
            echo "Pipeline failed for environment: ${params.ENVIRONMENT}"
            emailext (
                subject: "Deployment Failed: ${currentBuild.fullDisplayName}",
                body: "Deployment to ${params.ENVIRONMENT} failed. Version: ${params.VERSION}",
                to: 'team@example.com'
            )
        }
    }
}

// Helper functions for environment-specific deployments
def loadEnvironmentConfig(String environment) {
    def config = [
        development: [
            namespace: 'dev',
            replicas: 1,
            resources: [cpu: '0.5', memory: '512Mi'],
            domain: 'dev.example.com'
        ],
        staging: [
            namespace: 'staging',
            replicas: 2,
            resources: [cpu: '1', memory: '1Gi'],
            domain: 'staging.example.com'
        ],
        production: [
            namespace: 'prod',
            replicas: 3,
            resources: [cpu: '2', memory: '2Gi'],
            domain: 'example.com'
        ]
    ]
    return config[environment]
}

def deployToDevelopment(config) {
    sh """
        kubectl config use-context development
        kubectl apply -f k8s/development/
        kubectl set image deployment/${JAVA_APP_NAME} ${JAVA_APP_NAME}=${DOCKER_REGISTRY}/${JAVA_APP_NAME}:${params.VERSION} -n ${config.namespace}
        kubectl set image deployment/${NODE_APP_NAME} ${NODE_APP_NAME}=${DOCKER_REGISTRY}/${NODE_APP_NAME}:${params.VERSION} -n ${config.namespace}
        kubectl set image deployment/${PYTHON_APP_NAME} ${PYTHON_APP_NAME}=${DOCKER_REGISTRY}/${PYTHON_APP_NAME}:${params.VERSION} -n ${config.namespace}
        kubectl set image deployment/${REACT_APP_NAME} ${REACT_APP_NAME}=${DOCKER_REGISTRY}/${REACT_APP_NAME}:${params.VERSION} -n ${config.namespace}
    """
}

def deployToStaging(config) {
    sh """
        kubectl config use-context staging
        kubectl apply -f k8s/staging/
        kubectl set image deployment/${JAVA_APP_NAME} ${JAVA_APP_NAME}=${DOCKER_REGISTRY}/${JAVA_APP_NAME}:${params.VERSION} -n ${config.namespace}
        kubectl set image deployment/${NODE_APP_NAME} ${NODE_APP_NAME}=${DOCKER_REGISTRY}/${NODE_APP_NAME}:${params.VERSION} -n ${config.namespace}
        kubectl set image deployment/${PYTHON_APP_NAME} ${PYTHON_APP_NAME}=${DOCKER_REGISTRY}/${PYTHON_APP_NAME}:${params.VERSION} -n ${config.namespace}
        kubectl set image deployment/${REACT_APP_NAME} ${REACT_APP_NAME}=${DOCKER_REGISTRY}/${REACT_APP_NAME}:${params.VERSION} -n ${config.namespace}
    """
}

def deployToProduction(config) {
    // Add approval step for production deployment
    timeout(time: 1, unit: 'HOURS') {
        input message: 'Approve production deployment?'
    }
    
    sh """
        kubectl config use-context production
        kubectl apply -f k8s/production/
        kubectl set image deployment/${JAVA_APP_NAME} ${JAVA_APP_NAME}=${DOCKER_REGISTRY}/${JAVA_APP_NAME}:${params.VERSION} -n ${config.namespace}
        kubectl set image deployment/${NODE_APP_NAME} ${NODE_APP_NAME}=${DOCKER_REGISTRY}/${NODE_APP_NAME}:${params.VERSION} -n ${config.namespace}
        kubectl set image deployment/${PYTHON_APP_NAME} ${PYTHON_APP_NAME}=${DOCKER_REGISTRY}/${PYTHON_APP_NAME}:${params.VERSION} -n ${config.namespace}
        kubectl set image deployment/${REACT_APP_NAME} ${REACT_APP_NAME}=${DOCKER_REGISTRY}/${REACT_APP_NAME}:${params.VERSION} -n ${config.namespace}
    """
} 