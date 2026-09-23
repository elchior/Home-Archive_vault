---
tags: [state/closed/2022/12]
created: 2026-09-15T13:59:51.000+02:00
modified: 2026-09-23T16:32:20.477+02:00
---

# T2022-1230-0826 Ansible installation issue
#state/closed/2022/12
## Description
### Issue
#### Error
```bash
(base) me@hp-me:~/ansible$ ansible-inventory -i ./hosts --list -y
Traceback (most recent call last):
  File "/usr/bin/ansible-inventory", line 34, in <module>
    from ansible import context
ModuleNotFoundError: No module named 'ansible'
```

Running the above command as root or with sudo works
```bash
(base) me@hp-me:~/ansible$ sudo ansible-inventory -i ./hosts --list -y
all:
  children:
    ungrouped: {}
    vhost:
      hosts:
        seven:
          ansible_host: 10.1.1.15
          ansible_python_interpreter: /usr/bin/python3
        six:
          ansible_host: 10.1.1.143
          ansible_python_interpreter: /usr/bin/python3
```

#### Solution
Install the ansible module with pip
```bash
pip install ansible
```

I still had the following error from the installation command abover
```bash
ERROR: pip's dependency resolver does not currently take into account all the packages that are installed. This behaviour is the source of the following dependency conflicts.
anaconda-project 0.9.1 requires ruamel-yaml, which is not installed.
sphinx 4.0.1 requires Jinja2<3.0,>=2.3, but you have jinja2 3.1.2 which is incompatible.
sphinx 4.0.1 requires MarkupSafe<2.0, but you have markupsafe 2.1.1 which is incompatible.
```
This seems to be a problem related to anaconda...

But the ansible commands seem to work fine without root now.

## Todo

## Done
### 2022-??
- Created Task
