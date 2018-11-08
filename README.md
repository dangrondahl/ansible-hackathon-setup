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

## Validation

From here on SSH into a host machine using the `developer` user:

```bash
ssh developer@<host-ip>
```

where \<host-ip\> is one of the ip adresses under `[hostmachines]` in `azurehosts`.

To validate if the host has access to the target machines, create an inventory file on the host machine by echoing the following into `hosts`:

```bash
echo -e  "[servers]\n10.0.1.[6:9] ansible_user=developer ansible_password=hack@123456 ansible_port=5986 ansible_connection=winrm ansible_winrm_server_cert_validation=ignore" >> hosts
```

Now just run:

```bash
ansible all -i hosts -m win_ping
```

The response should be as below, in which case, the environment is ready for the hackathon:

```bash
10.0.1.6 | SUCCESS => {
    "changed": false,
    "ping": "pong"
}
10.0.1.9 | SUCCESS => {
    "changed": false,
    "ping": "pong"
}
10.0.1.8 | SUCCESS => {
    "changed": false,
    "ping": "pong"
}
10.0.1.7 | SUCCESS => {
    "changed": false,
    "ping": "pong"
}
```