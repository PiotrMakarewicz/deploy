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

### 5. Verify you have set up access via OCI CLI

This should print a list of users:
```bash
oci iam user list
```

### 6. Run the playbook to start an instance on OCI

```bash
ansible-playbook oracle/tasks/main.yml -e mode=deploy
```

### 7. Start yaptide on the instance

```bash
ansible-playbook --inventory environments/oracle/hosts site.yml
```

### 8. Teardown an instance

TODO FIXME this does not see the necessary variables

```bash
ansible-playbook oracle/tasks/main.yml -e mode=clean
```
