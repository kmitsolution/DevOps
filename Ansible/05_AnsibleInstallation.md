## Ansible Installation on Ubuntu
1. On master server, generate the SSH key with <b>ssh-keygen</b>
2. Copy the generated SSH keys onto the hosts using copy id command
3. Before installing Ansible package add Ansible repository to your system
```
  sudo apt-add-repository ppa:ansible/ansible
```
4. Run the update command before installing to update existing packages
```
  sudo apt-get update
```
5. Now install the Ansible package
```
   sudo apt-get install ansible
```
6. You can check if you’re on the latest version of Ansible by running the version command
```
ansible --version
```
  Syntax: ansible --version
