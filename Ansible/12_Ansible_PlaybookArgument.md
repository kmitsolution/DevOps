## Ansible playbook with arguments:

```yaml
---
- name: Example Playbook with Arguments
  hosts: all
  become: true
  vars:
    my_var: "{{ my_arg }}"
  tasks:
    - name: Task 1
      debug:
        msg: "My argument value is {{ my_arg }}"

    - name: Task 2
      debug:
        msg: "My variable value is {{ my_var }}"
```

In this example playbook, we define a variable called my_var and set its value to the value of the my_arg argument. The my_arg argument is passed to the playbook when executing it.

To execute this playbook with arguments, you can use the ansible-playbook command with the --extra-vars or -e option:

```
ansible-playbook my_playbook.yml --extra-vars "my_arg=example_value"
```

Replace my_playbook.yml with the filename of your playbook. The <b>--extra-vars</b> option allows you to pass key-value pairs as arguments to the playbook. In this case, we are passing my_arg=example_value as an argument.

When you run the playbook with the provided command, it will execute the tasks and print debug messages showing the values of both the argument and the variable.

Note: In this example, we assume you are running Ansible 2.10 or later, as the use of the vars keyword at the playbook level requires Ansible 2.10+.
