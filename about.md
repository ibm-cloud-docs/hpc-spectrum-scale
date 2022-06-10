---

copyright:
  years: 2022
lastupdated: "2022-05-31"

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

# About IBM Spectrum Scale
{: #about-spectrum-scale}

With {{site.data.keyword.scale_full}}, you can deploy high-performance computing (HPC) clusters by using {{site.data.keyword.scale_full_notm}} as the storage solution. This offering uses open source Terraform-based automation to provision and configure {{site.data.keyword.cloud}} resources. With simple steps to define configuration properties and the use of automated deployment, you can build your own storage-rich clusters in minutes. {{site.data.keyword.scale_full}} enables configuration for compute nodes and storage nodes to build a complete end to end working HPC cluster. The offering uses a bootstrap node where actual provisioning of compute and storage nodes and installation and configuration of {{site.data.keyword.scale_short}} takes place. The top-level Terraform code deploys the bootstrap node and starts subprocesses to trigger the secondary layer of Terraform code for actual deployment of cluster components.
{: shortdesc}

The bootstrap node (Ansible Controller Node in the [architecture diagram](/docs/hpc-spectrum-scale?topic=hpc-spectrum-scale-about-spectrum-scale&interface=ui#architecture-diagram)) performs the deployment and configuration of the compute and storage cluster resources. A custom image (see `bootstrap_osimage_name` in [Deployment values](/docs/hpc-spectrum-scale?topic=hpc-spectrum-scale-deployment-values)) is provided as part of this solution and it contains all of the automation scripts and packages that are needed for the bootstrap node. The bootstrap node is critical during the entire lifetime of this cluster. For example, you need this node for future actions like cleaning up resources. And if you need to troubleshoot issues with the cluster deployment, the bootstrap node is needed because it contains the Terraform logs for the compute and storage cluster creation. The bootstrap node should not be deleted until the cluster is no longer required.

The default VPC instance profile for the bootstrap node has been selected based on the performance of the Ansible scripts that are triggered to deploy the compute and storage cluster resources in parallel. If you choose a smaller VPC instance profile, the deployment time might be longer. 

The offering supports the bring-your-own-license (BYOL) model for {{site.data.keyword.scale_full_notm}} to deploy an HPC cluster on {{site.data.keyword.cloud_notm}}. Make sure that you have sufficient software licenses to deploy the required capacity on the {{site.data.keyword.cloud_notm}} cluster. Contact your {{site.data.keyword.cloud_notm}} sales or support team for evaluation licenses.

{{site.data.keyword.scale_short}} enables all three interfaces: UI, API, and CLI. To use the API and CLI interfaces, the Terraform-based automation code is available in this [public GitHub repository](https://github.com/IBM/ibm-spectrum-scale-ibm-cloud-schematics){: external}.

The offering enables the initial {{site.data.keyword.scale_short}}-based HPC cluster creation. Any updates that are needed post-deployment regarding {{site.data.keyword.scale_short}} configuration or setup should be performed by using {{site.data.keyword.scale_short}} tools and commands. If you use the {{site.data.keyword.bpshort}} interface to make changes to configuration properties and reapply those changes, you can cause disruptions to the running {{site.data.keyword.scale_short}} cluster. Restoring it back to a working state might not be easy.
{: important}

## Architecture diagram
{: #architecture-diagram}

![Architecture diagram](images/hpccluster_scale_scratch_architecture.svg){:caption="Figure 1. Architecture diagram of a {{site.data.keyword.scale_full_notm}} cluster on {{site.data.keyword.cloud_notm}}" caption-side="bottom"}

