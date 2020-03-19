
## Testing
#### Azure Portal UI testing
The new process for v2 ui definitions is a more streamlined process.  

* Navigate to the UI Sandbox -> http://aka.ms/createuidef/sandbox
* Paste in your UI definition.  In confidential compute case, this is located [here](marketplace/createUiDefinition.json)
* Hit Preview
* The various controls that can be added are available on the left navigation as well in the Sandbox

#### Azure Template testing
To test the underlying ARM templates, scripts and resoures (pre-staging) follow this process.
* Download and install Azure Storage Explorer -> https://azure.microsoft.com/en-us/features/storage-explorer/ (skip this if you're on linux and just use the UI - the azure initial setup has a template that creates the container ready for you to use)
* Create a storage account in the Azure subscription where you would like to test the template -> https://docs.microsoft.com/en-us/azure/storage/common/storage-quickstart-create-account?tabs=azure-portal
  * You can use Standard Performance, StorageV2 (general purpose), with LRS (no replication), and Hot tier for settings for testing
* Create a storage container in this new storage account by:
  * Opening the storage explorer client, login to your Azure subscription, navigate to the storage account in the tree and under this the Blob Containers node.  Right click this node and select create blob container.
  * After creating the container, right click on it and select Set Public Access level.  Set this to `Public read access for blobs and containers`
* Upload the contents of the _quickstart_ directory to new storage container. `NOTE: you only need the 'nestedtemplates' & 'scripts' folder with the folder structure.`
```bash

    templates
    ├── quickstart
    │   ├── nested
    │   │   ├── VMExtension.json
    │   │   └── VM.json
    │   └── scripts
    │       └── config.sh

```
Then set  (in the last step below) _artifactsLocation = "https://<your_account_store>.blob.core.windows.net/templates/quickstart/"

* Now navigate to the [Azure portal](https://portal.azure.com), click `+ Create a resource` in the upper left corner.
* Search for `Template deployment (deploy using custom templates)` and click Create.
* Click on `Build your own template in the editor`
* Remove the contents (json) in the editor and paste in the contents of `quickstart\mainTemplate.json`
* Click Save
* The template will be parsed and a rough UI will be shown to allow you to input parameters to provision and test these templates.  A few items to note on parameters.
  * _artifacts Location Sas Token in NOT required for testing deployments (this is used by Azure in production/live)
  * _artifacts Location **should be set to the base path to your storage container**.  You can easily get this by going back to the storage explorer tool, selecting your storage container and in the lower left copying the value from Properties in the lower left corner (URL)
  


## Submitting to the Marketplace:
1. Go to https://cloudpartner.azure.com/
2. New Offer -> Azure Application, fill in the details
3. SKU.package - Create the artifact make a zip file of:
 - nestedtemplates/
 - scripts/
 - createUiDefinition.json
 - mainTemplate.json
4. MarketPlace.Description uses html in that field - Use this [site](https://html5-editor.net/) to format and then paste that in - otherwise its just plain text with no line breaks etc
5. MarketPlace.Lead Management uses a variety of options:
    - if using Salesforce, get a 'salesforce object identifier'
    - if using Azure Tables follow the steps [here](https://docs.microsoft.com/en-gb/azure/marketplace/cloud-partner-portal-orig/cloud-partner-portal-lead-management-instructions-azure-table)
6. MarketPlace.Marketing Artifacts - use the logos in the assets folder at the root level
8. Support.Engineering Contact - `<enter email/contact>`
9. Support.Customer Support - `<enter email/contact>`
10. Support.URLs - `<enter support url>`
 
 
 
