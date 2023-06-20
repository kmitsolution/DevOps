## Conditional Statements
In Ansible, conditional statements allow you to control the flow of your playbook based on certain conditions. You can use conditional statements to perform tasks conditionally, skip tasks, include or exclude certain hosts, and more. Ansible provides several ways to express conditional logic. Here are some examples:

<b>when Statement:</b> The when statement is the most commonly used conditional statement in Ansible. It allows you to execute a task based on a specified condition. Here's an example:

```yaml
---
- name: Example Playbook with Conditional Task
  hosts: all
  become: true
  tasks:
    - name: Task 1
      command: echo "Hello, Ansible!"
      when: ansible_os_family == "Debian"
```

In this example, the task named "Task 1" will only be executed if the ansible_os_family fact is equal to "Debian". Otherwise, it will be skipped.

<b>failed_when Statement:</b> The failed_when statement allows you to mark a task as failed based on a condition. It is often used to check the result of a command or module and determine whether it should be considered a failure. Here's an example:

```yaml
---
- name: Example Playbook with Failed When
  hosts: all
  become: true
  tasks:
    - name: Task 1
      command: some_command
      register: result
      failed_when: "'error' in result.stdout"
```

In this example, if the string "error" is found in the stdout of the some_command task, the task will be marked as failed.

<b>Conditional Blocks:</b> Ansible also provides the ability to use conditional blocks, which allow you to group tasks and apply conditions to the entire block. There are three types of conditional blocks: when, failed_when, and changed_when. Here's an example:

```yaml
---
- name: Example Playbook with Conditional Block
  hosts: all
  become: true
  tasks:
    - name: Task 1
      command: some_command
      register: result

    - name: Task 2
      command: another_command
      when: result.stdout != ""

    - name: Task 3
      command: third_command
      changed_when: false
      failed_when: "'error' in result.stdout"

```
In this example, "Task 2" will only be executed if the stdout of "Task 1" is not empty. "Task 3" will always be marked as "changed=false" and will fail if the string "error" is found in the stdout of "Task 1".

Conditional statements provide flexibility in controlling the flow of your playbook based on conditions. They allow you to make decisions, skip tasks, and perform specific actions based on the state of the target hosts. You can use the when, failed_when, and conditional blocks to implement conditional logic in your Ansible playbooks.

### Logical Operators
In Ansible, you can use logical operators to combine multiple conditions in conditional statements. The logical operators and, or, and not can be used to create complex conditional expressions. Here are some examples of using logical operators in conditional statements:

Using and operator:

```yaml
- name: Example Playbook with Logical AND
  hosts: all
  become: true
  tasks:
    - name: Task 1
      command: echo "Both conditions are true"
      when: ansible_os_family == "Debian" and ansible_distribution_version == "10"
```

In this example, the task named "Task 1" will only be executed if both conditions ansible_os_family equals "Debian" and ansible_distribution_version equals "10" are true.

### Using or operator:

```yaml
- name: Example Playbook with Logical OR
  hosts: all
  become: true
  tasks:
    - name: Task 1
      command: echo "At least one condition is true"
      when: ansible_os_family == "Debian" or ansible_os_family == "RedHat"
```

In this example, the task named "Task 1" will be executed if either condition ansible_os_family equals "Debian" or ansible_os_family equals "RedHat" is true.

### Using not operator:

```yaml
- name: Example Playbook with Logical NOT
  hosts: all
  become: true
  tasks:
    - name: Task 1
      command: echo "The condition is false"
      when: not (ansible_distribution == "Ubuntu")
```

In this example, the task named "Task 1" will be executed if the condition ansible_distribution is not equal to "Ubuntu".

You can combine these logical operators with comparison operators (==, !=, <, >, etc.) and other conditional expressions to create more complex conditions. Remember to enclose complex conditions in parentheses to ensure correct evaluation.

Using logical operators in conditional statements allows you to express more advanced conditions and make decisions based on multiple factors in your Ansible playbooks.
