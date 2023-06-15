## Ansible Inventory file

An Ansible controller needs a list of hosts and groups of hosts upon which commands, modules, and tasks are performed on the managed nodes, this list is known as inventory. It may contain information such as – Host IPs, DNS Name, SSH User and Pass, SSH Port service info (in case it is other than port 22). The most common formats are INI and YAML. An inventory file is also sometimes called a host file. We will be using INI format in this guide. 


```
[webservers]
10.0.0.9
10.0.0.10

[dbservers]
10.0.0.11
10.0.0.12
```
### Alias Name

```
    webserver01 ansible_host=10.0.0.9
    [webservers]
    webserver01
```
#### Another Example
```
[webservers]
web1 ansible_host=192.168.1.10 ansible_user=ubuntu
web2 ansible_host=192.168.1.11 ansible_user=ubuntu

[databases]
db1 ansible_host=192.168.1.20 ansible_user=root

[loadbalancers]
lb1 ansible_host=192.168.1.30 ansible_user=admin

[all:vars]
ansible_ssh_private_key_file=/path/to/private_key.pem

```
In this example, the inventory file defines three groups of hosts: <b>[webservers], [databases], and [loadbalancers]</b>. Each group is followed by a list of hosts, identified by a hostname or IP address. Additionally, you can provide specific Ansible variables for each host, such as ansible_host to specify the IP address and ansible_user to specify the SSH user.

The <b>[all:vars]</b> section allows you to define variables that apply to all hosts in the inventory. In this case, ansible_ssh_private_key_file is set to specify the SSH private key file to use for authentication.

The inventory file can also include group variables, children groups, and more advanced configurations. You can use YAML or JSON formats for more complex inventory structures, including nested groups, variable inheritance, and dynamic inventory scripts.
### Creating custom inventory file

By default, Ansible looks for the inventory file at /etc/ansible/hosts, but you can specify a different inventory file using the -i or --inventory option when running Ansible commands.

#### Step 1 — Disabling host key checking 
Firstly, make a change in ansible.cfg file which is located at /etc/ansible directory

Uncomment the line host_key_checking = False. This is to disable SSH key host checking:
#### Step 2 — Create an inventory file
In /etc/ansible/ directory, create an inv.txt file, and add the below details to it:
```
            webserver01 ansible_host=10.0.0.9

            [webservers]
            webserver01
```
### Group InventoryFile
```
[webservers]
10.0.0.9

[dbservers]
10.0.0.10

[production:children]
dbservers
webservers
```
