# Ansible Hackathon Setup

These playbooks bootstraps and provisions a set of machines for the Ansible Hackathon Day.

## Usage

First we need to create and bootstrap azure resources needed for the hackathon.

Start by running:

```bash
ansible-playbook -i hosts bootstrap-resources.yml
```

This creates a set of host machines and some target servers.

It might take some time, so have patience.
After that, we need to get the ip addresses of the Virtual Machines created.

Run:

```bash
ansible-playbook -i hosts create-inventory.yml
```

This creates a file called `azurehosts` needed for the next step.

Run:

```bash
ansible-playbook -i azurehosts provision-hosts.yml
```

To install ansible, pywinrm, and creation of the `developer` use by which the developers on the hackathon logs into the hostmachines.

From here on, it should be possible to log into a host machine using SSH:

```bash
ssh developer@<host-ip>
```

To test if it has access to the target machines, one could create a hosts file on the hosts machine by echoing the following into `hosts`:

```bash
echo -e  "[servers]\n10.0.1.[6:9] ansible_user=developer ansible_password=hack@123456 ansible_port=5986 ansible_connection=winrm ansible_winrm_server_cert_validation=ignore" >> hosts
```

