# Ansible EC2 Management Project

This project demonstrates the use of Ansible to manage AWS EC2 instances. It includes configuration for dynamic inventory and playbooks to install Apache HTTPD and Java.

## Project Structure

*   `ansible.cfg`: Ansible configuration file defining defaults like inventory location and user.
*   `inventory/`: Directory containing the dynamic inventory script.
    *   `aws_ec2.yml`: Ansible dynamic inventory plugin configuration for AWS EC2.
*   `install_httpd.yml`: Playbook to install and configure Apache HTTPD.
*   `install_java.yml`: Playbook to install OpenJDK 11.
*   `hosts`: Static inventory file (alternative/legacy).
*   `index.html`: Sample HTML file deployed by the HTTPD playbook.

## Configuration

The `ansible.cfg` file sets the following defaults:
*   **Inventory**: Points to `./inventory`, allowing Ansible to automatically use the dynamic inventory.
*   **Remote User**: Sets the default remote user to `ansible`.
*   **Host Key Checking**: Disabled to facilitate easier connections to new instances.
*   **Logging**: Execution logs are saved to `ansible.log`.

## Inventory Management

### Dynamic Inventory (`inventory/aws_ec2.yml`)
This project utilizes the `amazon.aws.aws_ec2` plugin to dynamically fetch running EC2 instances from the `ap-south-1` region.
*   **Grouping**: Instances are grouped by their Name tag.
*   **Addressing**: Uses the public IP address as the `ansible_host`.

### Static Inventory (`hosts`)
A simple static inventory file is included as a reference or manual override, defining a single host with connection details.

## Playbooks

### Install Apache HTTPD (`install_httpd.yml`)
This playbook performs the following tasks on all target hosts:
1.  Installs the latest version of Apache (`httpd`).
2.  Starts and enables the `httpd` service.
3.  Deploys the local `index.html` file to `/var/www/html/index.html` on the server.

### Install Java (`install_java.yml`)
This playbook sets up the Java environment:
1.  Installs OpenJDK 11 (`java-11-amazon-corretto-devel`).
2.  Verifies the installation by running `java -version`.
3.  Displays the installed Java version in the output.

## Execution Flow

To run the playbooks, ensure you have Ansible installed and AWS credentials configured.

**Run HTTPD Installation:**
```bash
ansible-playbook install_httpd.yml
```

**Run Java Installation:**
```bash
ansible-playbook install_java.yml
```

*Note: Since `ansible.cfg` points to the inventory directory, you don't need to specify `-i` unless you want to override it.*

## Key Learning Points

*   **Ansible Architecture**: Understanding the roles of configuration files, inventory, and playbooks.
*   **Dynamic Inventory**: Leveraging plugins to manage cloud infrastructure dynamically, removing the need for static IP lists.
*   **Playbook Design**: Structuring playbooks with `hosts`, `vars`, and `tasks`.
*   **Module Usage**:
    *   `yum`: For package management.
    *   `service`: For managing system services.
    *   `copy`: For file deployment.
    *   `command` & `debug`: For execution verification and troubleshooting.
*   **Idempotency**: Writing tasks that can be run multiple times without unintended side effects.
