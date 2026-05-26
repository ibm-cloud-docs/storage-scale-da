---

copyright:
  years: 2026
lastupdated: "2026-05-26"

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

# Configuring CES with LDAP deployment values
{: #config-ldap-ces}

The CES feature offers two authentication options, the default user-defined method and the use of directory services like AD, LDAP, or NIS. The directory services enable access to files through the NFS protocol. LDAP is an optional component for CES, allowing users to either use an existing LDAP server or set up a new LDAP node specifically for the CES cluster.

By setting up the `enable_ldap`, `ldap_basedns`, `ldap_server`, `ldap_server_cert`, `ldap_admin_password`, `ldap_user_name`, `ldap_user_password`, and `ldap_instance` deployment value to the required domain name during the deployment, the LDAP feature is integrated along with the Scale CES.

## Verifying authentication
{: #verify-authentication}

Use the `mmuserauth` command to view the details on the type of authentication used for CES:

```pre
mmuserauth service check
```
{: codeblock}

The command that is mentioned indicates that the authentication process is being carried out through LDAP.

For more information about CES authentication, see [CES User Authentication](/docs/storage-scale-da?topic=storage-scale-da-config-ces-integration-ldap-authentication&interface=ui).

## Creating and configuring a LDAP certificate with your LDAP server
{: #create-configure-ldap-certificate}

If your existing LDAP server is configured without a certificate, follow these steps to create one and configure it with the LDAP server. If your LDAP server is already configured with a certificate, you can skip steps 1 through 16 and proceed directly to copying the certificate content [step 17].

1. Configure SSH key into your existing LDAP server.

2. Install the required software
   `apt install gnutls-bin ssl-cert -y`

3. Generate the SSL certificate and configure with the OpenLDAP server.
    ```text
    certtool --generate-privkey --sec-param High --outfile
    /etc/ssl/private/ldap_cakey.pem
    ```
    {: codeblock}

4. Create a CA template file.
    ```text
    cat <<EOF > /etc/ssl/ca.info
    cn = IBM Research
    ca
    cert_signing_key
    expiration_days = 3650
    EOF
    ```
    {: codeblock}

5. Generate a self-signed CA certificate.
    ```text
    certtool --generate-self-signed \
    --load-privkey /etc/ssl/private/ldap_cakey.pem \
    --template /etc/ssl/ca.info \
    --outfile /usr/local/share/ca-certificates/ldap_cacert.pem
    ```
    {: codeblock}

6. Update the CA certificates and copy the generated CA certificate to `/etc/ssl/certs/` path.
    ```text
    update-ca-certificates
    cp /usr/local/share/ca-certificates/ldap_cacert.pem /etc/ssl/certs/
    ```
    {: codeblock}

7. Generate a private key for the LDAP server.
    ```text
    certtool --generate-privkey --sec-param High --outfile
    /etc/ssl/private/ldapserver_slapd_key.pem
    ```
    {: codeblock}

8. Create an LDAP server certificate template.
    ```pre
    cat <<EOF > /etc/ssl/ldapserver.info
    organization = IBM Research
    cn = localhost
    tls_www_server
    encryption_key
    signing_key
    expiration_days = 3650
    EOF
    ```
    {: codeblock}

9. Generate a certificate for the LDAP server signed by the CA.
    ```pre
    certtool --generate-certificate \
    --load-privkey /etc/ssl/private/ldapserver_slapd_key.pem \
    --load-ca-certificate /etc/ssl/certs/ldap_cacert.pem \
    --load-ca-privkey /etc/ssl/private/ldap_cakey.pem \
    --template /etc/ssl/ldapserver.info \
    --outfile /etc/ssl/certs/ldapserver_slapd_cert.pem
    ```
    {: codeblock}

10. Set proper permissions for the LDAP server private key.
    ```pre
    chgrp openldap /etc/ssl/private/ldapserver_slapd_key.pem
    chmod 0640 /etc/ssl/private/ldapserver_slapd_key.pem
    gpasswd -a openldap ssl-cert
    ```
    {: codeblock}

11. Restart `slapd` service to apply the changes
    `systemctl restart slapd.service`

12. Create an LDIF file for configuring TLS in the LDAP server.
    ```pre
    cat <<EOF > /etc/ssl/certinfo.ldif
    dn: cn=config
    add: olcTLSCACertificateFile
    olcTLSCACertificateFile: /etc/ssl/certs/ldap_cacert.pem
    -
    add: olcTLSCertificateFile
    olcTLSCertificateFile: /etc/ssl/certs/ldapserver_slapd_cert.pem
    -
    add: olcTLSCertificateKeyFile
    olcTLSCertificateKeyFile: /etc/ssl/private/ldapserver_slapd_key.pem
    EOF
    ```
    {: codeblock}

13. Apply TLS configuration using `ldapmodify` command.
    ```pre
    ldapmodify -Y EXTERNAL -H ldapi:/// -f /etc/ssl/certinfo.ldif
    ```
    {: codeblock}

14. Configure the `slapd` service to listen on both ldap:// and ldaps://"
    ```pre
    sed -i 's\SLAPD_SERVICES="ldap:/// ldapi:///"\SLAPD_SERVICES="ldap:/// ldapi:/// ldaps:///"\g' /etc/default/slapd
    ```
    {: codeblock}

15. Update `/etc/ldap/ldap.conf` file.
    ```pre
    cat <<EOF >> /etc/ldap/ldap.conf
    TLS_CACERT /etc/ssl/certs/ldap_cacert.pem
    TLS_REQCERT allow
    EOF
    ```
    {: codeblock}

16. Restart `slapd` service to apply the changes.
    `systemctl restart slapd.service`

17. Copy the LDAP certificate content from "/etc/ssl/certs/ldap_cacert.pem" path as a single line and provide as input to the "ldap_server_cert" variable.

    `awk 'NF {sub(/\r/, ""); printf "%s\\n",$0;}' /etc/ssl/certs/ldap_cacert.pem`
    Replace `/etc/ssl/certs/ldap_cacert.pem` with the path to your existing LDAP certificate if it differs.
