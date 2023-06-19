## Ansible Environment
In Ansible, you can set environment variables for tasks using the environment keyword. This allows you to define specific environment variables that will be available during the execution of tasks on the target hosts.

Here's an example of setting environment variables in an Ansible playbook:

```yaml

- name: Set Environment Variables
  hosts: web_servers
  gather_facts: false

  tasks:
    - name: Set custom environment variables
      environment:
        MY_VARIABLE: "Hello"
        ANOTHER_VARIABLE: "World"

      debug:
        msg: "My variable is {{ MY_VARIABLE }} and another variable is {{ ANOTHER_VARIABLE }}"
```

In this playbook:

The <b>environment</b> keyword is used within tasks to define environment variables specific to that task.

In the task, two custom environment variables (MY_VARIABLE and ANOTHER_VARIABLE) are set using the environment keyword. The debug module is used to print the values of these variables.

When you run this playbook on the target hosts (web_servers group), it will set the specified environment variables for the respective tasks and display the values accordingly.

Note: If you want to set environment variables that are available to all tasks in the playbook, you can define them at the play-level using the vars keyword.
