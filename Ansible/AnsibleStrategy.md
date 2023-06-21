# Ansible Strategy
In Ansible, strategies refer to different approaches for executing tasks across multiple hosts. By default, Ansible uses the "linear" strategy, where tasks are executed sequentially on each host. However, Ansible provides several other strategies that can be used to optimize performance and parallelize task execution. Here are some commonly used Ansible strategies:

1. ## Linear Strategy (default):
1. Ansible executes tasks in a linear order, one host at a time.
2. It is simple and easy to understand but may not be the most efficient for large-scale deployments.

2. ## Free Strategy:

1. Tasks are executed on all hosts concurrently without any restrictions.
2. Useful when tasks can run independently of each other and don't have any dependencies.
3. Can be faster than the linear strategy for large inventories but may consume more resources.

### Example
```yaml
- name: Execute tasks with the "free" strategy
  hosts: all
  strategy: free
  tasks:
    - name: Task 1
      command: echo "Executing Task 1 on {{ inventory_hostname }}"
      register: task1_result

    - name: Task 2
      command: echo "Executing Task 2 on {{ inventory_hostname }}"
      register: task2_result

    - name: Task 3
      command: echo "Executing Task 3 on {{ inventory_hostname }}"
      register: task3_result

    - name: Display task results
      debug:
       msg:  "Executed Task  on {{ inventory_hostname }}"

```
In this example, the playbook is targeting the hosts defined in the "mygroup" group. The strategy keyword is set to "free" to enable the free strategy. Inside the tasks section, three tasks (Task 1, Task 2, and Task 3) are defined.

Each task uses the command module to execute a simple shell command, echoing a message along with the inventory_hostname variable to indicate which task is being executed on which host.

3. ## Serial Strategy:

Allows you to specify a maximum number of hosts that Ansible can work on simultaneously.
Useful for limiting resource usage when working with a large number of hosts or when running tasks with potential resource contention.

### Example
```yaml
- name: Execute tasks with the "free" strategy
  hosts: all
  serial:
    - 2  # Maximum number of hosts to process concurrently
  tasks:
    - name: Task 1
      command: echo "Executing Task 1 on {{ inventory_hostname }}"
      register: task1_result

    - name: Task 2
      command: echo "Executing Task 2 on {{ inventory_hostname }}"
      register: task2_result

    - name: Task 3
      command: echo "Executing Task 3 on {{ inventory_hostname }}"
      register: task3_result

    - name: Display task results
      debug:
       msg:  "Executed Task  on {{ inventory_hostname }}"
```
### Delegation Strategy:

In this strategy, tasks are delegated to a single host, and that host performs the work on behalf of others.
Useful when executing tasks that require a central node to coordinate actions across multiple hosts.
```yaml
- name: Execute tasks with the "free" strategy
  hosts: all
  tasks:
    - name: Task 1
      command: touch /tmp/10.txt
      delegate_to: localhost

    - name: Task 2
      command: echo "Executing Task 2 on {{ inventory_hostname }}"
      register: task2_result

    - name: Task 3
      command: echo "Executing Task 3 on {{ inventory_hostname }}"
      register: task3_result

    - name: Display task results
      debug:
       msg:  "Executed Task  on {{ inventory_hostname }}"

```
