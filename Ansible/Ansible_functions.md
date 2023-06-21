## Ansible functions
Ansible provides a wide range of built-in functions that you can use in your playbooks and templates to perform various operations and manipulate data. These functions allow you to work with variables, strings, lists, dictionaries, and more. Here are some commonly used Ansible functions:

### Variable Manipulation Functions:

Ansible provides several variable manipulation functions that allow you to modify and manipulate variables in your playbooks. Here are some commonly used variable manipulation functions:

1. <b>default:</b>

<b>Usage:</b> default(value, default_value)

<b>Description:</b> Sets a default value for a variable if it is undefined or empty.

<b>Example:</b>

```yaml
- name: Example using default function
  hosts: localhost
  vars:
          my_variable: 'Test'
  tasks:
    - name: Set default value for variable
      set_fact:
        my_variable: "{{ some_variable | default('default_value') }}"
    - name: Print value of my_variable
      debug:
              msg: "{{ my_variable }}"

```

2. <b>mandatory:</b>

#### Usage: mandatory(value)
#### Description: Raises an error if a variable is undefined or empty.
### Example:
```yaml
- name: Example using mandatory function
  hosts: all
  tasks:
    - name: Raise error if variable is not defined
      assert:
        that: some_variable | mandatory
        fail_msg: "Variable 'some_variable' is mandatory but not defined."
```

###  <b>combine:</b>
#### Usage: combine(dict1, dict2, ...)
#### Description: Combines two or more dictionaries into a single dictionary.
### Example:
```yaml
- name: Example using combine function
  hosts: localhost
  vars:
    dict1:
      key1: value1
    dict2:
      key2: value2
  tasks:
    - name: Combine dictionaries
      set_fact:
        combined_dict: "{{ dict1 | combine(dict2) }}"
    - name: Print dicitionary
      debug:
              msg: "{{ combined_dict }}"

```

### difference:

#### Usage: difference(list1, list2)
#### Description: Returns the difference between two lists or sets.
### Example:
```yaml
- name: Example using difference function
  hosts: all
  vars:
    list1: [1, 2, 3, 4]
    list2: [3, 4, 5, 6]
  tasks:
    - name: Find difference between lists
      set_fact:
        diff_list: "{{ list1 | difference(list2) }}"
```

### intersect

#### Usage: intersect(list1, list2)
#### Description: Returns the intersection of two lists or sets.
### Example:
```yaml
- name: Example using intersect function
  hosts: all
  vars:
    list1: [1, 2, 3, 4]
    list2: [3, 4, 5, 6]
  tasks:
    - name: Find intersection of lists
      set_fact:
        common_elements: "{{ list1 | intersect(list2) }}"
```

### union

#### Usage: union(list1, list2)
#### Description: Returns the union of two lists or sets.
### Example:
```yaml
- name: Example using union function
  hosts: all
  vars:
    list1: [1, 2, 3]
    list2: [3, 4, 5]
  tasks:
    - name: Find union of lists
      set_fact:
        merged_list: "{{ list1 | union(list2) }}"
```

These are just a few examples of variable manipulation functions in Ansible. They allow you to set default values, combine dictionaries, find differences and intersections in lists, and perform other operations on variables to manipulate and transform data within your playbooks.

## String Functions:

Ansible provides various string functions that allow you to manipulate and transform strings within your playbooks. Here are some commonly used string functions in Ansible:

### length:

1. Usage: {{ my_string | length }}
2. Description: Returns the length of a string.
### Example:
```yaml
- name: Example using difference function
  hosts: localhost
  tasks:
    - name: Reading the file
      command: cat /tmp/string.txt
      register: output
    - name: Print number of characters
      debug:
              msg:
              - "{{ output.stdout | length }}"

```

2. <b>upper:</b>

1. Usage: {{ my_string | upper }}
2. Description: Converts a string to uppercase.
### Example:
```yaml
- name: Example using difference function
  hosts: localhost
  tasks:
    - name: Reading the file
      command: cat /tmp/string.txt
      register: output
    - name: Print content in upper case
      debug:
              msg:
              - "{{ output.stdout | upper }}"
```

3. <b>lower:</b>

1. Usage: {{ my_string | lower }}
2. Description: Converts a string to lowercase.
### Example:
```yaml
- name: Example using difference function
  hosts: localhost
  tasks:
    - name: Reading the file
      command: cat /tmp/string.txt
      register: output
    - name: Print content in lowercase
      debug:
              msg:
              - "{{ output.stdout | upper }}"```

4. <b>replace:</b>

1. Usage: {{ my_string | replace('old', 'new') }}
2. Description: Replaces occurrences of a substring in a string with another substring.
### Example:
```yaml
- name: Example using difference function
  hosts: localhost
  tasks:
    - name: Reading the file
      command: cat /tmp/string.txt
      register: output
    - name: Print number of characters
      debug:
              msg:
              - "{{ output.stdout | upper | lower | replace('is','was') | upper}}"
```

5. <b>split:</b>

1. Usage: {{ my_string | split(separator) }}
2. Description: Splits a string into a list based on a delimiter.
### Example:
```yaml
- name: Example using difference function
  hosts: localhost
  tasks:
    - name: Reading the file
      command: cat /tmp/string.txt
      register: output
    - name: Print number of characters
      debug:
              msg:
              - "{{ output.stdout | split(' ')}}"
```

These are just a few examples of the string functions available in Ansible. They allow you to perform operations such as getting the length of a string, converting case, replacing substrings, using regular expressions, and splitting strings. Refer to the Ansible documentation for a complete list of string functions and their usage: Ansible Filters

## List Functions:

1. length: Returns the length of a list.
2. join: Joins elements of a list into a string with a specified separator.
3. map: Applies a filter or transformation to each element of a list.
4. select: Filters a list based on a condition.

## Dictionary Functions:

1. keys: Returns a list of keys in a dictionary.
2. values: Returns a list of values in a dictionary.
3. items: Returns a list of key-value pairs in a dictionary.
4. combine: Combines two or more dictionaries into a single dictionary.
5. dict2items: Converts a dictionary to a list of key-value pairs.

Mathematical Functions:

abs: Returns the absolute value of a number.
random: Generates a random number.
min: Returns the minimum value from a list of numbers.
max: Returns the maximum value from a list of numbers.

These are just a few examples of the many functions available in Ansible. You can find a comprehensive list of Ansible functions and their usage in the official Ansible documentation: https://docs.ansible.com/ansible/latest/user_guide/playbooks_filters.html

Using these functions, you can perform various operations and manipulate data within your Ansible playbooks, making them more dynamic and flexible.
