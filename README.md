# End-to-End LAMP Stack Automation and Monitoring on AWS EC2 using Ansible, Prometheus, and Grafana
# Overview
This project automates the deployment of a LAMP stack (Linux, Apache, MySQL, PHP) on AWS EC2 instances running Ubuntu using Ansible, and integrates Prometheus and Grafana for monitoring the infrastructure. The goal is to address the challenges of manually setting up a LAMP stack, which is time-consuming and error-prone, while providing real-time monitoring to ensure system reliability and performance. The automation provisions a fully functional LAMP stack and sets up monitoring dashboards with a single Ansible playbook.
# Problem Statement
Manually configuring a LAMP stack and monitoring it on EC2 involves:
* Provisioning and configuring an EC2 instance with Apache, MySQL, and PHP.
* Ensuring secure and consistent setup across environments- Securing the server, database, and application.
* Setting up monitoring tools to track system performance, resource usage, and application health.These tasks are repetitive, susceptible to errors, and
* challenging to scale. Additionally, without monitoring, identifying performance bottlenecks or failures is difficult.

# Objective
The objective is to create an Ansible playbook that automates the provisioning, configuration, and monitoring of a LAMP stack on an Ubuntu-based AWS EC2 instance. The playbook ensures:
* Automated deployment of a secure and functional LAMP stack.
* Integration of Prometheus and Grafana for real-time monitoring of server metrics (CPU, memory, disk, network) and application performance.
* Consistent, repeatable, and scalable deployments.
* A sample PHP application to verify the stack.
* Visual dashboards for monitoring system health.

