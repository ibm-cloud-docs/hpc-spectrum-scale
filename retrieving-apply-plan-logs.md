---

copyright:
  years: 2022
lastupdated: "2022-06-03"

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

# Retrieving apply plan logs
{: #retrieve-apply-plan-logs}

You can retrieve the logs from either the Schematics workspace or the bootstrap node to view both successful and failed cluster deployments.
{: shortdesc}

## Retrieving apply plan logs in Schematics workspace
{: #retrieve-apply-plan-logs-schematics-workspace}

After you apply a plan, a new log file is generated, which can be viewed in the _Jobs_ tab in the Schematics workspace. See the following sections for instructions and examples of successful or failed deployments.

### Successful cluster deployment
{: #successful-apply-plan}

1. In the _Jobs_ tab in the Schematics workspace, select the job and expand the log file.
2. If the job was successful in creating all of the resources that are part of the deployment, then your workspace goes to an active state.
3. Use the SSH command in the output of your apply plan log to log in as `vpcuser` for the bootstrap, compute, and storage node through the bastion host as `ubuntu` user. See the following sample response of a successful deployment:

**Sample response**

```
2022/05/09 14:35:53 Terraform apply | Apply complete! Resources: 41 added, 0 changed, 0 destroyed.
2022/05/09 14:35:53 Terraform apply |
2022/05/09 14:35:53 Terraform apply | Outputs:
2022/05/09 14:35:53 Terraform apply |
2022/05/09 14:35:53 Terraform apply | ssh_command = "ssh -J ubuntu@141.125.161.0 vpcuser@10.241.1.5"
2022/05/09 14:35:53 Command finished successfully.
```
{: screen}

### Failed cluster deployment
{: #failed-apply-plan}

1. In the _Jobs_ tab in the Schematics workspace, select the job and expand the log file for a better view.
2. If the job fails to create any of the resources that are a part of the deployment, then your workspace goes to a failed state. Deployment might error out if any of the deployment values are incorrect or if there are any issues at the infrastructure level. 
3. Fix the errors, and then click **Apply plan** again. See the following sample response of a failed deployment:

**Sample response**

```
2022/05/09 12:51:12 Terraform plan | Error: [ERROR] No SSH Key found with name ssh-key-east-new
2022/05/09 12:51:12 Terraform plan |
2022/05/09 12:51:12 Terraform plan | on main.tf line 85, in data "ibm_is_ssh_key" "compute_ssh_key":
2022/05/09 12:51:12 Terraform plan | 85: data "ibm_is_ssh_key" "compute_ssh_key" { # This block is trying to fetch the key_pair details for compute node, which is dependent on the code present on public repo
2022/05/09 12:51:12 Terraform plan |
2022/05/09 12:51:12 Terraform plan |
2022/05/09 12:51:12 Terraform plan |
2022/05/09 12:51:12 Terraform plan | Error: [ERROR] No SSH Key found with name ssh-key-east-new
2022/05/09 12:51:12 Terraform plan |
2022/05/09 12:51:12 Terraform plan | on main.tf line 89, in data "ibm_is_ssh_key" "storage_ssh_key":
2022/05/09 12:51:12 Terraform plan | 89: data "ibm_is_ssh_key" "storage_ssh_key" { # This block is trying to fetch the key_pair details for compute node, which is dependent on the code present on public repo
2022/05/09 12:51:12 Terraform plan |
2022/05/09 12:51:12 Terraform plan |
2022/05/09 12:51:12 �[1m�[31mTerraform PLAN error: Terraform PLAN errorexit status 1�[39m�[0m
2022/05/09 12:51:12 �[1m�[31mCould not execute job: Error : Terraform PLAN errorexit status 1�[39m�[0m
```
{: screen}

## Retrieving apply plan logs from bootstrap node
{: #retrieve-apply-plan-logs-bootstrap-node}

You can view both successful and failed logs from the bootstrap node. Complete the following steps to view the logs.

1. Log in to the bootstrap node by using the following command:

    ```
    ssh -J ubuntu@<IP_address_bastion_host> vpcuser@<IP_address_bootstrap_node>
    ```
    {: pre}

2. After you log in to the bootstrap node, run the following command to gain root user access:

    ```
    sudo su
    ```
    {: pre}

3. Change the directory to `/var/log` by running the following command:

    ```
    cd /var/log
    ```
    {: pre}

4. For a complete stack deployment log, run the following command:

    ```
    cat stack_deployment.log
    ```
    {: pre}

### Successful cluster deployment
{: #success-cluster-deployment}

If the cluster deployment is successful, a log message of "Uploading the success status file" is created and signal status is set as Success in the Terraform directory. See the following sample response of a successful deployment:

```
2022-05-09 14:35:48,968 - INFO - PLAY RECAP *********************************************************************

2022-05-09 14:35:48,968 - INFO - 10.241.1.7 : ok=3 changed=1 unreachable=0 failed=0 skipped=1 rescued=0 ignored=0

2022-05-09 14:35:48,968 - INFO - 10.241.1.8 : ok=3 changed=1 unreachable=0 failed=0 skipped=1 rescued=0 ignored=0

2022-05-09 14:35:48,968 - INFO - 10.241.1.9 : ok=3 changed=1 unreachable=0 failed=0 skipped=1 rescued=0 ignored=0

2022-05-09 14:35:48,968 - INFO -

2022-05-09 14:35:49,118 - INFO - [CLOUD-DEPLOY] Uploading the success status file.
2022-05-09 14:35:49,119 - INFO - [CLOUD-DEPLOY] Uploaded the success status file successfully.
```
{: screen}

### Failed cluster deployment
{: #failed-cluster-deployment}

If you see the log status "INFO -| Error", the cluster deployment failed at that particular point, and you need to debug the issue. You will also see a message at the end of the response: "Uploading the failure status file". See the following sample response of a failed deployment:

```
2022-05-10 09:35:55,204 - INFO - │ “Result”: {
2022-05-10 09:35:55,204 - INFO - │ “errors”: [
2022-05-10 09:35:55,204 - INFO - │ {
2022-05-10 09:35:55,204 - INFO - │ “code”: “not_authorized”,
2022-05-10 09:35:55,204 - INFO - │ “message”: “the provided token is not authorized to list keys in this account”
2022-05-10 09:35:55,204 - INFO - │ }
2022-05-10 09:35:55,205 - INFO - │ ],
2022-05-10 09:35:55,205 - INFO - │ “trace”: “70a2a6d5-48a5-4f08-8055-4fd21529dd1a”
2022-05-10 09:35:55,205 - INFO - │ },
2022-05-10 09:35:55,205 - INFO - │ “RawResult”: null
2022-05-10 09:35:55,205 - INFO - │ }
2022-05-10 09:35:55,205 - INFO - │
2022-05-10 09:35:55,205 - INFO - │
2022-05-10 09:35:55,205 - INFO - │ with data.ibm_is_ssh_key.storage_ssh_key,
2022-05-10 09:35:55,205 - INFO - │ on main.tf line 171, in data “ibm_is_ssh_key” “storage_ssh_key”:
2022-05-10 09:35:55,205 - INFO - │ 171: data “ibm_is_ssh_key” “storage_ssh_key” {
2022-05-10 09:35:55,205 - INFO - │
2022-05-10 09:35:55,205 - INFO - ╵
2022-05-10 09:35:55,191 - INFO - │ Error: the provided token is not authorized to view the specified instance (ID:*) in this account
2022-05-10 09:35:55,210 - INFO - [CLOUD-DEPLOY] Uploading the failure status file.
2022-05-10 09:35:55,210 - INFO - [CLOUD-DEPLOY] Uploaded the failure status file successfully.
```
{: screen}

