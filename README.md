# SVTECH
[SVTECH] Pre-Interview Test_SRE Position
***
01 - Build a script to install base servers for a production environment, and use any automation tools that suit you (ansible*, salt stack, terraform, bash...). The solution should set up the following components, and make any changes as you see fit so that we can assess your consideration for a production-grade system.
================================================================================
-	"Sysadmin" accounts with sudo privilege; hostname (dns); cli commands you use often.
-	Install docker daemon, specify logging driver + storage driver of your choice.
-	The server should be prepared/tuned for a high network traffic workload.
-	Logging Every Command Executed by Users and saving in a specific file.


Procedure:
1. Using automation tool is Ansible

      Installing Ansible on Ubuntu
- sudo apt update
- sudo apt install software-properties-common
- sudo add-apt-repository --yes --update ppa:ansible/ansible
- sudo apt install ansible
 
2. Structure

*** hosts ***
This project uses an Ansible hosts file to define the target servers for automation tasks. Below is a description of the configuration for the hosts file.

Breakdown:
[servers]
server1 ansible_host=192.168.254.129 ansible_user=client ansible_become_timeout=30

Configuration Details

[servers]: 
- This is a group name. All hosts listed under this section are grouped together and can be targeted collectively in Ansible playbooks.

server1: 
- This is an alias or name for the host. It can be any identifier you choose and is used within Ansible to reference the server.

ansible_host=192.168.254.129:
- This defines the IP address of the host machine. Ansible will connect to this IP address when executing tasks.

ansible_user=client: 
- This specifies the SSH user that Ansible will use to connect to the host. Ensure this user has the necessary permissions to perform the required operations.

*** setup.yml ***
This Ansible playbook is designed to set up a production-grade server with various configurations and installations. It performs the following tasks:

Overview
  Host: 
  - Applies configurations to all hosts in the inventory.
  Privilege Escalation:
  - Uses sudo to execute tasks with elevated privileges.
  Variables:
  - Customizable settings for user creation, hostname, Docker configuration, and system tuning.
  
Variables
  user_name: 
  - The username to create (default: Sysadmin).
  user_shell:
  - Default shell for the new user (default: /bin/bash).
  sudoers_file:
  - Path to the sudoers file for configuring sudo permissions (default: /etc/sudoers).
  hostname:
  - The hostname to set for the server (default: production-grade).
  docker_logging_driver:
  - Docker logging driver to configure (default: "json-file").
  docker_storage_driver:
  - Docker storage driver to configure (default: "overlay2").

Tasks
Create the User
-  Creates a user with the specified user_name and user_shell.

Add User to Sudoers File
-  Grants the user passwordless sudo access by adding an entry to the sudoers file.

Set the Hostname
-  Configures the server's hostname to the specified hostname.

Install Necessary Packages
-  Installs essential packages including curl, net-tools, and vim.

Install Docker
-  Installs Docker using the docker.io package.

Configure Docker Logging Driver
-  Sets up the Docker daemon configuration with specified logging and storage drivers.
-  Note: Triggers a handler to restart Docker if this configuration changes.

Restart Docker Service
-  Restarts the Docker service to apply the new configuration.

Tune the Server for High Network Traffic Workload
-  Adjusts kernel parameters to optimize the server for handling high network traffic.

Handlers
-  Restart Docker service if the configuration file is updated.


***
02 - Describe your idea to monitor the resource utilization of a Tech Stack. Draw a model that is Scalable, Resilient, and Responsive to the most traffic you have ever deployed. 
================================================================================
***
03 - When you receive an alert that a service is down or slow, what would you do to check that service and resolve the issue? Describe your steps to resolve and prevent the problem (you can describe a real situation that you have encountered).
================================================================================
1. Acknowledge the Alert
  Verify the Alert: Confirm that the alert is legitimate and not a false positive.
  Check Alert Details: Review the alert information to understand which service is affected and the nature of the issue (e.g., downtime, high latency).
2. Initial Assessment
  Access Monitoring Tools: Use monitoring dashboards (e.g., Grafana, Datadog) to get a quick overview of the affected service's performance metrics.
  Review Recent Changes: Check if there have been any recent changes or deployments that might have impacted the service.
3. Check Service Status
  Service Logs: Access and review the logs of the affected service. Look for error messages, stack traces, or unusual activity around the time the issue started.
  System Metrics: Examine system-level metrics (CPU, memory, disk I/O, network usage) to identify resource bottlenecks.
  Service Health: Use tools like systemctl, docker ps, or kubectl (for Kubernetes) to check the status of the service and its dependencies.

  Example commands:
  Check service status:
  systemctl status my-service
  
  View logs:
  journalctl -u my-service
  
  Check resource usage:
  top or
  df -h

4. Isolate the Problem
  Network Connectivity: Verify network connectivity between service components. Use ping, traceroute, or similar tools.
  Dependency Check: Ensure that all service dependencies (e.g., databases, third-party APIs) are functioning correctly.
  Application Performance: Test the application’s response times and request handling. Use tools like curl or httping to check endpoints.
 
  Example commands:
  Check network connectivity:
  ping -c 4 my-service

  Test API endpoint:
  curl -I http://my-service/api/health

5. Troubleshoot and Resolve
  Resource Limits: If resource constraints are identified, consider increasing resource limits or optimizing resource usage.
  Configuration Issues: Review and correct any misconfigurations in service settings.
  Restart Services: If a service is unresponsive, restart it and monitor if the issue is resolved.
 
  Example commands:
  Restart service:
  systemctl restart my-service
  
  Scale service (Kubernetes example):
  kubectl scale deployment my-service --replicas=3

6. Document and Communicate
  Incident Report: Document the issue, steps taken to resolve it, and any findings. Include timestamps, affected components, and resolution details.
  Notify Stakeholders: Communicate with relevant stakeholders about the incident resolution and any potential impacts.

7. Post-Incident Review
  Root Cause Analysis (RCA): Conduct an RCA to identify the underlying causes of the issue and prevent recurrence.
  Update Documentation: Ensure that runbooks, documentation, and monitoring configurations are updated based on the findings.
  Implement Improvements: Apply any necessary changes to improve service resilience and monitoring.

8. Preventive Measures
  Monitoring and Alerts: Enhance monitoring to capture more granular metrics and set up appropriate alerts for early detection of issues.
  Load Testing: Conduct load testing to identify and address performance bottlenecks before they impact production.
  Disaster Recovery: Implement and test disaster recovery plans to ensure quick recovery in case of future incidents.


***
Reference
================================================================================
Install Ansible on Ubuntu:
https://docs.ansible.com/ansible/latest/installation_guide/installation_distros.html#installing-ansible-on-ubuntu
