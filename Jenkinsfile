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