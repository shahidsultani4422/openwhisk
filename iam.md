---
copyright:
  years: 2017, 2022
lastupdated: "2022-06-29"

keywords: access policies, iam, roles, functions, platform roles, service-specific roles, service ID

subcollection: openwhisk

---

{{site.data.keyword.attribute-definition-list}}

# Setting access policies
{: #iam}

Access to {{site.data.keyword.openwhisk}} service instances for users in your account is controlled by IBM Cloud Identity and Access Management (IAM). For {{site.data.keyword.openwhisk_short}}, your {{site.data.keyword.openwhisk_short}} namespace is considered to be your service instance. Every user that accesses {{site.data.keyword.openwhisk_short}} entities in your namespace must be assigned an access policy with an IAM role defined. The policy determines what actions a user can perform within the context of the namespace that you select. The actions are then mapped to IAM user roles. If you created the namespace, then you do not need to set any IAM policies to view or work with your {{site.data.keyword.openwhisk_short}} entities. 
{: shortdesc}

{{site.data.keyword.openwhisk_short}} uses both the Platform and Service management roles. You can set policies about who can create namespaces at the platform level, and use the service roles to manage interaction with the namespaces themselves.

## How do I set IAM policies so that others can work with my namespace?
{: #iam_namespace_policies}

In order for others to work with entities in your namespace, you must to set the appropriate [IAM policies in the console](https://cloud.ibm.com/iam/overview) or [with the CLI](#cli-set). 

The minimum Platform level access is Viewer. The minimum Service level access is Reader. For more information about Platform and Service level access roles, see [Platform management roles](#iam_platform_roles) and [Service-specific roles](#service_specific_roles).

Want to learn more about IAM key concepts? Check out [the IAM overview](/docs/account?topic=account-iamoverview){: external} or the [Best practices for assigning access](/docs/account?topic=account-account_setup){: external}.
{: tip}

## How do I set IAM policies so that others can create namespaces in my account?
{: #iam_namespace_create}

To allow other users to manage {{site.data.keyword.openwhisk_short}} namespaces, including creating new namespaces, you must set the following access policies for those users.

* The user's **Platform role** must be set to Administrator. This policy applies to all resources of {{site.data.keyword.openwhisk_short}}.
* The user's **Service role**  must be set to Manager. This policy applies to all resources of {{site.data.keyword.openwhisk_short}}.

## How do I know which access policies have set for me?
{: #iam_set_policies_me}

You can see which access policies have been set for you in the [{{site.data.keyword.cloud_notm}} catalog](https://cloud.ibm.com/catalog){: external} console.

1. From the console, click **Manage** > **Access (IAM)** > **Users**. Or, navigate to `https://cloud.ibm.com/iam/users`.
2. Click your name in the user table.
3. Click the **Access policies** tab to see your access policies.


## Platform management roles
{: #iam_platform_roles}

The following table details the actions that are mapped to platform management roles. Platform management roles enable users to perform tasks on service resources at the platform level. For example, assign user access for the service, create or delete service IDs, create instances, and bind instances to applications.
{: shortdesc}

For more information about how to assign, edit, review, or delete resource access policies, see [IAM access](/docs/account?topic=account-userroles){: external}.
{: tip}

| Platform role | Description |
| -------------- | -------------- |
| Viewer | The Viewer role is a required role that allows users to work with namespaces of other accounts. You can also add the appropriate service-specific role for a namespace as described below. |
| Administrator | Administrators can create namespaces. The Administrator role is required to perform the service ID and API key lock operations during creation of the service. |
{: caption="Table 1. Platform roles" caption-side="bottom"}

## Service-specific roles
{: #service_specific_roles}

Service-specific roles determine the scope of an access policy within a specific service. For {{site.data.keyword.openwhisk_short}}, the roles can apply to a users ability to use the service, such as accessing the console or performing API calls.
{: shortdesc}

Permissions build on each other. For example, any operation that the Writer role is able to perform, the Manager role also can. However, the Manager role has additional permissions. To see the general permissions for each role, check out [Service access roles](/docs/account?topic=account-userroles#service_access_roles){: external}.

To see which roles are required to perform each operation, check out the following table.

| Action | Reader | Writer | Manager |
| -------------- | -------------- | -------------- | -------------- |
| `functions.namespaces.update` \n Update a namespace. |  |  | Yes |
| `functions.namespaces.delete` \n Delete a namespace. |   |  | Yes |
| `functions.namespaces.read` \n View the available namespaces. | Yes | Yes | Yes |
| `functions.entities.create` \n Create an entity within a Functions namespace, such as a package, action, trigger, or rule. | | Yes | Yes |
| `functions.entities.update` \n Update an entity within a Functions namespace, such as a package, action, trigger, or rule. | | Yes | Yes |
| `functions.entities.delete` \n Delete an entity from a Functions namespace, such as a package, action, trigger, or rule. | | Yes | Yes |
| `functions.entities.read` \n View the available entities within a namespace, such as package, action, trigger, or rule. | Yes | Yes | Yes |
| `functions.entities.activate` \n Activate an entity within a namespace. Activate entities, for example, by invoking an action, firing a trigger, or enabling or disabling a rule. | Yes | Yes | Yes |
{: caption="Table 2. Which roles can perform which operations?" caption-side="bottom"}


## Setting access policies for a service ID
{: #service-id-set-policy}

When you create an IAM-based namespace, it is assigned a service ID. You can set an access policy for this ID so that the API key of that service ID can be used to generate an IAM token. Then, use the token in an action code to call other services, such as {{site.data.keyword.cos_full_notm}}.
{: shortdesc}

### Setting access policies for a service ID in the console
{: #service-id-set-ui}

You can set an access policy for a service ID by using the IAM console.
{: shortdesc}

1. Open the [IAM Service ID page](https://cloud.ibm.com/iam/serviceids){: external}.
2. In the **Service IDs** list, select your {{site.data.keyword.openwhisk_short}} namespace. 
3. On the **Manage** page, click **Access policies**, then click **Assign access**.
4. Next, select an **Access Type**. You can choose from the following options. 
    * **Assign access within a resource group**: Use this option to grant your {{site.data.keyword.openwhisk_short}} service ID access to a resource group.
    * **Assign access to resources**: Use this option to grant your {{site.data.keyword.openwhisk_short}} service ID access to a specific resource, like an instance of {{site.data.keyword.cos_full_notm}}.
    * **Assign access to account management services**: Use this option to grant your {{site.data.keyword.openwhisk_short}} service ID access to account management services such as billing, user management, and more.

### Setting an access policy for your {{site.data.keyword.openwhisk_short}} service ID through the CLI
{: #cli-set}

Set an access policy for a service ID by using the CLI. 
{: shortdesc}

Copy the following command. Replace `<namespace_service_ID>` with the name of your {{site.data.keyword.openwhisk_short}} namespace. Replace `<IAM_role1,IAM_role2>` with the IAM roles you want to assign to your namespace. Replace `<other_service_name>` with the name of the {{site.data.keyword.IBM_notm}} service you want {{site.data.keyword.openwhisk_short}} to work with. Replace `<other_service_GUID>` with the GUID of the {{site.data.keyword.IBM_notm}} service instance.

```sh
ibmcloud iam service-policy-create <namespace_service_ID> --roles <IAM_role1,IAM_role2> --service-name <other_service_name> --service-instance <other_service_GUID>
```
{: pre}

| Option | Description |
| -------------- | -------------- |
| `<namespace_service_ID>` | The service ID for the namespace. To see all service IDs, run `ibmcloud iam service-ids`. |
| `<IAM_role>` | The type of IAM service access role that the action must have to use the target service. To see the supported roles for the other service, run `ibmcloud iam roles --service SERVICE_NAME`. For more information, see [IAM access roles](/docs/account?topic=account-userroles#service_access_roles). |
| `--service-name <other_service_name>` | The name of the other {{site.data.keyword.cloud_notm}} service name. |
| `--service-instance <other_service_GUID>` | The GUID of the other service instance that you want the action to have access to. To get the service instance GUID, run `ibmcloud resource service-instance <other_service_instance_name>`. |
{: caption="Table 1. Understanding the command components" caption-side="bottom"}

For more information, see the [**`service-policy-create`**](/docs/account?topic=cli-ibmcloud_commands_iam) command reference.
{: note}

Next steps
:   For more information about managing service credentials, see the [Manage service credentials for serverless applications](https://developer.ibm.com/tutorials/accessing-iam-based-services-from-ibm-cloud-functions/){: external} blog.


