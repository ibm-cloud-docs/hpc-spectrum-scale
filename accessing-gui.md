---

copyright:
  years: 2022
lastupdated: "2022-05-19"

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
{:step: data-tutorial-type='step'}
{:table: .aria-labeledby="caption"}

# Accessing the GUI
{: #access-gui}

After the cluster setup is done, you can monitor the resources and status of the service directly from the {{site.data.keyword.scale_full_notm}} GUI for both the compute and storage clusters. For more information about the GUI, see [{{site.data.keyword.scale_full_notm}} GUI](https://www.ibm.com/docs/en/spectrum-scale/5.1.3?topic=reference-spectrum-scale-gui){: external}.
{: shortdesc}

## Before you begin
{: #before-you-begin}

Before you begin accessing the {{site.data.keyword.scale_short}} GUI, review the following considerations and requirements:

* The initial setup must be done from your local machine.
* Provide the SSH key path from the local machine that's used to configure the compute and storage nodes.
* It's recommended to use the Safari browser to access the GUI.
* If you encounter slowness in loading or accessing the GUI, clear the browser's cache.
* You cannot open both the compute and storage GUIs on the same port 22443. Use a different port or close one of the GUIs so you can access the other GUI cluster.

## Setting up access
{: #setting-up-access}

1. Open a new command line terminal.
2. Run the following commands from the local machine:

    ```
    eval `ssh-agent`
    ssh-add -k <path_of_region_specific_key>
    ssh -A -L 22443:<GUI_node_IP>:443 -N ubuntu@<bastion_host_IP>
    ```
    {: codeblock}

3. Open the browser on the local machine, and run https://localhost:22443. 

## Fetching `GUI_node_IP`
{: #fetching-gui-node-IP}

1. Use the `ssh_command` from the output section to log in to the bootstrap node.
2. Run the following command:

    ```
    mmcloudworkflows cluster info
    ```
    {: pre}

3. If "Y" is enabled on the GUI section from the response, that is the IP address where the GUI service is installed. See the following example response:

    ```
    Spectrum Scale Storage Cluster
    |-------------------------------------------|---------------|--------|-----|
    |                Instance Id                |   Private IP  | Quorum | GUI |
    |-------------------------------------------|---------------|--------|-----|
    | 02c7_a2c7b819-2bc8-45e7-b0f0-1eec88248dbe |   10.241.1.7  |   Y    |  Y  |
    | 02c7_aadf7e5d-1c8e-4d17-b53a-a7cfa4dd916b |   10.241.1.8  |   Y    |     |
    | 02c7_d4606496-5d69-486b-bbbc-dc26caf81d0c |   10.241.1.9  |   Y    |     |
    |-------------------------------------------|---------------|--------|-----|
 
    Admin Node: 10.241.1.7
 
    Spectrum Scale Compute Cluster
    |-------------------------------------------|---------------|--------|-----|
    |                Instance Id                |   Private IP  | Quorum | GUI |
    |-------------------------------------------|---------------|--------|-----|
    | 02c7_d5701fa0-4d16-4a9c-acc9-658c2defe343 |   10.241.0.7  |   Y    |     |
    | 02c7_048ca65d-1cb8-461e-9d61-1e11ef5d8a8a |   10.241.0.6  |   Y    |     |
    | 02c7_f8de4760-b762-4b18-b3a7-533eb0bfab14 |   10.241.0.5  |   Y    |  Y  |
    |-------------------------------------------|---------------|--------|-----|
 
    Admin Node: 10.241.0.5
    ```
    {: screen}

