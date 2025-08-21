---

copyright:
  years: 2025
lastupdated: "2025-08-21"

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

# Integrating an existing OpenLDAP server with your IBM Storage Scale cluster
{: #integrating-existing-openldap}

If you already have an existing LDAP server with a certificate, then you can enable OpenLDAP with your {{site.data.keyword.scale_full_notm}} cluster [during deployment](/docs/storage-scale-da?topic=storage-scale-da-deployment-values) by setting the  `enable_ldap`,`ldap_basedns`, `ldap_server`, `ldap_server_cert`, `ldap_admin_password`, `ldap_user_name`, `ldap_user_password`, and `ldap_instance` deployment input values. If you do not have an existing LDAP server and certificate, the deployment process creates one for you and connects it to the IBM Cloud Scale cluster.
{: shortdesc}

If your existing LDAP server does not have a certificate, follow the steps mentioned in [Creating and Configuring an LDAP certificate with your LDAP server](/docs/storage-scale-da?topic=storage-scale-da-config-ldap-ces#create-configure-ldap-certificate) section.

Before you deploy the IBM Storage Scale cluster with the LDAP input values, complete the following LDAP requirements:

1. OpenLDAP version 2.4 or later is installed and configured.
2. The OpenLDAP server can communicate with the {{site.data.keyword.scale_full_notm}} cluster nodes over the network. Configure the network settings on both the OpenLDAP server and the {{site.data.keyword.scale_full_notm}} cluster nodes.
3. The OpenLDAP server and the {{site.data.keyword.scale_full_notm}} cluster nodes can communicate over port 389.
4. [Create and configure an LDAP certificate](/docs/storage-scale-da?topic=storage-scale-da-config-ldap-ces#create-configure-ldap-certificate), if not present.

|LDAP Variable	|Description	|Example value |
|----------|----------|----------|
|`enable_ldap`|Set this option to true to enable LDAP for IBM Cloud Storage Scale, with the default value set to false.|true |
|`ldap_basedns`	|The dns domain name is used for configuring the LDAP server. If an LDAP server is already in existence, ensure to provide the associated DNS domain name.|`ldapscale.com`|
| `ldap_server` | Provide the IP address for the existing LDAP server. If no address is given, a new LDAP server will be created. | `xxxxxx` |
| `ldap_server_cert` | Provide the existing LDAP server certificate. This value is required if the 'ldap_server' variable is not set to null. If the certificate is not provided or is invalid, the LDAP configuration may fail. | `xxxxxx` |
|`ldap_admin_password`	|The LDAP administrative password should be 8 to 20 characters long, with a mix of at least three alphabetic characters, including one uppercase and one lowercase letter. It must also include two numerical digits and at least one special character from (~@_+:) are required. It is important to avoid including the username in the password for enhanced security.	|`xxxxxx`|
|`ldap_user_name`	|Custom LDAP User for performing cluster operations. Note: Username should be between 4 to 32 characters, (any combination of lowercase and uppercase letters).[This value is ignored for an existing LDAP server]	|`scaleuser`|
|`ldap_user_password`	|The LDAP user password should be 8 to 20 characters long, with a mix of at least three alphabetic characters, including one uppercase and one lowercase letter. It must also include two numerical digits and at least one special character from (~@_+:) are required.It is important to avoid including the username in the password for enhanced security.[This value is ignored for an existing LDAP server].|`xxxxxx`|
|`ldap_instance`|Profile and Image name to be used for provisioning the LDAP instances. Note: Debian based OS are only supported for the LDAP feature.| [{ profile = "cx2-2x4" image = "ibm-ubuntu-22-04-5-minimal-amd64-1" }]|
{: caption='LDAP variables'}

Also, always allow access to the CIDR ranges for the VPC that the {{site.data.keyword.scale_full_notm}} cluster deployment creates. Make sure that the security groups for the existing LDAP server are allowlisted with the VPC CIDR range of newly created VPC. This way, the new VPC can connect to the existing your existing OpenLDAP server and that all management and login nodes can access your LDAP server.

If the security groups of the LDAP server are updated with the VPC CIDR ranges, you see a message similar to:

```text
null_resource.validate_ldap_server_connection[0] (remote-exec): The connection to the existing LDAP server 10.241.0.5 was successfully established.
```
{: codeblock}


However, if the connection to the existing LDAP server is not established, you see a message similar to:

```text
│ Error: remote-exec provisioner error
│ with null_resource.validate_ldap_server_connection[0], on main.tf line 355, in resource "null_resource" "validate_ldap_server_connection":
│ 355:   provisioner "remote-exec"
│ error executing "/tmp/terraform_888134906.sh": Process exited with status 1
```
{: codeblock}
