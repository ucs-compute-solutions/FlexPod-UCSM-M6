[![published](https://static.production.devnetcloud.com/codeexchange/assets/images/devnet-published.svg)](https://developer.cisco.com/codeexchange/github/repo/ucs-compute-solutions/Config_Converged_Infrastructure)

# Cisco Converged Infrastructure setup using Ansible

This repository contains Ansible playbooks to configure Cisco Nexus, Cisco UCS, Cisco MDS, NetApp ONTAP, NetApp ONTAP Tools for VMware, VMware ESXi and VMware vCenter. This repository can be used for setting up Cisco devices, NetApp ONTAP and associated NetApp tools as well as VMware ESXi and vCenter as covered in following Cisco Validated Design (CVD): https://www.cisco.com/c/en/us/td/docs/unified_computing/ucs/UCS_CVDs/flexpod_datacenter_vmware_netappaffa_u2.html (with minor changes).

![block-diagram](https://user-images.githubusercontent.com/60270001/110512605-59530580-80d3-11eb-9642-9f89a851d902.jpg)
[Image to be updated]

## Differences

The CVD used during this automation utilizes a dedicated set of vNICs for vMotion VMK. The new CVDs have moved away from using a dedicated vNIC pair and the vMotion VMK is now defined on the VMware distributed switch (VDS). This automation defines the vMotion VMK on the VDS.  

# Set up the execution environment

To execute various ansible playbooks, a linux based system will need to be setup with the packages listed at the following pages:

- Cisco UCS: https://galaxy.ansible.com/cisco/ucs
- Cisco NxOS: https://galaxy.ansible.com/cisco/nxos
- NetApp ONTAP: https://galaxy.ansible.com/netapp/ontap
- VMware: https://docs.ansible.com/ansible/latest/scenario_guides/vmware_scenarios/vmware_intro.html

For more information: https://youtu.be/0oO3jgQ65Ss

# How to execute these playbooks?

The Converged Infrastructure setup takes place in the steps covered below. For more information, watch: https://youtu.be/tr3n-WeRo4g
[Image needs to be updated to include ONTAP]
![Block-Diagram](https://user-images.githubusercontent.com/60270001/111256914-867e4700-85f0-11eb-9dfe-62e54909610b.jpg)

1. Physically set up the equipment, perform the initial device configuration to allow management connectivity to devices. For more information: https://youtu.be/9V6rJF_gLwM
2. Setup Cisco Nexus switches and Cisco UCS admin, equipment and LAN tasks using: "ansible-playbook Setup_LAN_Connectivity.yml -i inventory". For more information: https://youtu.be/AhLum-DlH5E
3. Setup NetApp All Flash FAS using: "ansible-playbook Setup_ONTAP.yml -i inventory --tags ontap_config_part_1". 
>>Note: Replace with tags - ontap_config_part_2 and ontap_config_part_3 to complete the ONTAP setup. Refer to the CVD for orderly execution.
4. Setup Cisco UCS server policies, service profile templates and SAN configuration using: "ansible-playbook Setup_UCS.yml -i inventory". For more information: https://youtu.be/gTOwiXoJJOA
5. Setup Cisco MDS configuration (when configuring FC) using: "ansible-playbook Setup_MDS.yml -i inventory".
6. Setup VMware vCenter and ESXi host configurations using: "ansible-playbook Setup_VMware.yml -i inventory".
7. Setup ONTAP Tools for VMware using: "ansible-playbook Setup_ONTAP_tools.yml -i inventory"

[Add reference here to AIQUM automated installation]
  
# Setting up Variables

All the variables used in this framework are defined in the following locations:

1. Variable that apply to multiple devices: group_vars/all.yml
2. Cisco UCS, Nexus and MDS group variables are located in group_vars
3. Cisco Nexus and MDS host specific variables are located in host_var
4. NetApp ONTAP specific variables are located in /vars/ontap_main.yml
5. ONTAP Tools for VMware specific variables are located in /vars/ontap_tools_main.yml
6. UCS configuration is broken down according to the tabs (admin, equipment, LAN, SAN, Server) and the variables associated with configuration of each tab are under the roles/UCS<tab_name>/defaults/main.yml
7. Nexus, MDS, ESXi and vCenter are all defined as unique roles and their specific variables are defined in the defaults/main.yml under the appropriate role directory

To make navigation of the roles easier, an HTML5 file: CI-Automation-Variable-Usage.html is included in the main directory of the framework and can be downloaded and clicked through in a browser to see the variables and default values. 

[This HTML file needs to be updated to include the NetApp portions]

# Post Configuration Tasks

The following items are not configured using the playbooks. Once the infrastructure is setup using the playbooks, users must manually confgure:
1. Change the VMware vCenter Cluster setting to set "Swap file location" to "Datastore specified by host"
2. Go the each ESXi host, click Configure and click "Swap File Location". Select "infra_swap" datastore as "Default swap file location"

# Playbook Execution Commands - Summary

1. Setup LAN on Nexus and UCS: "ansible-playbook ./Setup_LAN_Connectivity.yml -i inventory"
2. Setup NetApp ONTAP: "ansible-playbook Setup_ONTAP.yml -i inventory --tags ontap_config_part_1" 
   >> Note: other tag options are  ontap_config_part_2 & ontap_config_part_3"
3. Setup UCS (including SAN if needed): "ansible-playbook ./Setup_UCS.yml -i inventory"
4. Setup MDS (if needed): "ansible-playbook ./Setup_MDS.yml -i inventory"
5. Setup VMware: "ansible-playbook ./Setup_VMware.yml -i inventory"
6. Setup ONTAP Tools for VMware: "ansible-playbook Setup_ONTAP_tools.yml -i inventory"
