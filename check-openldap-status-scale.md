---

copyright:
  years: 2025
lastupdated: "2025-08-14"

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

# Checking OpenLDAP status
{: #checking-openldap-status}

After {{site.data.keyword.scale_full_notm}} cluster deployment, Schematic logs show essential information in the output section. From here, you can check your LDAP server status:

1. Connect to your OpenLDAP server through SSH by using the `ssh_to_ldap_node` command from the {{site.data.keyword.bpshort}} log output.

    For example:

    ```text
    ssh -o StrictHostKeyChecking=no -o UserKnownHostsFile=/dev/null -o ServerAliveInterval=5 -o ServerAliveCountMax=1 -J vpcuser@<floatingg_IP_address> ubuntu@<LDAP_server_IP>
    ```
    {: codeblock}

    where
    * `<floating_IP_address>` is the floating IP address for the bastion node.
    * `<LDAP_server_IP>` is the IP address for the OpenLDAP node.

2. Verify the LDAP service status:

    ```text
    systemctl status slapd
    ```
    {: codeblock}

3. Verify the LDAP groups and users created:

    ```text
    ldapsearch -Q -LLL -Y EXTERNAL -H ldapi:///
    ```
    {: codeblock}
