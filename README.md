# mode12
sudo wget -0 /etc/apt/keyrings/jenkins-keyring.asc \
sudo apt install jenkins -y
sudo cat /var/lib/jenkins/secrets/initialAdminPassword
-----------
sudo apt update
sudo apt install -y software-properties-common
sudo add-apt-repository --yes --update ppa:ansible/ansible
sudo apt install ansible
Now we are going to navigate the directory path to,
$ cd /etc/ansible/
Then, we will check the latest version of the ansible by,
$ ansible –version
$ ls –la
Then check the host file by,
$ sudo nano hosts
$ sudo ansible-galaxy init role-name
$ ls
tree role-name/
$ sudo ansible-galaxy init prerequisites
$ sudo ansible-galaxy init PostgreSQL
$ sudo ansible-galaxy init apache
$ ls
First for prerequisites,
$ cd prerequisites/tasks/
Then edit the main.yml file by the command,
$ sudo nano main.yml
---
# tasks file for prerequisites

- name: Install git
  apt:
    name: git
    state: present
    update_cache: yes
  Next for PostgreSQL,
$ ls

$ cd PostgreSQL/tasks/
$ sudo nano main.yml
Then add the content to this main.yml file as,
# tasks file for PostgreSQL

- name: Install PostgreSQL
  apt:
    name: postgresql
    state: present
    update_cache: yes

- name: Start PostgreSQL service
  systemd:
    name: postgresql
    state: started
    enabled: yes
Finally for the apache web server,
$ cd apache/tasks/
$ sudo nano main.yml
# tasks file for apache

- name: install Apache web server
  apt:
    name: apache2
    state: present
    update_cache: yes
$ sudo nano stack.yml
- hosts: localhost
  become: true
  roles:
    - prerequisites
    - PostgreSQL
    - apache
$ sudo ansible-playbook stack.yml
$ git --version
$ apache2 -v
-------------------------
$ mkdir -p ~/ansible-webapp
$ cd ~/ansible-webapp
$ nano webapp.yml
$ mkdir -p ~/ansible-webapp
$ cd ~/ansible-webapp
To add content to playbook use the following commands,
$ nano webapp.yml
- name: Deploy a Simple Web Application
  hosts: localhost
  connection: local
  become: true

  tasks:
    - name: Install Apache web server
      ansible.builtin.apt:
        name: apache2
        state: present
        update_cache: true

    - name: Start and enable Apache
      ansible.builtin.service:
        name: apache2
        state: started
        enabled: true

    - name: Deploy the web application
      ansible.builtin.copy:
        dest: /var/www/html/index.html
        content: |
          <!DOCTYPE html>
          <html>
          <head>
          <title>My Ansible Web App</title>
          </head>
          <body>
          <h1>Hello from Ansible!</h1>
          <p>My web application was deployed automatically.</p>
          </body>
          </html>
        mode: "0644"

    - name: Verify the web application
      ansible.builtin.uri:
        url: http://127.0.0.1/
        status_code: 200
      register: web_result

    - name: Display deployment status
      ansible.builtin.debug:
        msg: "Web application deployed successfully!"
$ ansible-playbook webapp.yml --ask-become-pass
$ sudo systemctl status apache2
$ curl http://localhost
