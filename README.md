# Ansible Hackathon Setup

These playbooks bootstraps and provisions a set of machines in azure to be used for an Ansible Hackathon.

## Usage

### Prerequisites

A Linux bootstrap machine, it could be windows subsystem for linux (WSL), Cloud bash, whatever.

You need to have an azure subscription, and be logged in.

One way of doing this is using the Azure CLI. Get it here: [https://docs.microsoft.com/en-us/cli/azure/install-azure-cli?view=azure-cli-latest](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli?view=azure-cli-latest)

Then log in:

```bash
az login
```

### Setup

First we need to create and bootstrap azure resources needed for the hackathon.

Start by running:

```bash
ansible-playbook -i hosts bootstrap-resources.yml
```

This creates a set of host machines and some target servers in the resource-group name `ansiblehack-rg`. 
It might take some time, so have patience.

After that, all inventory should be described in  `azurehosts`, this is needed for provisioning.

### Provisioning

To install ansible, pywinrm, and creation of the `developer` use by which the developers on the hackathon logs into the hostmachines.
Run:

```bash
ansible-playbook -i azurehosts provision-hosts.yml
```

### Validation

From here on SSH into a host machine using the `developer` user:

```bash
ssh developer@<host-ip>
```

where \<host-ip\> is one of the ip addresses under `[hosts]` in `azurehosts`.