### Automated ONTAP storage deployment using Ansible for FlexPod Datacenter with VMware vSphere 7.0 U2 and NetApp ONTAP 9.8 and 9.9
 
This repository contains Ansible roles and playbooks for an end-to-end ONTAP storage deployment for VMware vSphere 7.0 U2 in a FlexPod Datacenter.

The ONTAP deployment automation is based on the following roles:

	ontap_primary_setup
	ontap_network
	ontap_svm
	ontap_volumes
	ontap_lifs
	ontap_luns
	ontap_nvme
	ontap_finalize_setup

The ONTAP Tools for VMware vSphere (previously VSC) deployment is based on the following role and is available in the 'roles' directory of this repository:

	ontap_tools-config

These roles are developed as per the best practices prescribed in the Cisco Validated Design (CVD) "FlexPod Datacenter with Cisco UCS M6, VMware 7.0 U2 and NetApp ONTAP 9.9".

### Environment Validated

As the automation solution is specifically build for the above mentioned CVD, the current roles and playbooks support the following components:

	Storage Operating System: ONTAP 9.8 and 9.9
	Storage Protocols: iSCSI, NFS and NVMe
	VMware vSphere: 7.0 U2

### Prerequisite

1. It is assumed that the physical rack and stack, power-on, initialisation of ONTAP OS, setup of Node Management IPs and initial ONTAP Cluster with IP is completed.
NOTE: Aggregate creation is part of the automation.

2. The user should have an Ansible Control machine that has network reachability to the ONTAP storage system and internet access to pull this repository from GitHub.
Refer https://docs.ansible.com/ansible/latest/installation_guide/intro_installation.html for guidance on setting up an Ansible Control machine.

3. The Ansible control machine should have the NetApp and VMware dependency libraries and collections installed

```
pip3 install netapp-lib
ansible-galaxy collection install netapp.ontap
pip3 install pyvmomi
ansible-galaxy collection install community.vmware
pip3 install -r ~/.ansible/collections/ansible_collections/community/vmware/requirements.txt
```

### Getting Started

1. From the Ansible Control machine Download a ZIP version of this repository or clone it using the below command:
	
```
git clone https://github.com/ucs-compute-solutions/FlexPod-UCSM-M6.git
```

2. There are two variable files under the vars folder 'ontap_main.yml' for setup of ONTAP and 'ontap_tools_main.yml' for setup of ONTAP tools, that need to be filled out with environment specific parameters prior to executing the playbook.

3. This playbook to setup ONTAP will also use some common parameters defined in the all.yml variable file under group_vars, please ensure that the all.yml file is up to date. 

NOTE: The format of the variable file needs to be maintained as it is, any changes to the structure of the file may lead to failure in execution of the playbook.

NOTE: Sample values are pre-populated against some variables in order to provide the user additional clarity on how the variable needs to be filled out. Please replace the sample values with your environment specific information.

4. Update the credentials for the ONTAP Cluster and VMware vCenter

Navigate to the 'ontap' file within the 'group_vars' directory and update it with the admin credentials for the ONTAP cluster 

Example -

	# Credentials for connecting to the ONTAP Cluster
	username: admin
	password: password

Navigate to the 'vcenter' file within the 'group_vars' directory and update it with the admin credentials for VMware vCenter

Example -

	# This variable file is used by the playbooks for the ONTAP tools
	vcenter_username: "administrator@vsphere.local"
	vcenter_password: "password"

5. Update the Inventory file

Open the 'inventory' file and update it with a record for the ONTAP Cluster Management IP and vCenter IP

Example -


	[ontap]
	# ONTAP Cluster Management IP, list only one ONTAP Cluster IP
	192.168.10.5

	[vcenter]
	# vCenter Management IP. List only one vCenter IP. This is used by the playbooks for ONTAP tools.
	192.168.10.10

6. Executing the Playbook

A playbook by name 'Setup_ONTAP.yml' is available at the root of this repository. It calls all the required roles to complete the setup of the ONTAP storage system.

The ONTAP setup is split into three sections, use the tags - ontap_config_part_1, ontap_config_part_2 and ontap_config_part_3 to execute parts of the playbook at the appropriate stage of setup.

Execute the playbook from the Ansible Control machine as an admin/ root user using the following command:


	After setup of Cisco Nexus switches	-	ansible-playbook -i hosts Setup_ONTAP.yml -t ontap_config_part_1

	After setup of Cisco UCS		-	ansible-playbook -i hosts Setup_ONTAP.yml -t ontap_config_part_2

	After setup of VMware ESXi		-	ansible-playbook -i hosts Setup_ONTAP.yml -t ontap_config_part_3
	

If you would like to run a part of the deployment, you may use the appropriate tag that accompanies each task in the role and run the playbook in the below fashion -

	ansible-playbook -i inventory Setup_ONTAP.yml -t <tag_name>
	
Another playbokk by name 'Setup_ONTAP_tools' is available at the root of this repository. It call the appropriate roles to complete the setup of ONTAP Tools for VMware. 

	ansible-playbook -i inventory Setup_ONTAP_tools.yml

### Authors

 * Arvind Ramakrishnan (arvind.ramakrishnan@netapp.com)
 * Kamini Singh (kamini.singh@netapp.com) 
