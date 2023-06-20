## Command Module
The command module in Ansible is used to execute commands on the target hosts. It is one of the core modules available in Ansible and allows you to run commands directly on the remote hosts.

Here's an example of using the command module in an Ansible playbook:

```yaml

- name: Execute Command
  hosts: localhost
  gather_facts: false

  tasks:
    - name: Run a command
      command: echo "Hello, Ansible!"

    - name: Store command output in a variable
      command: ls /tmp
      register: ls_output

    - name: Display command output
      debug:
        var: ls_output.stdout_lines
```

In this playbook:

1. The hosts field specifies the target hosts or host group on which the playbook will be executed (web_servers in this example).
2. The gather_facts field is set to false to disable fact gathering from the target hosts. If you need to gather facts, you can set it to true.
3. The first task uses the command module to run the echo command on the target hosts, printing "Hello, Ansible!" to the console.
4. The second task runs the ls /tmp command on the target hosts and registers the output in the ls_output variable using the register keyword.
5. The third task uses the debug module to display the command output stored in the ls_output variable.

When you run this playbook, it will execute the specified commands on the target hosts and display the command outputs as defined in the tasks.

Note that the command module is often used for simple command execution. If you need to execute commands that require a shell environment or complex scripting, you may consider using the shell or script modules, respectively.
