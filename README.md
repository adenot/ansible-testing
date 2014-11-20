ansible-testing
===============

ansible-testing is designed to be a set of modules for Ansible to allow 
infrastructure testing. 

Hopefully these modules will be absorbed into [Ansible](http://github.com/ansible/ansible)
if the concept proves itself

## Installation

```
cd ~/src/ansible
git submodule add http://github.com/adenot/ansible-testing library/testing
```

## Usage
```
- hosts: testinstance

  roles:
  - base
  - webserver
```

# Implementation
```
---
 - name: Tests assert_* modules
   hosts: all
   gather_facts: false
   vars:
       testvar: 2
   tasks:
     - name: 'test file mode 0755'
       assert_mode: name=/tmp/testfile should_be=0755 

     - name: 'test file owner'
       assert_owner: name=/tmp/testfile should_be=adenot
       
     - name: 'test file group'
       assert_group: name=/tmp/testfile should_be=users

     - name: 'test file exists'
       assert_file: name=/tmp/testfile should_be=file
       
     - name: 'test link exists'
       assert_file: name=/tmp/testlink should_be=link to=/tmp/testfile
       
     - name: 'test directory exists'
       assert_file: name=/tmp/testdir should_be=directory
       
     - name: 'test process running httpd'
       assert_process: name=/sbin/agetty should_be=running with_args='38400'
       
     - name: 'test open port 80'
       assert_tcp: name=localhost:9001 should_be=open with_timeout=3
       
     - name: 'test file content'
       assert_content: name=/tmp/testfile should_match="example.*string"
          
     - name: 'test variables'
       assert_equals: value={{testvar}} should_be=1

       # For more generic testing (until it is part of the module)
     - name: 'test command return code is zero'
       assert_command: name='netstat -nl|grep ":9001"' should_be=successful
```
