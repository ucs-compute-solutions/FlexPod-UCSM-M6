
# FlexPod Converged Infrastructure setup using Ansible for FlexPod Datacenter with Cisco UCS 4.2(1) in UCS Managed Mode, VMware vSphere 7.0 U2, and NetApp ONTAP 9.9

This repository for FlexPod contains Ansible playbooks to configure Cisco Nexus, Cisco UCS, Cisco MDS, NetApp ONTAP, NetApp ONTAP Tools for VMware, VMware ESXi and VMware vCenter. This repository can be used for setting up Cisco devices, NetApp ONTAP and associated NetApp tools as well as VMware ESXi and vCenter as covered in the following Cisco Validated Design (CVD): https://www.cisco.com/c/en/us/td/docs/unified_computing/ucs/UCS_CVDs/flexpod_m6_esxi7u2.html.

The CVD lays out the complete process for configuring the FlexPod using Ansible. Since these playbooks are intended to save time in setting up a working FlexPod, a complete FlexPod as shown below is needed to execute the playbooks. Various simulators could be used to partially test individual playbooks.

![block-diagram](https://github.com/ucs-compute-solutions/FlexPod-UCSM-M6/blob/master/ReadmePics/Main-Topology.jpg)  

# Set up the execution environment

To execute various ansible playbooks, a linux based system will need to be setup as described in the CVD with the packages listed at the following pages:

- Cisco UCS: https://galaxy.ansible.com/cisco/ucs
- Cisco NxOS: https://galaxy.ansible.com/cisco/nxos
- NetApp ONTAP: https://galaxy.ansible.com/netapp/ontap
- VMware: https://galaxy.ansible.com/community/vmware

# How to execute these playbooks?

![Block![block-diagram](https://github.com/ucs-compute-solutions/FlexPod-UCSM-M6/blob/master/ReadmePics/Main-Topology.jpg)-Diagram](https://github.com/ucs-compute-solutions/FlexPod-UCSM-M6/blob/master/ReadmePics/Ansible-Order.jpg)

Because a number of manual tasks need to be executed between running the Ansible playbooks, the CVD document should be used as a guide for running the playbooks. Commentary is included in the variable files to guide filling in those values.

The steps for setting up a FlexPod with Fibre Channel boot and FC-NVMe and NFS storage protocols are:

1.  Create a directory and clone the repository from Github with "git clone https://github.com/ucs-compute-solutions/FlexPod-UCSM-M6.git".
2.  Fill in the variable files according to the CVD.
3.  Follow the manual steps in the CVD to set up the Nexus switches on the network and ssh into each switch.
4.  Execute the Nexus playbook with "ansible-playbook ./Setup_Nexus.yml -i inventory" to setup the Nexus switches.
5.  Follow the manual steps in the CVD to add timezone information to the Nexus switches.
6.  Follow the manual steps in the CVD to get the NetApp storage cluster on the network.
7.  Execute the NetApp storage playbook with "ansible-playbook -i inventory Setup_ONTAP.yml -t ontap_config_part_1".
8.  Query the fibre channel and FC-NVMe LIF WWPNs and add to the "all.yml" file.
9.  Follow the manual steps in the CVD to get the UCS fabric interconnects on the network and setup the fibre channel unified ports.
10.  Execute the UCS playbook with "ansible-playbook ./Setup_UCS.yml -i inventory" to setup the Cisco UCS.
11.  Follow the manual steps in the CVD to create UCS service profiles for three VMware ESXi management hosts.
12.  Query the ESXi host initiator WWPNs for both fibre channel and FC-NVMe interfaces and add to the "all.yml" file.
13.  Follow the manual steps in the CVD to set up the MDS switches on the network and ssh into each switch.
14.  Execute the MDS playbook with "ansible-playbook ./Setup_MDS.yml -i inventory".
15.  Follow the manual steps in the CVD to add timezone information to the MDS switches.
16.  Add the ESXi host initiator fibre channel WWPNs to "vars/ontap.yml"
17.  Execute the NetApp storage playbook with "ansible-playbook -i inventory Setup_ONTAP.yml -t ontap_config_part_2" to create and map the ESXi boot LUNs.
18.  Follow the manual steps in the CVD to install VMware ESXi on the three host servers and assign IPs to those servers.
19.  Execute the ESXi playbook with "ansible-playbook ./Setup_ESXi.yml -i inventory" to setup the three ESXi hosts.
20.  Bring a vCenter into the environment by either installing vCenter on the first ESXi host according to the CVD or copying it in.
21.  Setup the vCenter and add the three ESXi hosts to it by executing "ansible-playbook ./Setup_vCenter.yml -i inventory".
22.  Follow the manual steps in the CVD to complete setting up vCenter and the ESXi hosts.
23.  Execute the NetApp storage playbook with "ansible-playbook -i inventory Setup_ONTAP.yml -t ontap_config_part_3" to setup FC-NVMe.
24.  Execute the manual steps in the CVD to complete the FC-NVMe setup.
25.  Execute the manual steps in the CVD to add the Cisco UCS Manager Plug-in for VMware vSphere Web Client.
26.  Execute the NetApp ONTAP tools playbook with "ansible-playbook -i inventory Setup_ONTAP_tools.yml" to install the ONTAP Tools VM.
27.  Follow the manual steps in the CVD to finish setting up ONTAP tools and the rest of the FlexPod management cluster, including an optional Ansible setup of NetApp AIQUM.

The Ansible playbooks and CVD are structured in a way that a Fibre Channel Boot, Fibre Channel Boot with FC-NVMe, or an iSCSI Boot FlexPod configuration can be setup by adjusting the variables. Also, the playbooks can be used to setup the following alternative FlexPod Topologies: first a Fibre Channel topology using Nexus 93180YC-FX switches for both LAN and SAN switching and second a high-bandwidth topology with 100GE core networking.

![block-diagram](https://github.com/ucs-compute-solutions/FlexPod-UCSM-M6/blob/master/ReadmePics/NexusSAN-Topology.jpg)

![block-diagram](https://github.com/ucs-compute-solutions/FlexPod-UCSM-M6/blob/master/ReadmePics/High-Bandwidth-Topology.jpg)
