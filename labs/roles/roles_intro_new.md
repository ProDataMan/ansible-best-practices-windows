# Lab Guide: Using Ansible Roles to Create a Windows User with Permissions, Connect Ansible with that Account, Install IIS, Create a Web.config File, and Copy Web Files

In this lab, you will learn how to use Ansible roles to break down the tasks of creating a Windows user with permissions to install services, connecting Ansible with that account, installing IIS, creating a Web.config file using a template, and copying web files to the `c:\inetpub\wwwroot` folder. You will edit files using Visual Studio Code, commit and push changes to your GitHub repository named `ansible-working`, and use an SSH connection in PuTTY to pull down the files to the Ansible Control host before executing the Ansible playbook.

## Prerequisites

Before starting this lab, you should have the following:

- A Windows machine running with the latest updates installed.
- Ansible installed on your Ansible Control host.
- PuTTY installed on your Windows host to create an SSH connection to the Ansible host.
- Visual Studio Code installed on your Windows host.
- A GitHub account.

## Step 1: Creating a Windows User with Permissions

The first step is to create a Windows user with permissions to install services. You will use Ansible to automate this task.

1. Create a new Ansible role named `create-user` using the following command:

   ```
   ansible-galaxy role init create-user
   ```

2. Open the `tasks/main.yml` file in the `create-user` role and add the following code:

   ```
   - name: Grant user privileges
     win_command: 'net localgroup "Administrators" ansible /add'
     become: yes
     ignore_errors: yes
     register: result

   - name: Check if user is already a member of the group
     debug:
       msg: "User ansible is already a member of the Administrators group"
     when: "'The specified account name is already a member of the group.' in result.stderr"

   - name: Add user to the group if not already a member
     win_command: 'net localgroup "Administrators" ansible /add'
     become: yes
     when: "'The specified account name is already a member of the group.' not in result.stderr"
   ```

   This code creates a new user named `ansible` with the password `Password123`, adds the user to the `Administrators` group, grants the user remote desktop access, and grants the user administrator privileges.

3. Save the `main.yml` file and close it.

## Step 2: Connecting Ansible with the User Account

The next step is to connect Ansible with the `ansible` user account you just created.

1. Open the `ansible.cfg` file on your Ansible Control host and add the following lines:

   ```
   [defaults]
   INVENTORY = inventory_groups.yml
   remote_user = ansible
   ```

   This tells Ansible to connect to the remote Windows machine using the `ansible` user account.

2. Save the `ansible.cfg` file and close it.

## Step 3: Installing IIS

The next step is to install IIS on the remote Windows machine using Ansible.

1. Create a new Ansible role named `install-iis` using the following command:

   ```
   ansible-galaxy role init install-iis
   ```

2. Open the `tasks/main.yml` file in the `install-iis` role and add the following code:

   ```
   - name: Install IIS
     win_feature:
       name: Web-Server
       state: present
       include_management_tools: yes
   ```

   This code installs IIS on the remote Windows machine with the management tools included.

3. Save the `main.yml` file and close it.

## Step 4: Creating a Web.config File Using a Template

The next step is to create a Web.config file using a template.

1. Create a new Ansible role named `create-web-config` using the following command:

   ```
   ansible-galaxy role init create-web-config
   ```

2. In the `templates` folder inside the `create-web-config` role folder.

3. Create a new file named `web.config.j2` and add the following code:

   ```
   <?xml version="1.0" encoding="UTF-8"?>
   <configuration>
     <appSettings>
       <add key="sqlConnectionString" value="Server={{ ip_address }};Database={{ database_name }};User ID={{ db_username }};Password={{ db_password }};"/>
     </appSettings>
   </configuration>
   ```

   This is a sample Web.config file that will be used to configure IIS.

4. Open the `tasks/main.yml` file in the `create-web-config` role and add the following code:

   ```
   - name: Configure web.config for SQL connection string
     hosts: all
     vars:
       ip_address: 10.0.0.1
       database_name: mydatabase
       db_username: sqluser
       db_password: Pa$$w0rd
     tasks:
       - name: Create web.config file
         template:
           src: templates/web.config.j2
           dest: c:/inetpub/wwwroot/web.config
   ```

   This code creates a new `web.config` file using the `web.config.j2` template and copies it to the `C:\inetpub\wwwroot` folder. The `{{ python_path }}` variable is replaced with the path to Python 3.7.

5. Save the `main.yml` and `web.config.j2` files and close them.

## Step 5: Copying Web Files to the `c:\inetpub\wwwroot` Folder

The next step is to copy the web files to the `c:\inetpub\wwwroot` folder.

1. Create a new Ansible role named `copy-web-files` using the following command:

   ```
   ansible-galaxy role init copy-web-files
   ```

2. Open the `tasks/main.yml` file in the `copy-web-files` role and add the following code:

   ```
   - name: Copy web files
     win_copy:
       src: ./files
       dest: C:\inetpub\wwwroot
   ```

   This code copies the contents of the `./files` folder to the `C:\inetpub\wwwroot` folder on the remote Windows machine.

3. Save the `main.yml` file and close it.

## Step 6: Combining the Roles in an Ansible Playbook

The final step is to combine the roles in an Ansible playbook.

1. Create a new file named `roles_playbook.yml` in your Ansible Control host in the ansible-working folder and add the following code:

   ```
   ---
   - name: Install IIS and configure website
     hosts: windows
     roles:
       - create-user
       - install-iis
       - create-web-config
       - copy-web-files
   ```

   This code defines an Ansible playbook that runs the `create-user`, `install-iis`, `create-web-config`, and `copy-web-files` roles.

2. Save the `roles_playbook.yml` file and close it.

## Step 7: Editing Files Using Visual Studio Code

To edit files using Visual Studio Code, follow these steps:

1. Open Visual Studio Code.

2. Open the folder containing the roles and playbook by clicking on `File > Open Folder` and selecting the folder.

3. Open the file you want to edit by double-clicking on it in the `Explorer` pane.

4. Make your changes to the file.

5. Save the file by clicking on `File > Save` or using the keyboard shortcut `Ctrl + S`.

6. Stage the changes by clicking on the `Source Control` icon in the left-hand menu, selecting the file, and clicking on the `+` icon.

7. Commit the changes by entering a commit message and clicking on the `✓` icon.

8. Push the changes to the `ansible-working` GitHub repository by clicking on the `...` icon in the `Source Control` pane and selecting `Push`.

## Step 8: Pulling the Changes on the Ansible Host

To pull the changes on the Ansible host, follow these steps:

1. Open PuTTY and connect to the Ansible host using SSH.

2. Navigate to the folder where the roles and playbook are stored.

3. Run the following command to pull the changes from the `ansible-working` GitHub repository:

   ```
   git pull
   ```

   This command will download the latest changes from the `ansible-working` repository.

## Step 9: Executing the Ansible Playbook

To execute the Ansible playbook, follow these steps:

1. Open PuTTY and connect to the Ansible host using SSH.

2. Navigate to the folder where the playbook is stored.

3. Run the following command to execute the playbook:

   ```
   ansible-playbook roles_playbook.yml
   ```

   This command will run the `playbook.yml` file and execute the roles in the specified order.

4. Wait for the playbook to finish executing.

Congratulations! You have successfully created an Ansible playbook that creates a Windows user with permissions to install services, installs IIS, creates a Web.config file using a template, and copies web files to the `C:\inetpub\wwwroot` folder. 
