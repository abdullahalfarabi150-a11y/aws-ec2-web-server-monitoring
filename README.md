# AWS EC2 Web Server with Monitoring & Alerts​

##  Project Overview:
This project demonstrates the deployment, automation, monitoring, and alerting of a custom Apache web server on Amazon EC2.

I used Amazon Linux 2023 and automated the Apache installation and website deployment using EC2 User Data. I then implemented Amazon CloudWatch to monitor CPU utilization and configured a CloudWatch Alarm with Amazon SNS to send email notifications when sustained high CPU utilization was detected.

##  Architecture Flow:

User → Internet → EC2 → Apache → Custom HTML Website

EC2 CPU Utilization → CloudWatch → CloudWatch Alarm → SNS → Email Notification

This architecture demonstrates a basic AWS cloud environment combining compute, web hosting, automation, monitoring, and automated alerting.

##  Technologies & Services Used:
AWS Services
- Amazon EC2 
- Amazon CloudWatch 
- Amazon SNS 
- AWS Security Groups 
- EC2 User Data

Operating System & Web Server
- Amazon Linux 2023 
- Apache HTTP Server

Programming & Tools
- Bash / Shell Scripting 
- HTML 
- SSH

##  Deployment & Configuration:

### 1. EC2 Instance Deployment
An Amazon EC2 instance was launched using Amazon Linux 2023 with a t3.micro instance type and a public IPv4 address enabled. 

A Security Group was configured to allow:

HTTP (Port 80) from anywhere for public website access.
SSH (Port 22) from my IP address for secure remote administration.

### 2. Automated Apache Deployment
EC2 User Data was used to automate the initial server configuration.

During the instance launch process, the Bash script automatically:

- Updated the operating system packages
- Installed the Apache HTTP Server (httpd)
- Started the Apache service
- Enabled Apache to start automatically after reboot
- Configured permissions for the web directory
- Created and deployed the initial HTML webpage

This automated the initial web server setup without requiring manual Apache installation after connecting to the instance.

### 3. Custom Webpage
The initial webpage was deployed automatically through the User Data script. After verifying that the Apache server was working successfully, I updated and redesigned the webpage to create a more customized final website for the project. 

The final webpage is hosted at:

/var/www/html/index.html

The website was accessed through the EC2 instance's public IPv4 address.

## User Data Script:

```bash
#!/bin/bash
dnf update -y
dnf install -y httpd
systemctl start httpd
systemctl enable httpd
chown -R ec2-user:ec2-user /var/www/html

cat <<EOF > /var/www/html/index.html
<!DOCTYPE html>
<html>
<head>
<title>My Apache Server</title>
</head>
<body>
<h1>Welcome to My Custom Apache Web Server!</h1>
<p>Deployed on AWS EC2 using User Data.</p>
</body>
</html>
EOF

systemctl restart httpd
```
Note: The User Data script created the initial webpage during EC2 deployment. After confirming the deployment was successful, I later modified the HTML file to create the final customized webpage.

## Monitoring & Alerting:
After successfully deploying the Apache web server, I extended the project by implementing Amazon CloudWatch monitoring and Amazon SNS email alerting.

### 1. CloudWatch Monitoring
Amazon CloudWatch was configured to monitor the EC2 instance's CPU Utilization metric. A CloudWatch Alarm was created to detect sustained high CPU utilization.

Alarm Configuration:

- Metric: CPU Utilization
- Period: 5 minutes
- Datapoints to alarm: 1 out of 1
- Alarm action: Send notification through Amazon SNS

The 1 out of 1 configuration means the alarm enters the ALARM state when one evaluated 5 minute datapoint meets the configured threshold. The alarm enters the ALARM state when the configured CPU condition is met across the required evaluation periods.

### 2. SNS Email Alerting
Amazon SNS was integrated with the CloudWatch Alarm to provide email notifications.

The configuration included:

- Creating an SNS topic
- Adding an email subscription
- Confirming the email subscription
- Connecting the SNS topic to the CloudWatch Alarm

When the alarm condition is met, CloudWatch changes the alarm to the ALARM state and sends a notification through SNS.

### 3. Monitoring Test
The monitoring system was tested by generating CPU load on the EC2 instance using:

yes > /dev/null &

This increased CPU utilization and allowed the CloudWatch Alarm to be tested.

After testing, the CPU-intensive process was stopped using:

pkill yes

The test demonstrated the complete monitoring and alerting workflow:

EC2 → CloudWatch → CloudWatch Alarm → SNS → Email Notification

## Screenshots:

### 1. EC2 Instance Dashboard
![image alt](https://github.com/abdullahalfarabi150-a11y/apache-ec2-project/blob/423001b900f55ec2ad4ff1d0b7cf881463e323cf/ec2-instance-dashboard.png)

Shows the running Amazon EC2 instance and its key configuration used to host the Apache web server.

### 2. Security Group Configuration
![image alt](https://github.com/abdullahalfarabi150-a11y/apache-ec2-project/blob/e960261385394650989f73954a80ce71691c0496/Security-group-config.png)

Shows the network access rules configured to securely manage the instance through SSH and provide public HTTP access.

### 3. Final Website Output
![image alt](https://github.com/abdullahalfarabi150-a11y/apache-ec2-project/blob/92b85de7e9b8c9f0d6b4c3487717496f4b3dab53/final-website.png)

Shows the final customized webpage successfully hosted by Apache and accessed through the EC2 instance's public IPv4 address.

### 4. CloudWatch Alarm Configuration
![image alt](https://github.com/abdullahalfarabi150-a11y/apache-ec2-project/blob/51deb0886ee8416a50d2223af77c51f71baae22e/alarm-configuration.png)

Shows the CPU utilization alarm configured with a 5 minute evaluation period and 1 out of 1 datapoints required to trigger the alarm.

### 5. CloudWatch Alarm State
![image alt](https://github.com/abdullahalfarabi150-a11y/apache-ec2-project/blob/889a6b8379ebded32b1dcf5c05121ae982c61cec/cloudwatch-alarm-state.png)

Shows the CloudWatch Alarm entering the ALARM state after sustained high CPU utilization was generated during testing.

### 6. SNS Configuration
![image alt](https://github.com/abdullahalfarabi150-a11y/apache-ec2-project/blob/fd116041c0bbcb6248d4ce2a5f0078348802aa71/sns-configurartion.png)

Shows the Amazon SNS topic and confirmed email subscription configured to receive CloudWatch alarm notifications.

### 7. SNS Email Notification
![image alt](https://github.com/abdullahalfarabi150-a11y/apache-ec2-project/blob/9b515127403ee6b6aa977dfbc2548585faf49d8f/sns-email-notification.png) 

Shows the email notification successfully received after the CloudWatch Alarm was triggered.

## Project Outcome:
Successfully deployed and monitored a custom Apache web server on Amazon EC2.

The project demonstrates an end-to-end AWS workflow covering:

- Automated Apache deployment using EC2 User Data
- Custom HTML webpage hosting
- Secure network access using AWS Security Groups
- EC2 CPU monitoring using Amazon CloudWatch
- Automated CPU utilization alerting
- Email notifications using Amazon SNS
- End-to-end testing of the monitoring and alerting workflow

The completed solution demonstrates how AWS services can be combined to deploy, monitor, and manage a basic web server environment.
