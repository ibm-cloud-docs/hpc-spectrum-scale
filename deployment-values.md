---

copyright:
  years: 2022
lastupdated: "2022-05-26"

keywords: 

subcollection: hpc-spectrum-scale

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:external: target="_blank" .external}
{:pre: .pre}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:table: .aria-labeledby="caption"}

# Deployment values
{: #deployment-values}

The following deployment values can be used to configure the {{site.data.keyword.scale_short}} cluster instance on {{site.data.keyword.cloud}}:

| Value | Description | Is it required? | Default value |
| ----- | ----------- | --------------- | ------------ |
| `bastion_key_pair` | Name of the SSH key configured in your {{site.data.keyword.cloud_notm}} account that is used to establish a connection to the bastion and bootstrap nodes. Make sure that the SSH key is present in the same resource group and region where the cluster is provisioned. The automation supports only one SSH key that can be attached to the bastion and bootstrap nodes. If you do not have an SSH key in your {{site.data.keyword.cloud_notm}} account, create one by using the [SSH keys](/docs/vpc?topic=vpc-ssh-keys) instructions. | Yes | None |
| `bastion_osimage_name` | Name of the image that is used to provision the bastion node for the {{site.data.keyword.scale_short}} cluster. Select the latest Ubuntu stock image version. | No | ibm-ubuntu-20-04-3-minimal-amd64-2 |
| `bastion_vsi_profile` | The virtual server instance profile type name used to create the bastion node. For more information, see [Instance profiles](/docs/vpc?topic=vpc-profiles). | No | cx2-2x4 |
| `bootstrap_osimage_name` | Name of the custom image that you would like to use to create the bootstrap node for the {{site.data.keyword.scale_short}} cluster. | No | hpcc-scale-bootstrap-v1 |
| `bootstrap_vsi_profile` | The virtual server instance profile type name used to create the bootstrap node. For more information, see [Instance profiles](/docs/vpc?topic=vpc-profiles). | No | bx2-8x32 |
| `compute_cluster_filesystem_mountpoint` | Compute cluster (accessingCluster) file system mount point. | No | `/gpfs/fs1` |
| `compute_cluster_gui_password` | Password that is used for logging in to the compute cluster through the GUI. The password should contain a minimum of eight characters. For a strong password, use a combination of uppercase and lowercase letters, one number, and a special character. Make sure that the password doesn't contain the username. | Yes | None |
| `compute_cluster_gui_username` | GUI username to perform system management and monitoring tasks on the compute cluster. The username should be at least four characters (any combination of lowercase and uppercase letters). | Yes | None |
| `compute_cluster_key_pair` | Name of the SSH key configured in your {{site.data.keyword.cloud_notm}} account that is used to establish a connection to the compute cluster nodes. Make sure that the SSH key is present in the same resource group and region where the cluster is provisioned. The automation supports only one SSH key that can be attached to the compute nodes. If you do not have an SSH key in your {{site.data.keyword.cloud_notm}} account, create one by using the [SSH keys](/docs/vpc?topic=vpc-ssh-keys) instructions. | Yes | None |
| `compute_vsi_profile` | The virtual server instance profile type name used to create the compute cluster nodes. For more information, see [Instance profiles](/docs/vpc?topic=vpc-profiles). | No | bx2-2x8 |
| `compute_vsi_osimage_name` | Name of the custom image that you would like to use to create the compute cluster nodes for the {{site.data.keyword.scale_full}} cluster. If you'd like, you can follow the instructions for [Planning for custom images](/docs/vpc?topic=vpc-planning-custom-images) to create your own custom image. | No | hpcc-scale513-rhel79-v1 |
| `filesystem_block_size` | The file system block size. | No | 4M |
| `ibm_customer_number` | The {{site.data.keyword.IBM_notm}} Customer Number (ICN) that is used for the Bring Your Own License (BYOL) entitlement check. For more information on how to find your ICN, see [What is my {{site.data.keyword.IBM_notm}} Customer Number (ICN)?](https://www.ibm.com/support/pages/what-my-ibm-customer-number-icn){: external} | Yes | None |
| `remote_cidr_blocks` | Comma-separated list of IP addresses that can access the {{site.data.keyword.scale_short}} cluster bastion node through SSH. For security purposes, provide the public IP addresses assigned to the devices that are authorized to establish SSH connections (for example, 169.45.117.34). To fetch the IP address of the device, use https://ipv4.icanhazip.com/. | Yes | None |
| `resource_group` | Resource group name from your {{site.data.keyword.cloud_notm}} account where the VPC resources should be deployed. For more information, see [Managing resource groups](/docs/account?topic=account-rgs). | No | Default |
| `resource_prefix` | Prefix that is used to name the {{site.data.keyword.cloud_notm}} resources that are provisioned to build the {{site.data.keyword.scale_short}} cluster. Make sure that the prefix is unique since you cannot create multiple resources with the same name. The maximum length of supported characters is 64. | No | spectrum-scale |
| `storage_cluster_filesystem_mountpoint` | Storage cluster (owningCluster) file system mount point. | No | `/gpfs/fs1` |
| `storage_cluster_gui_password` | Password that is used for logging in to the storage cluster through the GUI. The password should contain a minimum of 8 characters. For a strong password, use a combination of uppercase and lowercase letters, one number, and a special character. Make sure that the password doesn't contain the username. | Yes | None |
| `storage_cluster_gui_username` | GUI username to perform system management and monitoring tasks on the storage cluster. The username should be at least four characters (any combination of lowercase and uppercase letters). | Yes | None |
| `storage_cluster_key_pair` | Name of the SSH key configured in your {{site.data.keyword.cloud_notm}} account that is used to establish a connection to the storage cluster nodes. Make sure that the SSH key is present in the same resource group and region where the cluster is provisioned. The automation supports only one SSH key that can be attached to the storage nodes. If you do not have an SSH key in your {{site.data.keyword.cloud_notm}} account, create one by using the [SSH keys](/docs/vpc?topic=vpc-ssh-keys) instructions. | Yes | None |
| `storage_vsi_profile` | Specify the virtual server instance profile type name used to create the storage nodes. For more information, see [Instance profiles](/docs/vpc?topic=vpc-profiles). | No | bx2d-32x128 |
| `storage_vsi_osimage_name` | Name of the custom image that is used to create the storage cluster nodes for the {{site.data.keyword.scale_short}} cluster. Only the default value is supported. | No | hpcc-scale513-rhel84-v1 | 
| `total_compute_cluster_instances` | Total number of compute cluster instances that you need to provision. A minimum of three nodes and a maximum of 64 nodes are supported. | No | 3 |
| `total_storage_cluster_instances` | Total number of storage cluster instances that you need to provision. A minimum of three nodes and a maximum of 18 nodes are supported. | No | 4 |
| `vpc_availability_zones` | {{site.data.keyword.cloud_notm}} availability zone names within the selected region where the {{site.data.keyword.scale_short}} cluster should be deployed. For more information, see [Region and data center locations for resource deployment](/docs/overview?topic=overview-locations). | Yes | None |
| `vpc_cidr_block` | {{site.data.keyword.vpc_short}} address prefixes that are needed for VPC creation. Since the automation supports only single availability zones, provide one CIDR address prefix for VPC creation. For more information, see [Bring your own subnet](/docs/vpc?topic=vpc-configuring-address-prefixes). | No | 10.241.0.0/18 |
| `vpc_compute_cluster_dns_domain` | {{site.data.keyword.dns_full_notm}} domain name to be used for the compute cluster. | No | compscale.com |
| `vpc_compute_cluster_private_subnets_cidr_blocks` | The CIDR block that's required for the creation of the compute cluster private subnet. Modify the CIDR block if it has already been reserved or used for other applications within the VPC or conflicts with any on-premises CIDR blocks when using a hybrid environment. Provide only one CIDR block for the creation of the compute subnet. | No | 10.241.0.0/24 |
| `vpc_region` | Name of the {{site.data.keyword.cloud_notm}} region where the resources need to be provisioned (for example, us-east, us-south). For more information, see [Region and data center locations for resource deployment](/docs/overview?topic=overview-locations). | Yes | None |
| `vpc_storage_cluster_dns-domain` | {{site.data.keyword.dns_full_notm}} domain name to be used for the storage cluster. | No | strgscale.com |
| `vpc_storage_cluster_private_subnets_cidr_blocks` | The CIDR block that's required for the creation of the storage cluster private subnet. Modify the CIDR block if it has already been reserved or used for other applications within the VPC or conflicts with any on-premises CIDR blocks when using a hybrid environment. Provide only one CIDR block for the creation of the storage subnet. | No | 10.241.1.0/24 |
{: caption="Table 1. Deployment values" caption-side="top"} 

