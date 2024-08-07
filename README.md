# SVTECH
[SVTECH] Pre-Interview Test_SRE Position

01 - Build a script to install base servers for a production environment, and use any automation tools that suit you (ansible*, salt stack, terraform, bash...). The solution should set up the following components, and make any changes as you see fit so that we can assess your consideration for a production-grade system.
-	"Sysadmin" accounts with sudo privilege; hostname (dns); cli commands you use often.
-	Install docker daemon, specify logging driver + storage driver of your choice.
-	The server should be prepared/tuned for a high network traffic workload.
-	Logging Every Command Executed by Users and saving in a specific file.
====================================================================================================================================

Procedure:
1. Using automation tool is Ansible
2. Structure

*** hosts ***
This project uses an Ansible hosts file to define the target servers for automation tasks. Below is a description of the configuration for the hosts file.

Breakdown:
[servers]
server1 ansible_host=192.168.254.129 ansible_user=client ansible_become_timeout=30

Configuration Details
[servers]: This is a group name. All hosts listed under this section are grouped together and can be targeted collectively in Ansible playbooks.

server1: This is an alias or name for the host. It can be any identifier you choose and is used within Ansible to reference the server.

ansible_host=192.168.254.129: This defines the IP address of the host machine. Ansible will connect to this IP address when executing tasks.

ansible_user=client: This specifies the SSH user that Ansible will use to connect to the host. Ensure this user has the necessary permissions to perform the required operations.

*** setup.yml ***
This Ansible playbook is designed to set up a production-grade server with various configurations and installations. It performs the following tasks:

Overview
  Host: Applies configurations to all hosts in the inventory.
  Privilege Escalation: Uses sudo to execute tasks with elevated privileges.
  Variables: Customizable settings for user creation, hostname, Docker configuration, and system tuning.
  
Variables
  user_name: The username to create (default: Sysadmin).
  user_shell: Default shell for the new user (default: /bin/bash).
  sudoers_file: Path to the sudoers file for configuring sudo permissions (default: /etc/sudoers).
  hostname: The hostname to set for the server (default: production-grade).
  docker_logging_driver: Docker logging driver to configure (default: "json-file").
  docker_storage_driver: Docker storage driver to configure (default: "overlay2").

Tasks
Create the User
  Creates a user with the specified user_name and user_shell.

Add User to Sudoers File
  Grants the user passwordless sudo access by adding an entry to the sudoers file.

Set the Hostname
  Configures the server's hostname to the specified hostname.

Install Necessary Packages
  Installs essential packages including curl, net-tools, and vim.

Install Docker
  Installs Docker using the docker.io package.

Configure Docker Logging Driver
  Sets up the Docker daemon configuration with specified logging and storage drivers.
  Note: Triggers a handler to restart Docker if this configuration changes.

Restart Docker Service
  Restarts the Docker service to apply the new configuration.

Tune the Server for High Network Traffic Workload
  Adjusts kernel parameters to optimize the server for handling high network traffic.

Ensure auditd is Installed
  Installs the auditd package for auditing purposes.

Configure auditd to Log All Commands
  Sets up auditd rules to log command executions for both root and regular users.

Restart auditd Service
  Restarts the auditd service to apply new logging rules.

Handlers
  Restart Docker
    Restarts the Docker service if the configuration file is updated.















02 - Describe your idea to monitor the resource utilization of a Tech Stack. Draw a model that is Scalable, Resilient, and Responsive to the most traffic you have ever deployed. 

03 - When you receive an alert that a service is down or slow, what would you do to check that service and resolve the issue? Describe your steps to resolve and prevent the problem (you can describe a real situation that you have encountered).
