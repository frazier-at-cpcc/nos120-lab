# NOS120 Lab Environment Setup

This repository contains Ansible playbooks for setting up the NOS120 lab environment with three Rocky Linux 9.5 virtual machines using KVM.

## Virtual Machine Specifications

Each VM is configured with:
- 30GB storage
- 2 vCPUs
- 4GB RAM
- Rocky Linux 9.5 minimal installation
- Static IP configuration

### VM Network Configuration
- workstation.lab.example.com (172.25.250.9)
- servera.lab.example.com (172.25.250.10)
- serverb.lab.example.com (172.25.250.11)

## Prerequisites

The lab host must have:
- KVM and libvirt installed and configured
- Ansible installed
- Sufficient storage space for VM images
- Network bridge configured (br0)

These prerequisites are handled by the `prepare_lab_host.yml` playbook.

## Playbook Descriptions

1. `prepare_lab_host.yml`
   - Configures the lab host with required packages and services
   - Sets up networking (bridge, DHCP, DNS)
   - Configures KVM environment

2. `generate_kickstarts.yml`
   - Generates customized kickstart files for each VM
   - Sets hostname and IP configuration
   - Configures storage and user accounts

3. `provision_vms.yml`
   - Downloads Rocky Linux 9.5 ISO
   - Creates VM disk images
   - Provisions VMs using virt-install
   - Configures VM resources (CPU, RAM, storage)

## Usage

1. First, ensure the lab host is properly configured:
```bash
ansible-playbook -i inventory.yml prepare_lab_host.yml
```

2. Generate kickstart files for the VMs:
```bash
ansible-playbook -i inventory.yml generate_kickstarts.yml
```

3. Provision the VMs:
```bash
ansible-playbook -i inventory.yml provision_vms.yml
```

## File Structure
```
.
├── README.md
├── inventory.yml
├── prepare_lab_host.yml
├── generate_kickstarts.yml
├── provision_vms.yml
├── kickstart_template.cfg
├── configure_workstation.yml
├── configure_servera.yml
└── configure_serverb.yml
```

## Post-Installation

After the VMs are provisioned, they can be accessed via:
- SSH using the student user account
- Cockpit web interface at https://lab-host:9090
- Virtual Machine Manager (virt-manager) GUI

Each VM is configured with:
- Student user account
- Network connectivity through br0 bridge
- SELinux in enforcing mode
- Basic security settings
