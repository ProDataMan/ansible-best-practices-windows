# Lab Guide: Installing MySQL on Ubuntu using Ansible and Attaching an Existing Database File

In this lab, we will learn how to use Ansible to install MySQL on Ubuntu and attach an existing database file.

## Prerequisites
- Ubuntu installed on your machine.
- Ansible installed on your machine.
- Access to a MySQL server.

## Step 1: Create a playbook to install MySQL
1. Open a terminal on your Ubuntu machine.
2. Navigate to the directory where you want to create the playbook.
3. Create a new file with the extension `.yml` (e.g. `mysql_install.yml`).
4. Open the file in your favorite text editor.
5. Add the following code to the file:

```yaml
---
- name: Install MySQL
  hosts: localhost
  become: true
  tasks:
    - name: Install MySQL
      apt:
        name: mysql-server
        state: present
```

This code will instruct Ansible to install MySQL on the localhost.

## Step 2: Run the playbook to install MySQL
1. Save the `mysql_install.yml` file.
2. Open a terminal on your Ubuntu machine.
3. Navigate to the directory where you saved the playbook.
4. Run the following command to execute the playbook:

```
ansible-playbook mysql_install.yml
```

This command will run the playbook and install MySQL on your machine.

## Step 3: Attach an existing database file
1. Open a terminal on your Ubuntu machine.
2. Navigate to the directory where the existing database file is stored.
3. Use the following command to import the database file into MySQL:

```
mysql -u root -p < database_file.sql
```

This command will prompt you to enter the MySQL root password. Once you enter the password, the database file will be imported into MySQL.

## Conclusion
In this lab, we learned how to use Ansible to install MySQL on Ubuntu and attach an existing database file. Now you can use MySQL on your Ubuntu machine and work with an existing database.
