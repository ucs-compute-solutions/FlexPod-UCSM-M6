
# FlexPod Converged Infrastructure setup using Ansible

This repository for FlexPod contains Ansible playbooks to configure Cisco Nexus, Cisco UCS, Cisco MDS, NetApp ONTAP, NetApp ONTAP Tools for VMware, VMware ESXi and VMware vCenter. This repository can be used for setting up Cisco devices, NetApp ONTAP and associated NetApp tools as well as VMware ESXi and vCenter as covered in the following Cisco Validated Design (CVD): https://www.cisco.com/c/en/us/td/docs/unified_computing/ucs/UCS_CVDs/flexpod_datacenter_vmware_netappaffa_u2.html. The CVD lays out the complete process for configuring the FlexPod using Ansible.

![block-diagram](https://github.com/ucs-compute-solutions/FlexPod-UCSM-M6/blob/master/ReadmePics/Main-Topology.jpg)  

# Set up the execution environment

To execute various ansible playbooks, a linux based system will need to be setup as shown in the CVD with the packages listed at the following pages:

- Cisco UCS: https://galaxy.ansible.com/cisco/ucs
- Cisco NxOS: https://galaxy.ansible.com/cisco/nxos
- NetApp ONTAP: https://galaxy.ansible.com/netapp/ontap
- VMware: https://galaxy.ansible.com/community/vmware

# How to execute these playbooks?

![Block![block-diagram](https://github.com/ucs-compute-solutions/FlexPod-UCSM-M6/blob/master/ReadmePics/Main-Topology.jpg)-Diagram](https://github.com/ucs-compute-solutions/FlexPod-UCSM-M6/blob/master/ReadmePics/Ansible-Order.jpg)

Because a number of manual tasks need to be executed between running the Ansible playbooks, the CVD document should be used as a guide for running the playbooks. Commentary is included in the variable files to guide filling in those values.

The Ansible playbooks and CVD are structured in a way that a Fibre Channel Boot, Fibre Channel Boot with FC-NVMe, or an iSCSI Boot FlexPod configuration can be setup by adjusting the variables. Also, the playbooks can be used to setup the following alternative FlexPod Topologies: first a Fibre Channel topology using Nexus 93180YC-FX switches for both LAN and SAN switching and second a high-bandwidth topology with 100GE core networking.

![block-diagram](https://github.com/ucs-compute-solutions/FlexPod-UCSM-M6/blob/master/ReadmePics/NexusSAN-Topology.jpg)

![block-diagram](https://github.com/ucs-compute-solutions/FlexPod-UCSM-M6/blob/master/ReadmePics/High-Bandwidth-Topology.jpg)
