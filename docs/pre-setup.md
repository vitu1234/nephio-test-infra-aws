### **SETUP ANSIBLE - AWS CREDENTIALS**

This should contain aws credentials

In the playbooks/cred.yml file add the AWS access

```bash
access_key: PUT_HERE
secret_key: PUT_HERE
```

#### Method 1: Manually enter password

- Create an Ansible vault called pass.yml in the playbooks/cred.yml directory with the following command,

```bash
  # 1. Create an ansible vault
  ansible-vault create playbooks/cred.yml

  # 2. There's a prompt for a password, it's needed for playbook execution/edit
  New Vault password:
  Confirm New Vault password:
```

With this method, you will be prompted for a password every time playbooks are executed or pass.yml is edited.

#### Method 2: Automate password access

- Though less secure, pass.yml (your ansible vault) can be created with an accompanying password file. This is referenced when executing playbooks and editing pass.yml so you no longer need to manually enter the password.

```bash
# 1. Create a hashed password file vault.pass in the root directory
openssl rand -base64 2048 > vault.pass

# 2. Create an ansible vault. 'vault.pass' is referenced with '--vault-password-file' option
ansible-vault create playbooks/cred.yml --vault-password-file vault.pass
```

With this method, --vault-password-file must now always be used when running the playbook or editing pass.yml, for example,

```bash
# 1. Create a hashed password file vault.pass in the root directory
openssl rand -base64 2048 > vault.pass

# 2. Create an ansible vault. 'vault.pass' is referenced with '--vault-password-file' option
ansible-vault create playbooks/cred.yml --vault-password-file vault.pass
```

With this method, --vault-password-file must now always be used when running the playbook or editing pass.yml, for example,

```bash
# Editing 'pass.yaml'
ansible-vault edit playbooks/cred.yml --vault-password-file vault.pass
```

#### Bonus - Encrypt and decrypt the credentials

```bash
ansible-vault encrypt playbooks/cred.yml
ansible-vault decrypt playbooks/cred.yml
```



Get AWS SESSION TOKEN:

```powershell
  aws sts get-session-token --duration-seconds 129600
  #if AMF is enabled in AWS use this command - assuming AWS CLI is already configured following the above link
  aws sts get-session-token --duration-seconds 129600 --serial-number arn:aws:iam::935317843233:mfa/VtiPhone-14P --token-code 890658
```

  Install python libraries in environment by creating a virtual env and activating it

  Create passwordless (***ubuntu***) user on the machine that will run the playbook

```bash
  python3 -m venv aws-ve
  source aws-venv/bin/activate
```
