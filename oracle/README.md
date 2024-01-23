# Deployment on Oracle Cloud

## A step-by-step guide

This assumes you are working on Ubuntu Linux

### 1. Install Oracle Cloud CLI

```bash
bash -c "$(curl -L https://raw.githubusercontent.com/oracle/oci-cli/master/scripts/install/install.sh)"
```

If this is not working, refer to instructions at: https://docs.oracle.com/en-us/iaas/Content/API/SDKDocs/cliinstall.htm#InstallingCLI__linux_and_unix

### 2. Install `oci-ansible-collection`

```bash
curl -L https://raw.githubusercontent.com/oracle/oci-ansible-collection/master/scripts/install.sh | bash -s -- --verbose
```

If this is not working, refer to instructions at: https://github.com/oracle/oci-ansible-collection

### 3. Install Ansible

```bash
sudo apt update
sudo apt install software-properties-common
sudo add-apt-repository --yes --update ppa:ansible/ansible
sudo apt install ansible
```

If this is not working refer to instructions at: https://docs.ansible.com/ansible/latest/installation_guide/intro_installation.html#intro-installation-guide

### 4. Activate `oci-ansible-collection` virtualenv

```bash
source ~/lib/oci-ansible-collection/bin/activate
```

### 4.5. Install Docker Ansible collection

```bash
ansible-galaxy collection install community.docker --upgrade
```

### 5. Verify you have set up access via OCI CLI

This should print a list of users:
```bash
oci iam user list
```

### 6. Run the playbook to start an instance on OCI

```bash
ansible-playbook oracle/tasks/main.yml -e mode=deploy
```

### 7. Change the IP address in `environments/oracle/hosts` to the public IP of the new instance and `roles/backend/defaults/main.yml`


### 8. Start yaptide on the instance

```bash
ansible-playbook --inventory environments/oracle/hosts site.yml
```

### 8,5. SSH to the instance and check the contents of `~/ui/.env` and `~/yaptide/.env`

### 9. Teardown an instance

TODO FIXME this does not see the necessary variables

```bash
ansible-playbook oracle/tasks/main.yml -e mode=clean
```



## To improve
 - set up VSCode editing via SSH
 - Add another SSH key locally
 - Automatically answering YES on accepitng ssh fingerprint on connecting to machine in the site.yml
 - save IP of the new instance in inventory
 - Clean up with proper Ansible fact management
 - Set FQDN automatically
 - self-signed SSL certificate should not be under version control
 - change SSL certificate to use the right DNS, not example.com
 - connect backend with Ares cluster
 - make openstack deploy idempotent so that you do not have to make a new instance if something goes wrong (NEW project for Dr Grzanka?)

 - dodać w przeglądarce certyfikat do frontendu i backendu

## To mention
 - HTTPS forced on calls between front and backend (we want HTTP but app forces HTTPS, which the backend does not listen for)


 ## Useful commands

1. SSH to the deployed instance

```bash
ssh -i ~/.ssh/yaptide/private_key.pem ubuntu@<server_ip>
```

