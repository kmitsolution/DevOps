Adhoc Commands
Ansible ad hoc commands allow you to execute one-off tasks or commands on managed hosts without the need to create a playbook. Ad hoc commands are useful for quick tasks, troubleshooting, or performing actions that do not require a complex playbook structure. Here's the basic syntax for running ad hoc commands in Ansible:
```
ansible <host-pattern> -m <module> -a "<module_arguments>"
```
Let's break down the components:

1. <b><host-pattern>:</b> Specifies the target hosts or groups of hosts on which the ad hoc command will be executed. This can be a single host, a group name from the inventory, or a pattern that matches multiple hosts.
2. <b>-m <module>:</b> Specifies the Ansible module to use for the ad hoc command. Modules provide specific functionality for the task you want to perform. For example, the command module is used to execute shell commands, the copy module is used for file copying, and the ping module is used to check host connectivity.
3. <b>-a "<module_arguments>":</b> Specifies the arguments or parameters for the selected module. The module arguments depend on the chosen module and the task you want to perform. For example, if using the command module, you can provide the shell command as an argument.

If the configuration is not complex( do not have conditional statements or loops etc.) then we can use the Adhoc commands.

## ping Module:

#### To check web servers are reachable or not using Adhoc commands.
```
ansible webservers -m ping
```
#### To check all hosted servers are reachable or not using Adhoc commands.
```
ansible all -m ping
```
#### To check webserver1 is reachable or not using Adhoc command.
```
ansible 192.168.33.11 -m ping
```
#### To check all hosted servers are reachable or not using Adhoc commands when the hosts file is in a different location (/home/vagrant/mydir/hosts)
```
ansible -i /home/vagrant/mydir/hosts all -m ping
```
## shell module

#### Install apache on webservers
```
ansible webservers -m shell -a " apt update && apt install apache2 -y"
```
#### Uninstall apache on webservers
```
ansible webservers -m shell -a " apt purge apache2 -y"
```
#### Create a file in /tmp/1.txt on dbservers and webservers
```
ansible webservers,dbservers -m shell -a "touch /tmp/1.txt"
```
#### Change the permission of /tmp/1.txt file by give rw permission to owner, r permission to group, and other users.
```
ansible webservers,dbservers -m shell -a "chmod 644 /tmp/1.txt"
```
#### Copy /tmp/1.txt file to /tmp/2.txt file
```
ansible webservers,dbservers -m shell -a "cp /tmp/1.txt /tmp/2.txt"
```

## apt
#### Install apache on webservers
```
ansible webservers -m apt -a "name=apache2 state=present"
```
#### Uninstall apache package
```
ansible webservers -m apt -a "name=apache2 state=absent purge=yes"
```

## service module

#### Start apache server on webservers
```
 ansible webservers -m service -a "name=apache2 state=started"
```
#### Stop apache service on webservers
```
ansible webservers -m service -a "name=apache2 state=stopped"
```
#### Restart apache service on webservers
```
ansible webservers -m service -a "name=apache2 state=restarted"
```
## copy module

 #### Copy a file /tmp/status.txt from master server to webservers
```
  ansible webservers -m copy -a "src=/tmp/status.txt dest=/tmp/status.txt"
```
 #### Copy a file /tmp/status.txt from master server to webservers at /tmp/newstatus.txt
```
  ansible webservers -m copy -a "src=/tmp/status.txt dest=/tmp/newstatus.txt"
```
 #### Create a file on webservers /tmp/4.txt with content Hello World
```
  ansible webservers -m copy -a "content='Hello World' dest=/tmp/4.txt"
```
