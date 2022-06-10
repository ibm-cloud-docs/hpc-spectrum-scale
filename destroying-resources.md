---

copyright:
  years: 2022
lastupdated: "2022-05-16"

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
{:ui: .ph data-hd-interface='ui'}
{:cli: .ph data-hd-interface='cli'}
{:api: .ph data-hd-interface='api'}
{:table: .aria-labeledby="caption"}

# Destroying resources
{: #deleting-resources}

If you want to destroy the {{site.data.keyword.scale_short}} cluster and all of its associated VPC resources, you can remove them from your {{site.data.keyword.cloud}} account. First, you need to destroy the associated resources (storage and compute resources and security groups) from the bootstrap node, and then you need to destroy them by using your interface of choice.

Make sure that you delete all of the {{site.data.keyword.scale_short}} cluster resources that are deployed through the controller node. Failure to do so will delete the controller node when {{site.data.keyword.bpshort}} performs clean up of all the deployed resources, but all the cluster resources would be left behind.
{: important}

## Step 1. Destroying resources from bootstrap node
{: #destroy-resources-bootstrap-node}

1. To access the bootstrap node, use the `ssh_command` from the output of your apply plan log.
2. After you log in to the bootstrap node, run the following command to gain root user access:

   ```
   sudo su
   ```
   {: pre}

3. Run the following command to destroy the {{site.data.keyword.scale_short}} cluster's associated resources:

   ```
   mmcloudworkflows cluster destroy ibmcloud
   ```
   {: pre}

4. See the following sample response:

   ```
   [root@spectrum-scale-bootstrap vpcuser]# mmcloudworkflows cluster destroy ibmcloud
   2022-05-11 06:50:43,944 - INFO - Logging in to file: /var/adm/ras/ibm_cloud_workflow_logs/mm_cloud_workflow_teardown.log_2022-May-11_06-50-43

    =======================================================================
    |                          ! Danger Zone !                            |
    ======================================================================|
    |  This workflow, will result in teardown of IBM Spectrum Scale       |
    |  cluster and resources. However, it will not destroy VPC, Bastion   |
    |  Host resources and the s3 bucket.                                  |
    |                                                                     |
    |  Notes:                                                             |
    |  1. Ensure to STOP all your applications before proceeding further. |
    |  2. All IBM Spectrum Scale instances must be in either 'running',   |
    |     'pending', 'stopping' or 'stopped' state.                       |
    =======================================================================
        
   Do you want to continue teardown [y/N]: y
   2022-05-11 06:50:45,525 - INFO - Proceeding for tear down ..
   2022-05-11 06:50:45,525 - INFO - Obtaining necessary permissions to destroy cluster resources
   2022-05-11 06:50:46,162 - INFO - Proceeding to destroy the IBM Spectrum Scale cluster
   2022-05-11 06:50:46,162 - INFO - This may take a few minutes to complete.
   [root@spectrum-scale-bootstrap vpcuser]#
   ```
   {: screen}

Destroying resources takes time based on the number of instance values that you provided for the deployment values.
{: note}

## Step 2. Destroying resources using the UI
{: #destroy-resources-ui}
{: ui}

1. In the {{site.data.keyword.cloud_notm}} console on the **Schematics > Workspaces** page, select **Actions > Destroy** to delete all of the related VPC resources that were deployed as part of that workspace.
2. If you select the option to destroy resources, decide whether you want to destroy all of them. This action cannot be undone. 
3. Confirm the action by entering the workspace name in the text box and click **Destroy**.

## Step 2. Destroying resources using the CLI
{: #deleting-resources-cli}
{: cli}

Run the following command to remove your VPC resources from your workspace:

```
ibmcloud schematics destroy --id <WORKSPACE_ID>
```
{: pre}

You can monitor the log files to view the deletion progress of all {{site.data.keyword.cloud_notm}} resources.
{: note}

## Step 2. Destroying resources using Schematics Python API
{: #deleting-resources-api}
{: api}

1. To destroy resources by using the {{site.data.keyword.bplong_notm}} Python APIs, create a Python file and provide a name of your choice, for example, `schematics_destroy_resources.py`.
2. Copy and paste the [Destroy resources using Schematics Python API](/docs/hpc-spectrum-scale?topic=hpc-spectrum-scale-deleting-resources&interface=api#example-request-destroy) example request to your Python file.
3. Change the following parameters as part of the request:
  * Replace your {{site.data.keyword.cloud_notm}} key to the `authenticator = IAMAuthenticator('<ibm-api-key>')` variable.
  * Change the API endpoint to the endpoint mentioned in [API endpoints](https://cloud.ibm.com/apidocs/schematics?code=python#api-endpoints){: external} according to the location that you want your {{site.data.keyword.bpshort}} workspace to reside, for example, `schematics_service.set_service_url('https://us.schematics.cloud.ibm.com')`.
4. Inside the `schematics_service.destroy_workspace_command` function, provide the following parameters:
  * Provide the workspace ID that you generated in the [Creating a workspace](/docs/hpc-spectrum-scale?topic=hpc-spectrum-scale-creating-workspace&interface=api) task, for example, `us-south.workspace.Terraform-Schematics-Python-Workspace.b3bbc9f5`.
  * Export your {{site.data.keyword.cloud_notm}} API key by using the following command:
  
    ```
    export IBMCLOUD_API_KEY =”<ibm-cloud-api-key>” 
    ```
    {: pre}
  
  * Run the following curl command to create a refresh token:

    ```
    curl -X POST "https://iam.cloud.ibm.com/identity/token" -H "Content-Type: application/x-www-form-urlencoded" -d "grant_type=urn:ibm:params:oauth:grant-type:apikey&apikey=$IBMCLOUD_API_KEY" -u bx:bx
    ```
    {: pre}

5. Run the Python script by using `python3 <python-file-name>` to destroy the resources that you created in the {{site.data.keyword.bpshort}} workspace in the {{site.data.keyword.cloud_notm}}.
6. You get an activity ID in the response if the parameters passed as part of the request are valid. You should be able to see that the resources are deleted in the {{site.data.keyword.bpshort}} workspace that you created in the {{site.data.keyword.cloud_notm}} console. If you don’t get a successful response, the error response contains the errors that you need to resolve. Resolve those errors and run the script until you are able to get a valid response and destroy the resources.
7. If you want to check the logs of the action, see [Retrieving action logs with {{site.data.keyword.bpshort}} API](/docs/hpc-spectrum-scale?topic=hpc-spectrum-scale-retrieve-action-logs) to retrieve the logs. The response contains the status of the action, and it appears in either a `COMPLETED` or `FAILED` state.

### Example Python request
{: #example-request-destroy}
{: api}

```python

# Destroy resources using Schematics Python API

import json, logging

from ibm_cloud_sdk_core.authenticators import IAMAuthenticator
from ibm_schematics.schematics_v1 import SchematicsV1

logging.basicConfig()
logging.root.setLevel(logging.NOTSET)
logging.basicConfig(level=logging.NOTSET)

authenticator = IAMAuthenticator('<ibmcloud-api-key>')

schematics_service = SchematicsV1(authenticator = authenticator)

schematics_service.set_service_url('https://us.schematics.cloud.ibm.com')

logging.info("Started Deletion of Schematic Resources")

workspace_activity_destroy_result = schematics_service.destroy_workspace_command(
    w_id='<workspace-id>',
    refresh_token='<refresh-token>'
).get_result()

print(json.dumps(workspace_activity_destroy_result, indent=2))

logging.info("Completed Deletion of Schematic Resources")
```
{: codeblock}

### Example Python response
{: #example-response-destroy}
{: api}

```python
INFO:root:Started Deletion of Schematic Resources
DEBUG:urllib3.connectionpool:Starting new HTTPS connection (1): iam.cloud.ibm.com:443
DEBUG:urllib3.connectionpool:https://iam.cloud.ibm.com:443 "POST /identity/token HTTP/1.1" 200 985
DEBUG:urllib3.connectionpool:Starting new HTTPS connection (1): us.schematics.cloud.ibm.com:443
DEBUG:urllib3.connectionpool:https://us.schematics.cloud.ibm.com:443 "PUT /v1/workspaces/us-south.workspace.Schematic-Sunil-Test-Workspace.5a4cbf11/destroy HTTP/1.1" 202 49
{
  "activityid": "4bc90aac48f7cf9850bddf1ba9407eb5"
}
INFO:root:Completed Deletion of Schematic Resources
```
{: screen}