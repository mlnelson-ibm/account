---



copyright:

  years: 2017, 2021
lastupdated: "2021-05-25"

keywords: resource group, account resources, users access to resource groups, create resource group

subcollection: account

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:tip: .tip}
{:note: .note}
{:ui: .ph data-hd-interface='ui'}
{:cli: .ph data-hd-interface='cli'}
{:api: .ph data-hd-interface='api'}
{:java: .ph data-hd-programlang='java'}
{:python: .ph data-hd-programlang='python'}
{:javascript: .ph data-hd-programlang='javascript'}
{:curl: .ph data-hd-programlang='curl'}
{:go: .ph data-hd-programlang='go'}

# Managing resource groups
{: #rgs}

A resource group is a way for you to organize your account resources in customizable groupings so that you can quickly assign users access to more than one resource at a time. Any account resource that is managed by using {{site.data.keyword.Bluemix}} Identity and Access Management (IAM) access control belongs to a resource group within your account. Cloud Foundry services are assigned to orgs and spaces and can't be added to a resource group.

To start managing your resource groups, in the {{site.data.keyword.cloud}} console, go to **Manage** > **Account** > **Account resources** > **Resource groups**. You can create, view, and rename your resource groups, add resources and manage access to your resource groups. You can also delete any resource group only if it doesn't contain any resources, and it isn't the default resource group. For more information about working with resource groups, see [Best practices for organizing resources and assigning access](/docs/account?topic=account-account_setup).

## Creating a resource group
{: #create_rgs}

If you have a Pay-As-You-Go or Subscription account, you can create multiple resource groups to easily manage quota and view billing usage for a set of resources. You can also group resources to make it easier for you to assign users access to more than one instance at a time.  

You must be assigned an IAM policy with the Administrator role on All Account Management services to create additional resource groups. If you have a Lite account or 30-day trial, you can't create extra resource groups, but you can rename your default resource group.

Connections between a resource group and a Cloud Foundry org or space are restricted by your quota. See [What is an alias?](/docs/account?topic=account-connect_app#what_is_alias) for more information.
{: note}

### Creating a resource group in the console 
{: #rgs_ui}
{: ui}

1. In the console, go to **Manage** > **Account** > **Account resources** > **Resource groups**.
2. Click **Create**.
3. Enter a name for your resource group. 
4. Click **Create**.

### Creating a resource group by using the CLI
{: #rgs_cli}
{: cli}

1. Log in, and select the account.

   ```
   ibmcloud login
   ```
   {:codeblock}
   
2. Create a new resource group by running [`ibmcloud resource group-create`](/docs/cli?topic=cli-ibmcloud_commands_resource#ibmcloud_resource_group_create) command. For example, the following command creates a resource group that is named `group2`:

```
ibmcloud resource group-create group2
```
{:codeblock}

## Creating a resource group by using the API
{: #rgs_api}
{: api}

If you have a Pay-As-You-Go or Subscription account, you can create multiple resource groups to easily manage quota and view billing usage for a set of resources. You can also group resources to make it easier for you to assign users access to more than one instance at a time.

You must be assigned an IAM policy with the Administrator role on All Account Management services to create additional resource groups. If you have a Lite account or 30-day trial, you can't create extra resource groups, but you can rename your default resource group.

To create a resource group, call the [{{site.data.keyword.cloud}} Resource Manager API](https://cloud.ibm.com/apidocs/resource-controller/resource-manager?code=go#create-resource-group) as shown in the following example:

```bash
curl -X POST https://resource-controller.cloud.ibm.com/v2/resource_groups -H 'Authorization: Bearer <IAM_TOKEN>'
  -H 'Content-Type: application/json' -d '{
      "account_id": "987d4cfd77b04e9b9e1a6asdcc861234",
      "name": "test"
    }'
```
{: codeblock}
{: curl}

```java
CreateResourceGroupOptions createResourceGroupOptions = new CreateResourceGroupOptions.Builder()
        .accountId(exampleUserAccountId)
        .name("ExampleGroup")
        .build();

Response<ResCreateResourceGroup> response = resourceManagerService.createResourceGroup(createResourceGroupOptions).execute();
ResCreateResourceGroup resCreateResourceGroup = response.getResult();

System.out.println(resCreateResourceGroup);
```
{: codeblock}
{: java}

```javascript
const params = {
  accountId: exampleUserAccountId,
  name: "ExampleGroup"
};

resourceManagerService.createResourceGroup(params)
  .then(res => {
    resourceGroupId = res.result.id;
    console.log(JSON.stringify(res.result, null, 2));
  })
  .catch(err => {
    console.warn(err)
  });
```
{: codeblock}
{: javascript}

```python
res_create_resource_group = resource_manager_service.create_resource_group(
  account_id=example_user_account_id,
  name='ExampleGroup',
).get_result()

print(json.dumps(res_create_resource_group, indent=2))
```
{: codeblock}
{: python}

```go
createResourceGroupOptions := resourceManagerService.NewCreateResourceGroupOptions()
createResourceGroupOptions.SetAccountID(exampleUserAccountID)
createResourceGroupOptions.SetName("ExampleGroup")

resCreateResourceGroup, response, err := resourceManagerService.CreateResourceGroup(createResourceGroupOptions)
if err != nil {
  panic(err)
}
b, _ := json.MarshalIndent(resCreateResourceGroup, "", "  ")
fmt.Println(string(b))
```
{: codeblock}
{: go}

## Renaming a resource group
{: #rename_rgs}

Your first resource group is created and named `Default` for you. You can update the name of this group or any other groups that you create.

### Renaming a resource group in the console 
{: #renaming-rgs-ui}
{: ui}

1. In the console, go to **Manage** > **Account** > **Account resources** > **Resource groups**.
2. Click the **Actions** ![List of actions icon](../icons/action-menu-icon.svg) menu, and select **Rename**.
3. Enter a unique name and click **Save**.

### Renaming a resource group by using the CLI
{: #renaming-rgs-cli}
{: cli}

1. Log in, and select the account.

   ```
   ibmcloud login
   ```
   {:codeblock}
   
2. Rename a resource group by running the [`ibmcloud resource group-update`](/docs/cli?topic=cli-ibmcloud_commands_resource#ibmcloud_resource_group_update) command. For example, the following command renames the `Default` resource group to `Admin`:

```
ibmcloud resource group-update Default [-n, --name Admin]
```
{:codeblock}

### Renaming a resource group by using the API
{: #rename_rgs-api}
{: api}

The variable `name` identifies the new name of the resource group. For more details, see the [API](https://cloud.ibm.com/apidocs/resource-controller/resource-manager#update-resource-group). 

```bash
curl -X PATCH https://resource-controller.cloud.ibm.com/v2/resource_groups/09f8c1c0742c493f80baaf7835212345 -H 'Authorization: Bearer <IAM_TOKEN>'
  -H 'Content-Type: application/json' -d '{
      "name": "test1"
    }'
```
{: codeblock}
{: curl}

```java
UpdateResourceGroupOptions updateResourceGroupOptions = new UpdateResourceGroupOptions.Builder()
  .id(resourceGroupId)
  .name("RenamedExampleGroup")
  .state("ACTIVE")
  .build();

Response<ResourceGroup> response = resourceManagerService.updateResourceGroup(updateResourceGroupOptions).execute();
ResourceGroup resourceGroup = response.getResult();

System.out.println(resourceGroup);
```
{: codeblock}
{: java}

```javascript
const params = {
  id: resourceGroupId,
  state: 'ACTIVE',
  name: 'RenamedExampleGroup'
};

resourceManagerService.updateResourceGroup(params)
  .then(res => {
    console.log(JSON.stringify(res.result, null, 2));
  })
  .catch(err => {
    console.warn(err)
  });
```
{: codeblock}
{: javascript}

```python
resource_group = resource_manager_service.update_resource_group(
  id=resource_group_id,
  name='RenamedExampleGroup',
  state='ACTIVE',
).get_result()

print(json.dumps(resource_group, indent=2))
```
{: codeblock}
{: python}

```go
updateResourceGroupOptions := resourceManagerService.NewUpdateResourceGroupOptions(
  resourceGroupID,
)
updateResourceGroupOptions.SetName("RenamedExampleGroup")
updateResourceGroupOptions.SetState("ACTIVE")

resourceGroup, response, err := resourceManagerService.UpdateResourceGroup(updateResourceGroupOptions)
if err != nil {
  panic(err)
}
b, _ := json.MarshalIndent(resourceGroup, "", "  ")
fmt.Println(string(b))
```
{: codeblock}
{: go}

## Adding resources to a resource group
{: #add_to_rgs}
{: ui}

Services that are managed with IAM belong to a resource group instead of a Cloud Foundry org or space. When you create an instance of one of these services from the catalog, you're prompted to assign the instance to a resource group. Your resource group selection at the time of creating the instance is final and can't be changed.

Users in your account must be assigned two access policies to create resources from the catalog and add them to a resource group:

* A policy with viewer role or higher on the resources group itself
* A policy with editor role or higher on the service in the account

To add the resources to a resource group, complete the following steps: 
1. In the console, go to **Manage** > **Account** > **Account resources** > **Resource groups**.
2. Click the **Actions** ![List of actions icon](../icons/action-menu-icon.svg) menu, and select **Add resources**.
3. From here, you are directed to the catalog. You can search the offerings or filter based on a specific category, provider, pricing plan, type of compliance, or release type. Examples of resources include apps, service instances, container clusters, storage volumes, virtual servers, and software.

## Adding resources to a resource group by using the API
{: #add_to_rgs_api}
{: api}

See [Managing catalog settings](/docs/account?topic=account-filter-account#set-public-visibility-api) to set the visibility of the {{site.data.keyword.cloud}} catalog and to control access to products in the public catalog and private catalogs for users in your account.

## Viewing resources in a resource group
{: #view_rg_resources}
{: ui}

To easily view the resources that are assigned to a resource group, go to the **Menu** icon ![Menu icon](../icons/icon_hamburger.svg) > **My resources**. Then, filter by resource group from the My resources page. 

## Viewing resources by using the CLI
{: #viewing-rgs-cli}
{: cli}

1. Log in, and select the account.

   ```
   ibmcloud login
   ```
   {:codeblock}
   
2. View the resources that are assigned to a specific resource group by running the [`ibmcloud resource service-instances`](/docs/cli?topic=cli-ibmcloud_commands_resource#ibmcloud_resource_service_instances) command. For example, the following command lists all the resources that are in the `Default` resource group:

```
ibmcloud resource service-instances -g Default
```
{:codeblock}

## Viewing resources in a resource group by using the API
{: #viewing-rgs-api}
{: api}

To view resources in a resource group, call the [{{site.data.keyword.cloud}} Resource Controller API](https://cloud.ibm.com/apidocs/resource-controller/resource-controller?code=go#list-resource-instances) as shown in the following example:

```bash
curl -X GET https://resource-controller.cloud.ibm.com/v2/resource_instances -H 'Authorization: Bearer <>'
```
{: codeblock}
{: curl}

```java
ListResourceInstancesOptions listResourceInstancesOptions = new ListResourceInstancesOptions.Builder()
  .name(resourceInstanceName)
  .build();

Response<ResourceInstancesList> response = service.listResourceInstances(listResourceInstancesOptions).execute();
ResourceInstancesList resourceInstancesList = response.getResult();

System.out.printf("listResourceInstances() response:\n%s\n", resourceInstancesList.toString());
```
{: codeblock}
{: java}

```javascript
const params = {
  name: resourceInstanceName,
};

resourceControllerService.listResourceInstances(params)
  .then(res => {
    console.log('listResourceInstances() response:\n' + JSON.stringify(res.result, null, 2));
  })
  .catch(err => {
    console.warn(err)
  });
```
{: codeblock}
{: javascript}

```python
resource_instances_list = resource_controller_service.list_resource_instances(
    name=resource_instance_name
).get_result()

print('\nlist_resource_instances() response:\n',
      json.dumps(resource_instances_list, indent=2))
```
{: codeblock}
{: python}

```go
listResourceInstancesOptions := resourceControllerService.NewListResourceInstancesOptions()
listResourceInstancesOptions = listResourceInstancesOptions.SetName(resourceInstanceName)

resourceInstancesList, response, err := resourceControllerService.ListResourceInstances(listResourceInstancesOptions)
if err != nil {
  panic(err)
}
b, _ := json.MarshalIndent(resourceInstancesList, "", "  ")
fmt.Printf("\nListResourceInstances() response:\n%s\n", string(b))
```
{: codeblock}
{: go}

