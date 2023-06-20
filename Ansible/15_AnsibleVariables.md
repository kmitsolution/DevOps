## Variables in Ansible
In Ansible, variables are used to store and manipulate data within playbooks and roles. They allow you to make your playbooks more dynamic and reusable. 

There are several types of variables in Ansible:
1. <b>Inventory Variables:</b> Variables defined in the inventory file (hosts) or in host-specific files (host_vars and group_vars). These variables are associated with specific hosts or groups.
2. <b>Playbook Variables:</b> Variables defined within a playbook using the vars keyword at the play or task level. These variables are accessible only within that specific playbook.
3. <b>Role Variables:</b> Variables defined within Ansible roles. They are stored in the defaults/main.yml file of a role. Role variables can be overridden by playbook variables or inventory variables.
4. <b>Fact Variables:</b> Facts are variables automatically gathered by Ansible about the target hosts. These variables provide information such as network interfaces, operating system details, or hardware information. Fact variables are accessible as hostvars and ansible_facts within playbooks.
5. <b>Registered Variables:</b> Variables created by registering the output of a task to a variable. These variables store task results and can be used later in the playbook.
6. <b>Environment Variables:</b> Environment variables available on the control machine where Ansible is executed. They can be accessed using the env dictionary.
7. <b>Magic Variables:</b> Ansible provides a set of predefined variables known as "magic" variables that hold useful information during playbook execution. Examples include inventory_hostname, ansible_host, ansible_user, etc.

You can use these variables to store values such as IP addresses, file paths, usernames, passwords, or any other data required in your playbook. Variables can be accessed and manipulated using Jinja2 templating syntax ({{ variable_name }}) throughout your playbook.

Here's an example of defining and using a variable in an Ansible playbook:

```yaml
---
- name: Example Playbook with Variables
  hosts: all
  become: true
  vars:
    my_variable: "Hello, Ansible!"
  tasks:
    - name: Print Variable
      debug:
        msg: "{{ my_variable }}"
```

In this example, the variable my_variable is defined in the vars section of the playbook. The debug task then prints the value of the variable using the {{ my_variable }} syntax.

### Inventory variables 
In Ansible, inventory variables are used to assign specific values to hosts or groups defined in your inventory file (hosts). These variables allow you to customize the behavior of your playbooks based on host-specific or group-specific requirements. Here's how you can define and use inventory variables:

<b>Host Variables:</b> You can define variables specific to individual hosts. In your inventory file (hosts), add variables under the host's entry using the following format:

```
hostname ansible_host=192.168.1.100 ansible_user=admin
```

In this example, ansible_host and ansible_user are inventory variables assigned to the host hostname. You can define as many variables as needed for a particular host.

<b>Group Variables:</b> Variables can also be defined for groups of hosts. In your inventory file (hosts), create a group and define variables under the group's entry:

```
[webservers]
web1 ansible_host=192.168.1.101 ansible_user=webuser
web2 ansible_host=192.168.1.102 ansible_user=webuser

[webservers:vars]
http_port=80
```

In this example, the group [webservers] contains two hosts (web1 and web2), each with their own variables. Additionally, the [webservers:vars] section defines a variable http_port that is shared among all hosts within the webservers group.

<b>Variable Precedence:</b> Ansible follows a specific order of precedence when resolving variables: host-specific variables override group variables, which in turn override playbook variables.
```
Host Variables > Group Variables > Playbook Variables
```
This allows you to define variables at different levels and have more specific values take precedence over more general ones.

To use inventory variables in your playbooks, you can reference them using Jinja2 templating syntax ({{ variable_name }}). For example:

```yaml
---
- name: Example Playbook with Inventory Variables
  hosts: webservers
  become: true
  tasks:
    - name: Print Host Variables
      debug:
        msg: "Hostname: {{ inventory_hostname }}, IP: {{ ansible_host }}, User: {{ ansible_user }}"

    - name: Print Group Variable
      debug:
        msg: "HTTP Port: {{ http_port }}"
```

In this example, the playbook is executed on the webservers group. The debug tasks demonstrate how to access inventory variables. The inventory_hostname, ansible_host, ansible_user variables are host-specific, while the http_port variable is defined at the group level.

