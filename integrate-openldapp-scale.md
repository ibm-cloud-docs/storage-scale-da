---

copyright:
  years: 2025
lastupdated: "2025-08-24"

keywords:

subcollection: storage-scale-da

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

# Integrating the OpenLDAP server with your IBM Storage Scale cluster
{: #integrating-openldap}

You can enable the OpenLDAP in your {{site.data.keyword.scale_full_notm}} cluster [during deployment](/docs/storage-scale-da?topic=storage-scale-da-deployment-values) by setting the deployment values for the variables: `enable_ldap`,`ldap_basedns`, `ldap_server`, `ldap_server_cert`, `ldap_admin_password`, `ldap_user_name`, `ldap_user_password`, and `ldap_instance`. If you do not have an existing LDAP server, the deployment process creates one for you and connects it to the {{site.data.keyword.scale_full_notm}} cluster.

|LDAP Variable	|Description	|Example value |
|----------|----------|----------|
|`enable_ldap`| Set this option to true to enable LDAP for IBM Storage Scale, with the default value set to false. | false |
|`ldap_basedns`	| The dns domain name is used for configuring the LDAP server. If an LDAP server is already in existence, ensure to provide the associated DNS domain name. |`ldapscale.com`|
| `ldap_server` | Provide the IP address for the existing LDAP server. If no address is given, a new LDAP server will be created. | Null |
| `ldap_server_cert` | Provide the existing LDAP server certificate. This value is required if the 'ldap_server' variable is not set to null. If the certificate is not provided or is invalid, the LDAP configuration may fail. For more information on how to create or obtain the certificate, see [existing LDAP server certificate](/docs/allowlist/hpc-service?topic=hpc-service-integrating-openldap). | Null |
|`ldap_admin_password`	| Custom LDAP User for performing cluster operations. Note: Username should be between 4 to 32 characters, (any combination of lowercase and uppercase letters).[This value is ignored for an existing LDAP server]	| "" |
|`ldap_user_name`	| Custom LDAP User for performing cluster operations. Note: Username should be between 4 to 32 characters, (any combination of lowercase and uppercase letters).[This value is ignored for an existing LDAP server].	| "" |
|`ldap_user_password`	| The LDAP user password must be 8 to 20 characters long and include at least two alphabetic characters (with one uppercase and one lowercase), one numeric digit, and at least one special character from the set (!@#$%^&*()_+=-). Spaces are not allowed. The password must not contain the username for enhanced security. [This value is ignored for an existing LDAP server]. | "" |
|`ldap_instance`| Specify the list of virtual server instances to be provisioned as ldap nodes in the cluster. Each object in the list defines the instance profile (machine type), the count (number of instances), the image (OS image to use). This configuration allows you to customize the server for setting up ldap server. The profile must match a valid IBM Cloud VPC Gen2 instance profile format. For more information, see [Instance Profiles](/docs/vpc?topic=vpc-profiles&interface=ui). | [{ profile = "cx2-2x4" image  = "ibm-ubuntu-22-04-5-minimal-amd64-1" }] |
{: caption='LDAP variables'}
