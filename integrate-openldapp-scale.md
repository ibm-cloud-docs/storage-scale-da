---

copyright:
  years: 2025
lastupdated: "2025-07-15"

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

You can enable OpenLDAP with your {{site.data.keyword.scale_full_notm}} cluster [during deployment](/docs/storage-scale-da?topic=storage-scale-da-deployment-values) by setting the `enable_ldap`,`ldap_basedns`, `ldap_server`, `ldap_server_cert`, `ldap_admin_password`, `ldap_user_name`, `ldap_user_password`, and `ldap_instance` deployment input values. If you do not have an existing LDAP server, the deployment process creates one for you and connects it to the {{site.data.keyword.scale_full_notm}} cluster.

|LDAP Variable	|Description	|Example value |
|----------|----------|----------|
|`enable_ldap`|Set this option to true to enable LDAP for IBM Cloud Storage Scale, with the default value set to false.|true |
|`ldap_basedns`	|The dns domain name is used for configuring the LDAP server. If an LDAP server is already in existence, ensure to provide the associated DNS domain name.|`ldapscale.com`|
| `ldap_server` | Provide the IP address for the existing LDAP server. If no address is given, a new LDAP server will be created. | Null |
| `ldap_server_cert` | Provide the existing LDAP server certificate. This value is required if the 'ldap_server' variable is not set to null. If the certificate is not provided or is invalid, the LDAP configuration may fail. | Null |
|`ldap_admin_password`	|The LDAP administrative password should be 8 to 20 characters long, with a mix of at least three alphabetic characters, including one uppercase and one lowercase letter. It must also include two numerical digits and at least one special character from (~@_+:) are required. It is important to avoid including the username in the password for enhanced security.	|`xxxxxx`|
|`ldap_user_name`	|Custom LDAP User for performing cluster operations. Note: Username should be between 4 to 32 characters, (any combination of lowercase and uppercase letters).[This value is ignored for an existing LDAP server]	|`scaleuser`|
|`ldap_user_password`	|The LDAP user password should be 8 to 20 characters long, with a mix of at least three alphabetic characters, including one uppercase and one lowercase letter. It must also include two numerical digits and at least one special character from (~@_+:) are required.It is important to avoid including the username in the password for enhanced security.[This value is ignored for an existing LDAP server].|`xxxxxx`|
|`ldap_instance`|Profile and Image name to be used for provisioning the LDAP instances. Note: Debian based OS are only supported for the LDAP feature.| [{ profile = "cx2-2x4" image = "ibm-ubuntu-22-04-5-minimal-amd64-1" }]|
{: caption='LDAP variables'}