By utilizing inventory variables, you can define host-specific or group-specific values that are applied when running your playbooks. This provides flexibility and allows you to customize the behavior of your playbooks based on the hosts or groups in your inventory.

## Playbook variables
In Ansible, playbook variables are used to define data that can be accessed and utilized within a specific playbook. These variables allow you to make your playbook more dynamic and adaptable. Here's how you can define and use playbook variables:

<b>Play-Level Variables:</b> You can define variables at the play level using the vars keyword. These variables are accessible to all tasks within that play. Here's an example:

```yaml
---
- name: Example Playbook with Play-Level Variables
  hosts: all
  become: true
  vars:
    my_variable: "Hello, Ansible!"
  tasks:
    - name: Print Variable
      debug:
        msg: "{{ my_variable }}"
```

In this example, the variable my_variable is defined within the vars section of the playbook. It can be referenced using the {{ my_variable }} syntax within any task within that play.

<b>Task-Level Variables:</b> Variables can also be defined at the task level using the vars keyword. These variables are specific to a particular task. Here's an example:

```yaml
---
- name: Example Playbook with Task-Level Variables
  hosts: all
  become: true
  tasks:
    - name: Task 1
      debug:
        msg: "{{ my_variable }}"
      vars:
        my_variable: "Hello, Ansible!"
```

In this example, the variable my_variable is defined within the vars section of the task itself. It is accessible only within that specific task.

<b>Variable Precedence:</b> Ansible follows a specific order of precedence when resolving variables: playbook-level variables override inventory variables, and task-level variables override both playbook-level and inventory variables.

```
Task-Level Variables > Play-Level Variables > Inventory Variables
```
This allows you to define variables at different levels and have more specific values take precedence over more general ones.

<b>Variable Files:</b> You can store variables in separate YAML files and include them in your playbook using the vars_files keyword. This allows you to organize and reuse variables across multiple playbooks.

```yaml
---
- name: Example Playbook with Variable Files
  hosts: all
  become: true
  vars_files:
    - vars/my_variables.yml
  tasks:
    - name: Print Variable
      debug:
        msg: "{{ my_variable }}"
```

In this example, the <b>vars_files</b> keyword is used to include a YAML file (vars/my_variables.yml) containing variable definitions. The variables from the file can be accessed within the playbook.

Playbook variables enable you to define and utilize data within the scope of a specific playbook, making it more customizable and flexible. By leveraging variables, you can separate the data from the logic, promote reusability, and tailor the behavior of your playbooks based on the specific needs of your infrastructure or environment.

#### vars_prompt
In Ansible, the vars_prompt feature allows you to prompt the user for variable values during playbook execution. It enables interactive input and makes your playbooks more dynamic. Here's how you can use vars_prompt in Ansible:

```yaml
---
- name: Example Playbook with vars_prompt
  hosts: localhost
  gather_facts: false
  vars_prompt:
    - name: username
      prompt: "Enter your username"
      private: no
    - name: password
      prompt: "Enter your password"
      private: yes
  tasks:
    - name: Print Variables
      debug:
        msg:
          - "Username: {{ username }}"
          - "Password: {{ password }}"
```

In this example, the <b>vars_prompt</b> section is defined at the play level. It prompts the user for two variable values: username and password. The name parameter specifies the variable name, while the prompt parameter provides the message displayed to the user. The private parameter determines whether the user input for the variable should be hidden (in the case of sensitive information like passwords).

After the user provides the variable values, the playbook proceeds to the tasks section. In this case, a single task is defined to print the values of the variables using the debug module.

When you execute the playbook, Ansible will prompt the user to enter the values for the username and password variables. The entered values will be stored in the respective variables, which can then be used throughout the playbook.

Note that vars_prompt works only when executing the playbook interactively. If you run Ansible in non-interactive mode or through automation scripts, the prompt will not be displayed, and the variables will need to be provided through other means, such as inventory files, command-line arguments, or variable files.

Using vars_prompt, you can obtain user input during playbook execution, allowing for more flexible and interactive behavior in your Ansible playbooks.
## Roles variables
In Ansible, role variables are used to define data specific to a role. Role variables allow you to create reusable and modular playbooks by separating the variable definitions from the playbook logic. Here's how you can work with role variables in Ansible:

