## ansible.builtin.debug module – Print statements during execution

The debug module in Ansible is used to print debug messages during playbook execution. It allows you to display variable values, task results, or any other information you need for troubleshooting or understanding the playbook's behavior. Here's an example of how to use the debug module:
```yaml
- name: Display variable value
  debug:
    var: my_variable

- name: Display task result
  debug:
    msg: "The task was successful"
```

In the first example, the debug module is used to print the value of the variable my_variable. The var parameter specifies the variable name.

In the second example, the debug module is used to display a custom message. The msg parameter contains the message to be printed.

You can also display the values of multiple variables or combine them with custom messages. Here's an example:

```yaml

- name: Display multiple values
  debug:
    var:
      - var1
      - var2
    msg: "The values are {{ var1 }} and {{ var2 }}"
```

In this case, the debug module will display the values of var1 and var2, along with a custom message.

Certainly! Here's an example of using the debug module in an Ansible playbook:

```yaml

- name: Debug Example
  hosts: localhost
  gather_facts: false

  tasks:
    - name: Set a variable
      set_fact:
        my_variable: "Hello, Ansible!"

    - name: Display variable value
      debug:
        var: my_variable

    - name: Perform arithmetic operation
      vars:
        number1: 10
        number2: 5
      debug:
        msg: "The sum is {{ number1 + number2 }}"
```

In this playbook:

1. The <b>set_fact</b> task sets the value of my_variable to "Hello, Ansible!" using the set_fact module.
2. The <b>debug</b> task is then used to display the value of my_variable using the var parameter.
3. Another debug task demonstrates performing an arithmetic operation (number1 + number2) and displaying the result using the msg parameter.

When you run this playbook, it will print the value of my_variable and the result of the arithmetic operation on the console.

Note: The gather_facts: false line is included in this example to disable fact gathering from the remote hosts. Since this example is using localhost as the target, there is no need to gather facts.

