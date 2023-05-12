# Lab Guide: Installing IIS on a Windows Host using Ansible and Updating a Web.config file using a Template

In this lab guide, we will use Ansible to install IIS on a Windows host and update a Web.config file using a template. We will also add a database connection string to the MySQL server on Ubuntu that we created in the previous lab guide. The template will allow for updating of the database server IP address, database name, and other necessary database connection details.

## Prerequisites

- A Windows host with Ansible installed.
- A Ubuntu server with MySQL installed (from the previous lab guide).
- Access to a GitHub repository at https://github.com/ProDataMan/ansible-best-practices-windows.git.

## Step 1: Install IIS using Ansible

1. Clone the Ansible repository from https://github.com/ProDataMan/ansible-best-practices-windows.git to your local machine.
2. Open the `ansible-best-practices-windows/playbooks/iis.yml` file in a text editor.
3. Add the following lines to the end of the file:

```
- name: Install IIS
  win_feature:
    name: Web-Server
    state: present
```

4. Save the file and close the text editor.
5. Open a command prompt on your Windows host and navigate to the `ansible-best-practices-windows` directory.
6. Run the following command to install IIS using Ansible:

```
ansible-playbook playbooks/iis.yml
```

7. Ansible will install IIS on the Windows host.

## Step 2: Update the Web.config file using a template

1. Create a new file called `web.config.j2` in the `ansible-best-practices-windows/templates` directory.
2. Open the `web.config.j2` file in a text editor.
3. Add the following lines to the file:

```
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <connectionStrings>
    <add name="MySqlConnection" providerName="MySql.Data.MySqlClient" connectionString="server={{ db_server_ip }};database={{ db_name }};uid={{ db_username }};password={{ db_password }}" />
  </connectionStrings>
</configuration>
```

4. Save the file and close the text editor.
5. Open the `ansible-best-practices-windows/playbooks/iis.yml` file in a text editor.
6. Add the following lines to the end of the file:

```
- name: Update Web.config file
  win_template:
    src: templates/web.config.j2
    dest: C:\inetpub\wwwroot\web.config
    vars:
      db_server_ip: <MySQL server IP address>
      db_name: <database name>
      db_username: <database username>
      db_password: <database password>
```

7. Replace `<MySQL server IP address>`, `<database name>`, `<database username>`, and `<database password>` with the appropriate values for your environment.
8. Save the file and close the text editor.
9. Run the following command to update the Web.config file using the template:

```
ansible-playbook playbooks/iis.yml
```

10. Ansible will update the Web.config file with the database connection string.

## Conclusion

In this lab guide, we used Ansible to install IIS on a Windows host and update a Web.config file using a template. We also added a database connection string to the MySQL server on Ubuntu that we created in the previous lab guide. The template allowed for updating of the database server IP address, database name, and other necessary database connection details.