# Solution Approach
The solution uses Ansible to orchestrate the deployment and configuration process, with Prometheus and Grafana for monitoring. The approach includes:
* EC2 Instance Setup: Launch an Ubuntu EC2 instance with security groups allowing HTTP, SSH, and monitoring ports (e.g., 9090 for Prometheus, 3000 for Grafana).
# Ansible Playbook: Write a playbook to:
* Install and configure Apache, MySQL, and PHP.
* Deploy a sample PHP application to verify the stack.
* Install Prometheus to collect metrics from the EC2 instance and Apache.
* Install Grafana and configure dashboards for visualizing metrics.
* Secure the setup (e.g., MySQL root password, firewall rules).

  ![networ server  node expoter](https://github.com/rukevweubio/End-to-End-LAMP-Stack-Automation-and-Monitoring-on-AWS-EC2-using-Ansible-Prometheus-Grafana-/blob/main/ansibles/picture/Screenshot%20(692).png)

    ![networ server grafana](https://github.com/rukevweubio/End-to-End-LAMP-Stack-Automation-and-Monitoring-on-AWS-EC2-using-Ansible-Prometheus-Grafana-/blob/main/ansibles/picture/Screenshot%20(691).png)

![networ server](https://github.com/rukevweubio/End-to-End-LAMP-Stack-Automation-and-Monitoring-on-AWS-EC2-using-Ansible-Prometheus-Grafana-/blob/main/ansibles/picture/Screenshot%20(686).png)


# Monitoring Setup:
Configure Prometheus to scrape metrics from the node exporter (system metrics) and Apache exporter (web server metrics), and integrate with Grafana for visualization.

# Testing: Verify the LAMP stack with a PHP page and monitor system health via Grafana dashboards.

# Tools Used
* Ansible: Automation tool for provisioning and configuration.
* AWS EC2: Cloud platform hosting the Ubuntu server.
* Ubuntu 20.04 LTS: Operating system for the EC2 instance.
* Apache2: Web server for serving PHP applications.
* MySQL: Relational database management system.
* PHP: Server-side scripting language.
* Prometheus: Time-series database for collecting and storing metrics.
* Grafana: Visualization platform for creating monitoring dashboards.
* Node Exporter: Prometheus exporter for system metrics.
* Apache Exporter: Prometheus exporter for Apache metrics.
* Cron for automation of the log file to S3 bucket
* s3 bucket for storage of  logs 

```
inventory.ini: Specifies the EC2 instance for Ansible.
playbooks.yaml: Orchestrates deployment and monitoring.
template: Modular tasks for Apache, MySQL, PHP, Prometheus, and Grafana.
ansible.cfg: Configures Ansible settings.
```

# Prerequisites
Before running the project, ensure you have:
* AWS Account with EC2 access.
* Ansible installed (tested with Ansible 2.9+).
* SSH Key Pair for EC2 access.
* Python 3 and boto3 (for AWS integration, if using dynamic inventory).
* AWS CLI configured with credentials.
* Prometheus and Grafana accounts (optional for prebuilt dashboards).

# Setup Instructions
```
Clone the Repository:
git clone https://github.com/rukevweubio/End-to-End-LAMP-Stack-Automation-and-Monitoring-on-AWS-EC2-using-Ansible-Prometheus-Grafana
cd End-to-End-LAMP-Stack-Automation-and-Monitoring-on-AWS-EC2-using-Ansible-Prometheus-Grafana
```

# Configure AWS EC2:
* Launch an Ubuntu 20.04 EC2 instance (t2.micro or larger recommended).
* Configure the security group to allow:
* Port 22 (SSH) for Ansible.
* Port 80 (HTTP) for Apache.
* Port 9090 (Prometheus).
* Port 3000 (Grafana).
* Note the instance’s public IP or DNS.


# Update Inventory:

```Edit inventory/ec2.yml:all:
  hosts:
    lamp_monitoring:
      ansible_host: <EC2_PUBLIC_IP>
      ansible_user: ubuntu
      ansible_ssh_private_key_file: /path/to/your-key.pem
```




# Run the Playbook:
```ansible-playbook -i inventory.ini playbooks.yaml```


# Verify the Deployment:
* LAMP Stack: Open http://<EC2_PUBLIC_IP>/index.php to see a sample PHP page with PHP info and MySQL connection status.
* Prometheus: Access http://<EC2_PUBLIC_IP>:9090 to verify metrics collection.
* Grafana: Log in at http://<EC2_PUBLIC_IP>:3000 (default user: admin, password: set in variables) and check dashboards for system and Apache metrics.



# Sample Playbook (lamp_monitoring.yml)
```---
- name: Deploy LAMP stack with monitoring on EC2
  hosts: lamp_monitoring
  become: yes
  roles:
    - apache
    - mysql
    - php
    - prometheus
    - grafana
```

# Monitoring Dashboards
Grafana dashboards display:
* System Metrics: CPU usage, memory, disk I/O, network traffic (via Node Exporter).
* Apache Metrics: Request rate, response times, error rates (via Apache Exporter).
* Custom Metrics: Add PHP or MySQL-specific exporters for deeper insights.

# Challenges Faced
* Port Conflicts: Ensuring no conflicts between Apache, Prometheus, and Grafana ports.
* MySQL Automation: Automating mysql_secure_installation non-interactively required custom scripting.
* Prometheus Scraping: Configuring Prometheus to scrape metrics reliably across dynamic EC2 IPs.
* Grafana Setup: Automating Grafana datasource and dashboard provisioning was complex due to API dependencies.
* Resource Constraints: Free-tier EC2 instances struggled with the combined load of LAMP, Prometheus, and Grafana.

# Possible Future Improvements
* Dynamic Inventory: Use Ansible’s AWS dynamic inventory for auto-discovery of EC2 instances.
* Auto-Scaling: Deploy the LAMP stack across multiple EC2 instances with an Application Load Balancer.
* SSL/TLS: Add Let’s Encrypt for HTTPS support.
* Advanced Monitoring: Integrate MySQL and PHP exporters for application-specific metrics.
* Backup Automation: Add automated MySQL backups to S3.
* CI/CD Pipeline: Integrate with GitHub Actions or Jenkins for automated deployments.
* Containerization: Migrate to Docker or Kubernetes for portability and scalability.


