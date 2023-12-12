## Dojo-Jump CI/CD Pipeline with Ansible and GitHub Actions

This project implements a continuous integration and continuous deployment (CI/CD) pipeline for the Dojo-Jump static website using Ansible and GitHub Actions.

**Project Goals:**

- Automate the deployment of Dojo-Jump to a remote server.
- Leverage Ansible for configuration management and playbook execution.
- Utilize GitHub Actions to trigger the deployment on push events.

**Technologies Used:**

- Ansible: Configuration management tool
- GitHub Actions: CI/CD platform
- Nginx: Web server
- Dojo-Jump: Static website game

**Pipeline Workflow:**

1. Push changes to the GitHub repository.
2. GitHub Actions automatically triggers the CI/CD pipeline.
3. The workflow:
   - Sets up the Python environment.
   - Installs Ansible.
   - Configures SSH key authentication.
   - Runs the Ansible playbook to deploy Dojo-Jump.
4. Ansible playbook:
   - Installs Nginx on the server.
   - Copies the Dojo-Jump website files to `/var/www/html/`.
   - Configures and enables Nginx for the website.
   - Restarts Nginx.

**Inventory File:**

```
[webservers]
node1 ansible_host=35.177.71.250 ansible_user=ubuntu
```

**Playbook:**

```yaml
---
- hosts: all
  become: yes
  tasks:
    - name: Install Nginx
      apt:
        name: nginx
        state: present

    - name: Copy Website Files
      copy:
        src: https://github.com/chandradeoarya/dojo-jump
        dest: /var/www/html/
        mode: "0644"

    - name: Create Nginx configuration file
      template:
        src: nginx.conf.j2
        dest: /etc/nginx/sites-available/dojo-jump

    - name: Enable Nginx configuration
      file:
        src: /etc/nginx/sites-available/dojo-jump
        dest: /etc/nginx/sites-enabled/dojo-jump
        state: link

    - name: Restart Nginx
      systemd:
        name: nginx
        state: restarted
```

**Next Steps:**

- Replace `nginx.conf.j2` with a custom Nginx configuration file tailored to your specific needs.
- Implement additional tasks in the playbook for further customization.
- Consider integrating tools like Docker for a more portable deployment environment.

This CI/CD pipeline provides a foundation for automating Dojo-Jump deployments and ensures a smooth and efficient update process.
