## Import Playbook using include module
In Ansible, you can include one playbook within another playbook using the include or import_playbook directives. Here's an example of how to include a playbook within another playbook:

Let's say you have two playbooks: main_playbook.yml and included_playbook.yml.

### main_playbook.yml:

```yaml
---
- name: Main Playbook
  hosts: all
  become: true
  tasks:
    - name: Include another playbook
      include: included_playbook.yml
```

### included_playbook.yml:

```yaml
- name: Task 1
  debug:
        msg: "This is Task 1 in the included playbook"

- name: Task 2
  debug:
        msg: "This is Task 2 in the included playbook"

```

In this example, the main_playbook.yml includes the included_playbook.yml using the include directive. When you run the main_playbook.yml, it will execute the tasks defined in the included_playbook.yml as part of the main playbook's execution.

To run the main_playbook.yml, you can use the ansible-playbook command:

```
ansible-playbook main_playbook.yml
```

This will execute both the main playbook and the included playbook tasks.

Note that you can also use the import_playbook directive instead of include if you want to include the playbook statically during playbook parsing. The usage is similar, but import_playbook provides more features like variable scoping and conditional inclusion.
## import_playbook


Let's say you have two playbooks: main_playbook.yml and imported_playbook.yml.

### main_playbook.yml:

```yaml
---
- name: Main Playbook
  hosts: all
  become: true
  tasks:
    - name: Import another playbook
      import_playbook: imported_playbook.yml
```

### imported_playbook.yml:

```yaml
- name: Task 1
  debug:
        msg: "This is Task 1 in the included playbook"

- name: Task 2
  debug:
        msg: "This is Task 2 in the included playbook"
```

In this example, the main_playbook.yml imports the imported_playbook.yml using the import_playbook directive. When you run the main_playbook.yml, it will import and execute the tasks defined in the imported_playbook.yml as part of the main playbook's execution.

To run the main_playbook.yml, you can use the ansible-playbook command:

```
ansible-playbook main_playbook.yml
```

This will execute both the main playbook and the imported playbook tasks.

One important difference between include and import_playbook is that import_playbook provides more advanced features like variable scoping and conditional inclusion. It also allows the imported playbook to act as a stand-alone playbook when needed.

Additionally, import_playbook allows you to specify a name for the imported playbook. This name can be useful when dealing with multiple imports within a playbook. Here's an example:

```yaml

- name: Import another playbook with a name
  import_playbook: imported_playbook.yml
  name: Custom Name
```

Using the name attribute, you can give a custom name to the imported playbook, which will be shown in the playbook execution output.

These features make import_playbook more versatile when working with complex playbooks and playbook organization.
