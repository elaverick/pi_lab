# Ansible Scripts for Mini-Lab Environment Setup on Raspberry Pi

This repository contains a collection of Ansible playbooks designed to set up a mini-lab environment on Raspberry Pi devices running Ubuntu Server. These scripts automate the setup of various services, with the initial focus on an LDAP server and a Root CA (Certificate Authority). The playbooks are intended to work on freshly installed Ubuntu Server environments.

## Features

- **LDAP Server Setup**: Provision an LDAP server for managing users and groups.
- **Root CA Setup**: Create a Root Certificate Authority that is kept offline after generating the required certificates.
- **Variable Management**: Simplified handling of variables, including sensitive data.
- **User and Group Management**: Automatically create LDAP users and groups from a list.

## Requirements

- Raspberry Pi running a clean installation of [Ubuntu Server](https://ubuntu.com/download/raspberry-pi).
- Ansible installed on the control machine.
- SSH access to Raspberry Pi devices.

## Directory Structure

```
.
├── ldap.yaml                    # LDAP server setup playbook
├── rootCA.yaml                  # Root Certificate Authority setup playbook
├── vars/
│   ├── variables.yaml           # Regular variables (e.g., domain name, TLD)
│   └── sensitiveVars.yaml       # Sensitive variables (e.g., email addresses, passwords)
└── inventory.yaml               # Ansible inventory file
```

## Configuration

### Variables

- **Regular Variables**: These are stored in the `vars/variables.yaml` file and contain general configuration such as domain names and TLDs.
  
- **Sensitive Variables**: Sensitive data, such as email addresses and passwords, are stored in `vars/sensitiveVars.yaml`. This file is masked to prevent the accidental committing of private information.

### LDAP Server Configuration

You can define the LDAP users and groups in the `vars/variables.yaml` file by modifying the `ldapUsers` and `ldapGroups` collections. The playbook will automatically provision users and groups defined in these lists.

Example structure:

```yaml
ldapUsers:
  - uid: "jdoe"
    firstname: "John"
    surname: "Doe"
    gidNumber: 5000
  - uid: "jsmith"
    firstname: "Jane"
    surname: "Smith"
    gidNumber: 5000

ldapGroups:
  - name: "admins"
    gidNumber: 5000
  - name: "users"
    gidNumber: 5001
```

### Root CA Setup

The Root Certificate Authority is used to generate certificates for the environment. After generating the required certificates, the Root CA should be kept offline. The playbooks handle copying the generated certificates locally to facilitate distribution between machines.

## Running the Playbooks

### Setting up the LDAP Server

To run the LDAP server playbook, execute the following command:

```bash
ansible-playbook ldap.yaml -e "ldap_password=YOUR_PASSWORD"
```

Replace `YOUR_PASSWORD` with the password you want to use for the LDAP server. The password is passed as an extra variable (`EXTRA_VARS`) to prevent committing it to the repository.

### Setting up the Root CA

To run the Root CA setup playbook:

```bash
ansible-playbook rootCA.yaml
```

The playbook will generate the necessary certificates and store them in a location specified in the playbook. These certificates will then be copied to the local machines to pass between them as needed.

## Sensitive Variables

Sensitive information such as email addresses and passwords should be stored in the `vars/sensitiveVars.yaml` file. The contents of this file are masked to prevent any private data from being committed to the repository.

### Example of sensitiveVars.yaml

```yaml
emailAddress: "admin@example.com"
```

## Contributing

Feel free to open issues or submit pull requests to improve these playbooks. Contributions are welcome, especially regarding additional services to be configured on the Raspberry Pi.

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.
