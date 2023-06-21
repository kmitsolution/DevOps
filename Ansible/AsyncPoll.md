## Asynchronous actions and polling

By default Ansible runs tasks synchronously, holding the connection to the remote node open until the action is completed. This means within a playbook, each task blocks the next task by default, meaning subsequent tasks will not run until the current task completes. This behavior can create challenges. For example, a task may take longer to complete than the SSH session allows for, causing a timeout. Or you may want a long-running process to execute in the background while you perform other tasks concurrently. Asynchronous mode lets you control how long-running tasks execute.
```
---

- hosts: localhost
  remote_user: root

  tasks:

  - name: Simulate long running op (15 sec), wait for up to 45 sec, poll every 5 sec
    ansible.builtin.command: /bin/sleep 15
    async: 45
    poll: 5
  - name: print
    debug:
       msg: "Finished"
```
```
---

- hosts: localhost
  remote_user: root

  tasks:

  - name: Simulate long running op (15 sec), wait for up to 45 sec, poll every 5 sec
    ansible.builtin.command: /bin/sleep 15
    async: 45
    poll: 0
  - name: print
    debug:
       msg: "Finished"
   ```

 # Example

```yaml
---
 - hosts: localhost
   tasks:
      - name: Run asynchronous task
        command: sleep 15
        async: 3600  # Maximum time to wait for task completion (in seconds)
        poll: 0     # Polling interval (in seconds)
        register: task_result

      - name: Wait for task to complete
        async_status:
         jid: "{{ task_result.ansible_job_id }}"
        register: async_result
        until: async_result.finished
        retries: 60  # Maximum number of retries
        delay: 10    # Delay between retries

      - name: Check task result
        debug:
         var: async_result
```    
In this example, the command module is used to execute a long-running command, and the async option is set to 3600 seconds (1 hour) to allow enough time for the task to complete. The poll option is set to 0 seconds, which means Ansible will run the task in background

The register keyword is used to capture the result of the command module in the task_result variable. Later, the async_status module is used to check the status of the asynchronous task using the jid (Job ID) obtained from task_result. The until keyword is used to specify the condition for waiting until the task is finished. In this case, async_result.finished is checked.

You can adjust the retries and delay values according to your requirements. Finally, the debug module is used to display the async_result variable, which will contain the status and other details of the asynchronous task.

Remember to replace long_running_command with the actual command you want to run asynchronously.
