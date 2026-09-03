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
