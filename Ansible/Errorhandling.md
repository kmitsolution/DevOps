## Error Handling in Ansible
### Ignoring failed commands
By default Ansible stops executing tasks on a host when a task fails on that host. You can use ignore_errors to continue on in spite of the failure.

```
- name: Do not count this as a failure
  ansible.builtin.command: /bin/false
  ignore_errors: true
```
The ignore_errors directive only works when the task is able to run and returns a value of ‘failed’. It does not make Ansible ignore undefined variable errors, connection failures, execution issues (for example, missing packages), or syntax errors

### Ignoring unreachable host errors

You can ignore a task failure due to the host instance being ‘UNREACHABLE’ with the ignore_unreachable keyword. Ansible ignores the task errors, but continues to execute future tasks against the unreachable host. For example, at the task level:

```
- name: This executes, fails, and the failure is ignored
  ansible.builtin.command: /bin/true
  ignore_unreachable: true
```
```
- name: This executes, fails, and ends the play for this host
  ansible.builtin.command: /bin/true
```

```
- hosts: all
  ignore_unreachable: true
  tasks:
  - name: This executes, fails, and the failure is ignored
    ansible.builtin.command: /bin/true

  - name: This executes, fails, and ends the play for this host
    ansible.builtin.command: /bin/true
    ignore_unreachable: false
```

## Defining failure
Ansible lets you define what “failure” means in each task using the failed_when conditional. As with all conditionals in Ansible, lists of multiple failed_when conditions are joined with an implicit and, meaning the task only fails when all conditions are met. If you want to trigger a failure when any of the conditions is met, you must define the conditions in a string with an explicit or operator.

You may check for failure by searching for a word or phrase in the output of a command
```
- name: Fail task when the command error output prints FAILED
  ansible.builtin.command: /usr/bin/example-command -x -y -z
  register: command_result
  failed_when: "'FAILED' in command_result.stderr"
```
or based on the return code
```
- name: Fail task when both files are identical
  ansible.builtin.raw: diff foo/file1 bar/file2
  register: diff_cmd
  failed_when: diff_cmd.rc == 0 or diff_cmd.rc >= 2
```
You can also combine multiple conditions for failure. This task will fail if both conditions are true:

```
- name: Check if a file exists in temp and fail task if it does
  ansible.builtin.command: ls /tmp/this_should_not_be_here
  register: result
  failed_when:
    - result.rc == 0
    - '"No such" not in result.stdout'
```
If you have too many conditions to fit neatly into one line, you can split it into a multi-line YAML value with >.

```
- name: example of many failed_when conditions with OR
  ansible.builtin.shell: "./myBinary"
  register: ret
  failed_when: >
    ("No such file or directory" in ret.stdout) or
    (ret.stderr != '') or
    (ret.rc == 10)
```

