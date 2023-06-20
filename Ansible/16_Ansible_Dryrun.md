## Ansible dry run
To perform a dry run or simulate the execution of an Ansible playbook without making any actual changes to the target systems, you can use the --check or --diff option. Here's how you can use these options:

<b>--check option:</b> The --check option is used to simulate the playbook execution without actually making any changes. It will display the changes that would be made if the playbook were run normally. Here's an example:

```yaml
---
 - name: play for dry run
   hosts: webservers
   tasks:
        - name: Copying testing.txt file to webservers
          copy: src=/tmp/testing.txt dest=/tmp/test.txt
```
```
ansible-playbook your_playbook.yml --check
```

### Dry run with check_mode option

Playbook:- multask.yaml
```yaml
---
 - name: play for dbservers multiple tasks
   hosts: dbservers
   vars_prompt:
      name: data
      prompt: Enter the value
   tasks:
         # Task1
        - name: create a file
          command: touch /tmp/myfile.txt
          check_mode: no
        # Task2
        - name: Add content to the file
          copy:
              content: "{{ data }}"
              dest: /tmp/myfile.txt
          check_mode: yes
         # Task 3
        - name: Read the content
          command: cat /tmp/myfile.txt
          register: output
          # Task 4
        - name: Print the content
          debug:
             var: output.stdout
