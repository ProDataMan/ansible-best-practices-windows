# Lab: Using AAP to Manage Windows Hosts and Install IIS with Survey Input

Lab Objective: In this lab, you will learn how to use AAP to manage Windows hosts and install IIS with survey input. You will create a playbook that installs IIS on a Windows host, then use a survey to populate the contents of the index.html file on that host.

Lab Steps:

1. In Chrome, navigate to the public IP of your Ansible Automation Platform Server.
2. In AAP, navigate to the "Credentials" section (using the menu on the left) and verify that the `win_cred` credential has been created for the `webserver1` host.
3. Navigate to the "Projects" section and create a new project named `windows-iis`.
4. In the "windows-iis" project, create a new playbook named `win_iis.yml` with the following contents:
   
   ```yaml
   ---
   - name: Install IIS on Windows
     hosts: webserver1
     become: true
     tasks:
     - name: Install IIS Web Server Role
       win_feature:
         name: Web-Server
         state: present
     - name: Start World Wide Web Publishing Service
       win_service:
         name: W3SVC
         start_mode: auto
         state: started
   ```
5. Click the "Save" button to save the `win_iis.yml` playbook.
6. In the "windows-iis" project, create a new survey named `index_survey` with the following questions:
   
   ```yaml
   ---
   - name: index_title
     prompt: "Enter the title for the web page:"
     default: "Welcome to my web page"
   - name: index_body
     prompt: "Enter the body text for the web page:"
     default: "This is my first web page!"
   ```
7. Click the "Save" button to save the `index_survey` survey.
8. In the "windows-iis" project, create a new playbook named `update_index.yml` with the following contents:

   ```yaml
   ---
   - name: Update index.html on Windows
     hosts: webserver1
     become: true
     vars_prompt:
     - name: index_title
       prompt: "Enter the title for the web page:"
     - name: index_body
       prompt: "Enter the body text for the web page:"
     tasks:
     - name: Create index.html file
       win_copy:
         content: "<html><head><title>{{ index_title }}</title></head><body><h1>{{ index_title }}</h1><p>{{ index_body }}</p></body></html>"
         dest: "C:\\inetpub\\wwwroot\\index.html"
   ```
9. Click the "Save" button to save the `update_index.yml` playbook.
10. In the "AAP Inventory", navigate to the `Hosts` tab and click the `webserver1` host.
11. Click the "Run Playbook" button to run the `win_iis.yml` playbook against the `webserver1` host.
12. After the playbook finishes, click the "Run Playbook" button again to run the `update_index.yml` playbook against the `webserver1` host.
13. In the survey prompt, enter the desired title and body text for the index.html file on the `webserver1` host.
14. Verify that the index.html file on the `webserver
