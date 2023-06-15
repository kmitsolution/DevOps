## Ansible Modules
Ansible provides a vast collection of modules, which are pre-built, reusable units of automation that enable you to perform specific tasks on managed hosts. Modules are the building blocks of Ansible playbooks and can be invoked to carry out various actions. Here are some commonly used Ansible modules:

### System Modules:

1. <b>apt:</b> Manages packages on Debian-based systems.
2. <b>yum:</b> Manages packages on Red Hat-based systems.
3. <b>dnf:</b> Manages packages on newer versions of Fedora and CentOS.
4. <b>service:</b> Manages system services.
5. <b>user:</b> Manages user accounts and their attributes.
6. <b>group:</b> Manages groups and their attributes.
7. <b>file:</b> Manages files and directories, including permissions and ownership.
8. <b>template:</b> Manages the creation of files using Jinja2 templates.
9. <b>copy:</b> Copies files from the control machine to managed hosts.
10. <b>lineinfile:</b> Manages lines in files, such as adding, modifying, or removing lines.
11. <b>command or shell:</b> Executes commands on the managed hosts.
12. <b>raw:</b> Executes commands on the managed hosts without the need for Python.

### Cloud Modules:

<b>ec2:</b> Manages Amazon EC2 instances.
<b>azure_rm:</b> Manages resources in Microsoft Azure.
<b>gcp_compute_instance:</b> Manages Google Cloud instances.
<b>digital_ocean_droplet:</b> Manages DigitalOcean droplets.


### Networking Modules:

1. <b>ios_command:</b> Executes commands on Cisco IOS devices.
2. <b>nxos_command:</b> Executes commands on Cisco Nexus devices.
3. <b>netmiko_send_command:</b> S0ends commands to network devices using the Netmiko library.

### Containerization Modules:

1. <b>docker_container:</b> Manages Docker containers.
2. <b>docker_image:</b> Manages Docker images.
3. <b>k8s:</b> Manages Kubernetes resources.

### Database Modules:

1. <b>mysql_db:</b> Manages MySQL databases.
2. <b>postgresql_db:</b> Manages PostgreSQL databases.

These are just a few examples of the extensive module library available in Ansible. You can find many more modules for various tasks, such as managing cloud resources, network devices, load balancers, version control systems, and more. Ansible modules are continuously updated and expanded by the community, providing a wide range of options for automating different aspects of infrastructure and application management.
