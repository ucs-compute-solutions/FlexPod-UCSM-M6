# Cisco Converged Infrastructure setup using Ansible

 This is repository contains Ansible playbooks to configure Cisco Nexus, Cisco UCS, Cisco MDS, VMware ESXi and VMware vCenter. This repository can be used for setting up Cisco devices as well as VMware ESXi and vCenter as covered in following Cisco Validated Design (CVD): https://www.cisco.com/c/en/us/td/docs/unified_computing/ucs/UCS_CVDs/flexpod_datacenter_vmware_netappaffa_u2.html (with minor changes). Storage configuration is not part of this framework.

## Differences

The CVD used during this automation utilizes a dedicated set of vNICs for vMotion VMK. The new CVDs have moved away from using a dedicated vNIC pair and the vMotion VMK is now defined on the VMware distributed switch (VDS). This automation defines the vMotion VMK on the VDS.  

# Set up the execution environment

To execute various ansible playbooks, a linux based system will need to be setup with the packages listed at following pages:

Cisco UCS: https://galaxy.ansible.com/cisco/ucs
Cisco NxOS: https://docs.ansible.com/ansible/latest/network/user_guide/platform_nxos.html
VMware: https://docs.ansible.com/ansible/latest/scenario_guides/vmware_scenarios/vmware_intro.html

For more information: https://youtu.be/0oO3jgQ65Ss

# How to execute this automation framework?

The Converged Infrastructure setup takes place in the steps covered below. For more information, watch: https://youtu.be/tr3n-WeRo4g


1. Physically set up the equipment, perform the initial device configuration to allow management connectivity to devices. For more information: https://youtu.be/9V6rJF_gLwM
2. Setup Cisco Nexus switches and Cisco UCS admin, equipment and LAN tasks using: "ansible-playbook Setup_LAN_Connectivity.yml -i inventory". For more information: https://youtu.be/AhLum-DlH5E
3. Setup Cisco UCS server policies, service profile templates and SAN configuration using: "ansible-playbook Setup_UCS.yml -i inventory". For more information: https://youtu.be/gTOwiXoJJOA
4. Setup Cisco MDS configuration (when configuring FC) using: "ansible-playbook Setup_MDS.yml -i inventory".
5. Setup VMware vCenter and ESXi host configurations using: "ansible-playbook Setup_VMware.yml -i inventory".

# Setting up Variables

All the variables used in this framework are defined in the following locations:

1. Variable that apply to multiple devices: group_vars/all.yml
2. Cisco UCS, Nexus and MDS group variables are located in group_vars
3. Cisco Nexus and MDS host specific variables are located in host_vard
4. UCS configuration is broken down according to the tabs (admin, equipment, LAN, SAN, Server) and the variables associated with configuration of each tab are under the roles/UCS<tab_name>/defaults/main.yml
5. Nexus, MDS, ESXi and vCenter are all defined as unique roles and their specific variables are defined in the defaults/main.yml under the appropriate role directory

To make navigation of the roles easier, an HTML5 file: CI-Automation-Variable-Usage.html is included in the main directory of the framework and can be downloaded and clicked through in a browser to see the variables and default values.

# Post Configuration Tasks

The following items are not configured using the playbooks. Once the infrastructure is setup using the playbooks, users must manually confgure:
1. Change the VMware vCenter Cluster setting to set "Swap file location" to "Datastore specified by host"
2. Go the each ESXi host, click Configure and click "Swap File Location". Select "infra_swap" datastore as "Default swap file location"

# Playbook Execution Commands - Summary

1. Setup LAN on Nexus and UCS: "ansible-playbook ./Setup_LAN_Connectivity.yml -i inventory"
2. Setup UCS (including SAN if needed): "ansible-playbook ./Setup_UCS.yml -i inventory"
3. Setup MDS (if needed): "ansible-playbook ./Setup_MDS.yml -i inventory"
4. Setup VMware: "ansible-playbook ./Setup_VMware.yml -i inventory"
