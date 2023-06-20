## notify directive
notify directive to trigger a handler in Ansible:

Let's assume you have a playbook main_playbook.yml and a handler file handlers/main_handler.yml.

#### main_playbook.yml:

```yaml
---
- name: Example Playbook with Notify
  hosts: all
  become: true
  tasks:
    - name: Task 1
      debug:
        msg: "This is Task 1"
      notify: Run Handler
```

#### handlers/main_handler.yml:

```yaml
---
- name: Run Handler
  debug:
    msg: "This is the handler task"
```

In this example, the main_playbook.yml consists of a task called Task 1 that prints a debug message. The notify directive is added to the task, specifying the name of the handler to trigger (Run Handler).

When you run the main_playbook.yml using ansible-playbook, the Task 1 task will execute, and at the end of the task, the notify directive will trigger the associated handler.

To execute the playbook and trigger the handler, use the following command:

```
ansible-playbook main_playbook.yml
```

Upon execution, you will see the debug message from the handler task This is the handler task printed in the output.

The notify directive is used to associate a task with a handler. When a task with a notify directive completes, it triggers the specified handler, allowing you to perform additional actions or tasks based on certain conditions or events.

Handlers are defined in separate files under the handlers directory within your Ansible project. They are typically used to restart services, reload configurations, or perform other actions that respond to changes made during the playbook execution.

Note: Handlers are only triggered once during a playbook run, even if multiple tasks notify the same handler. Ansible ensures that handlers are executed efficiently and avoids duplicate execution.
