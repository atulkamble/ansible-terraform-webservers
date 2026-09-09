# Configure Ansible Server and hosts 
```
git clone https://github.com/atulkamble/ansible-terraform-webservers.git
cd aws-ansible-terraform-webservers/terraform
terraform init 
terraform plan
terraform apply 
```
# delete Infra Env 
```
terraform destroy
```

# tips
```
1. From AWS EC2 Console >> Create Keypair >> Replace keypair path (ansible.pem)
2. sign in as root >> su >> enter password
```
# ansible-ec2-webservers

```
// update host setting inventory/hosts
[webservers-apache]
13.200.222.115

[webservers-nginx]
13.235.78.187

sudo touch apache.yml
sudo nano apache.yml
sudo touch nginx.yml
sudo nano nginx.yml

ansible-playbook -i inventory/hosts playbooks/apache.yml
ansible-playbook -i inventory/hosts playbooks/nginx.yml
ansible-playbook -i inventory/hosts apache.yml
ansible-playbook -i inventory/hosts nginx.yml

// apache server
sudo httpd -version
sudo httpd -t
sudo httpd -V | grep SERVER_CONFIG_FILE
sudo systemctl status httpd.service
sudo systemctl stop httpd.service
sudo systemctl start httpd.service

// nginx server
sudo nginx -v
sudo nginx -t
sudo systemctl status nginx
sudo systemctl stop nginx
sudo systemctl start nginx

ansible-playbook --syntax-check playbooks/nginx.yml
ansible-playbook -vvvv ping.yml

ansible-playbook -i inventory/hosts playbooks/nginx.yml
ansible-playbook -i inventory/hosts playbooks/apache.yml
```
```
sudo nano playbooks/apache.yml
```
```
---
- name: Install and configure Apache web server on Amazon Linux
  hosts: web
  become: yes
  tasks:
    - name: Update yum cache
      yum:
        name: '*'
        state: latest
        update_cache: yes

    - name: Install Apache (httpd)
      yum:
        name: httpd
        state: present

    - name: Ensure Apache is running and enabled at boot
      service:
        name: httpd
        state: started
        enabled: yes

    - name: Deploy a basic index.html
      copy:
        content: "<html><body><h1>Apache is working on EC2!</h1></body></html>"
        dest: /var/www/html/index.html
        owner: apache
        group: apache
        mode: '0755'
```
```
/etc/ansible/inventory/hosts
```
```
[web]
```
// server 
```
ansible-playbook -vvvv ping.yml
ansible-playbook --syntax-check playbooks/apache.yml
ansible-playbook -i inventory/hosts playbooks/apache.yml
ansible-playbook -i inventory/hosts playbooks/apache.yml -v
ansible-playbook -i inventory/hosts playbooks/apache.yml -vv
ansible-playbook -i inventory/hosts playbooks/apache.yml -vvv
ansible-playbook -i inventory/hosts playbooks/apache.yml -vvvv
```
// host 
```
ss -tuln
cd /var/www/html/
ls
cat index.html 
sudo systemctl status httpd
cat /etc/passwd
cat /etc/group
cd ..
ls -la
```
## Ansible Playbook Verbosity Levels

| Command                           | Verbosity Level | What It Shows                                                         |
| --------------------------------- | --------------- | --------------------------------------------------------------------- |
| `ansible-playbook site.yml -v`    | **Level 1**     | Standard task results, clean `stdout`, and `stderr`.                  |
| `ansible-playbook site.yml -vv`   | **Level 2**     | Task results plus input parameters passed to modules.                 |
| `ansible-playbook site.yml -vvv`  | **Level 3**     | Connection details, remote paths, and Python interpreter information. |
| `ansible-playbook site.yml -vvvv` | **Level 4**     | Detailed SSH connection debugging for deep troubleshooting.           |

### Quick Understanding

* **`-v`** → Basic verbose output
* **`-vv`** → More module/task details
* **`-vvv`** → Connection and execution details
* **`-vvvv`** → Full connection/SSH troubleshooting

### Example

```bash
ansible-playbook site.yml -vvv
```

**Tip:** For most Ansible troubleshooting, start with **`-vvv`**. Use **`-vvvv`** when diagnosing SSH or connection problems.