Role Directory Structure: Ansible follows a specific directory structure for roles. Within the role directory, you typically have a defaults directory where you can define default variables for the role. The structure looks like this:

```
my_role/
├── defaults/
│   └── main.yml
├── tasks/
├── handlers/
├── files/
├── templates/
└── meta/
```

<b>Defining Role Variables:</b> In the defaults/main.yml file of the role, you can define the default values for the role variables. These variables will be used if no other value is provided when using the role. For example:

```yaml
# defaults/main.yml
---
my_variable: default_value
another_variable: another_default_value
```

In this example, the role has two variables: my_variable and another_variable, with their default values defined.

<b>Overriding Role Variables:</b> When using the role, you can override the default variable values by providing new values in your playbook or in the inventory. The overriding values take precedence over the default values defined in the role. Here's an example:

```yaml
---
- name: Example Playbook using Role
  hosts: all
  become: true
  roles:
    - my_role
  vars:
    my_variable: overridden_value
```

In this example, the playbook uses the my_role role, and the my_variable variable is overridden with the value overridden_value at the playbook level.

<b>Accessing Role Variables:</b> Inside the role, you can access the role variables using the {{ variable_name }} syntax. For example, in a task within the role:

```yaml
---
- name: Example Task using Role Variables
  debug:
    msg: "Value of my_variable: {{ my_variable }}"
```

This task prints the value of the my_variable role variable.

Role variables provide a way to define and manage data specific to a role, making your playbooks more modular and reusable. By separating the variable definitions from the playbook logic, you can easily customize the behavior of the role by overriding the variable values when using the role in your playbooks.

## Facts variables
In Ansible, fact variables are automatically gathered by the system during playbook execution. These variables provide information about the target hosts, such as hardware details, operating system facts, network interfaces, and more. They allow you to access and utilize system-specific information within your playbooks. Here's how you can work with fact variables in Ansible:

Accessing Fact Variables: Ansible gathers fact variables automatically during playbook execution. These variables are accessible using the ansible_facts dictionary. For example, to access the ansible_facts variable containing all the gathered facts:

```yaml
---
- name: Example Playbook with Fact Variables
  hosts: all
  become: true
  tasks:
    - name: Print Facts
      debug:
        var: ansible_facts
```

In this example, the ansible_facts variable is printed using the debug module. It contains all the gathered facts for the target host(s).

<b>Accessing Specific Fact Variables:</b> You can access specific fact variables within the ansible_facts dictionary. For example, to access the operating system information:

```yaml
---
- name: Example Playbook with Fact Variables
  hosts: all
  become: true
  tasks:
    - name: Print OS
      debug:
        var: ansible_facts['ansible_distribution']
```

In this example, the ansible_distribution fact variable, which provides the distribution name of the operating system, is printed.

<b>Using Fact Variables in Conditionals:</b> Fact variables can be used in conditional statements to perform tasks based on specific system properties. For example:

```yaml
---
- name: Example Playbook with Fact Variables
  hosts: all
  become: true
  tasks:
    - name: Print Message if Ubuntu
      debug:
        msg: "This is an Ubuntu system"
      when: ansible_facts['ansible_distribution'] == 'Ubuntu'
```

In this example, the playbook prints a message if the target host is running Ubuntu, based on the value of the ansible_distribution fact variable.

<b>Customizing Fact Gathering:</b> By default, Ansible gathers facts automatically. However, you can control the fact gathering behavior using the gather_facts parameter at the play or task level. For example:

```yaml
---
- name: Example Playbook with Fact Gathering
  hosts: all
  become: true
  gather_facts: false  # Disable fact gathering for this play
  tasks:
    - name: Print Facts
      debug:
        var: ansible_facts
      gather_facts: true  # Enable fact gathering for this task
```

In this example, fact gathering is disabled for the play but explicitly enabled for the specific task.

Fact variables provide a wealth of information about the target hosts, enabling you to make informed decisions and perform specific tasks based on system properties. They are accessible through the ansible_facts dictionary and can be used in conditionals, printed for debugging purposes, or utilized in various other ways to make your Ansible playbooks more powerful and adaptable.
