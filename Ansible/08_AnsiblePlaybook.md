## Ansible Playbook
An Ansible playbook is a YAML file that contains a set of plays, tasks, and other configurations that define the desired state of a system and the actions to be executed on remote hosts. Playbooks allow you to automate tasks and manage infrastructure as code.

1. <b>Play:</b> A play is the top-level structure in an Ansible playbook. It consists of a set of tasks that are executed on a defined group of hosts. Each play has a name, hosts specification, and a list of tasks.
2. <b>Task:</b> A task represents a single action that Ansible should perform on a target host. It typically involves invoking a module with specific parameters to carry out a specific action. Tasks are executed sequentially in the order they appear in the playbook.
3. <b>Module:</b> Modules are pre-defined units of code that Ansible uses to perform specific tasks on remote hosts. They encapsulate functionality like package installation, file manipulation, service management, and more. Modules are invoked within tasks to carry out actions on the target hosts.
4. <b>Inventory:</b> The inventory is a file or a collection of files that define the hosts and host groups on which Ansible should operate. It provides the information needed to connect to remote hosts, such as IP addresses, domain names, SSH credentials, and host groupings.
5. <b>Variables:</b> Variables allow you to define dynamic values that can be used throughout the playbook. They enable flexibility and reusability by separating data from the playbook logic. Variables can be defined at different levels, such as playbook-level variables, host-level variables, or group-level variables.
6. <b>Handlers:</b> Handlers are special tasks that are triggered by other tasks using a notification mechanism. They are typically used to perform actions that need to be executed only when specific conditions are met. For example, restarting a service after a configuration file change.
7. <b>Roles:</b> Roles are a way to organize and encapsulate reusable parts of a playbook. A role is a collection of files, tasks, templates, and other components that can be easily included in multiple playbooks. Roles provide a modular and structured approach to playbook development.

These components work together to define the desired state of a system and the tasks required to achieve that state. By combining plays, tasks, modules, variables, and other components, you can create powerful and flexible automation workflows with Ansible.

### Example

```yaml
---
- name: Example Playbook
  hosts: web_servers
  become: true

  tasks:
    - name: Install Nginx
      apt:
        name: nginx
        state: present

    - name: Start Nginx service
      service:
        name: nginx
        state: started
        enabled: true
```

In this example playbook:

The <b>name</b> field provides a description for the playbook.
The <b>hosts</b> field specifies the target hosts or host groups on which the playbook will be executed. In this case, the playbook will run on hosts belonging to the web_servers group.
The <b>become</b> field indicates that privilege escalation is required (using sudo or su) to execute the tasks.

The playbook contains two tasks:

1. The first task uses the apt module to install Nginx on the target hosts.
2. The second task uses the service module to start the Nginx service and enable it to start automatically on system boot.

To execute the playbook, you can use the ansible-playbook command:
```
ansible-playbook myplaybook.yml
```

