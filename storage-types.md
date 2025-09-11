---

copyright:
  years: 2025
lastupdated: "2025-09-11"

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
{:beta: .beta}
{:row-headers: .row-headers}
{:table: .aria-labeledby="caption"}

# Storage types
{: #storage-types}

The {{site.data.keyword.scale_short}} solution offers three different storage types:
* Scratch storage
* Persistent storage
* Evaluation storage
{: shortdesc}

The offering enables deployment of either scratch (or ephemeral) or persistent storage, depending on application requirements. A scratch configuration uses virtual server instances with instance storage, whereas a persistent configuration uses bare metal servers with locally attached NVMe storage. If a virtual server instance with instance storage is powered off, all data that is stored on the instance storage volumes is rendered inaccessible after a subsequent power up of the virtual server instance. Therefore, use of scratch storage is not recommended for long-running or mission-critical workloads. In addition to higher resilience, persistent storage provides higher performance and capacity than scratch storage.

## Scratch storage
{: #scratch-storage}

A scratch configuration uses virtual server instances with instance storage. If a virtual server instance with instance storage is powered off then all the data that is stored on the instance storage volumes is rendered inaccessible after a subsequent power up of the virtual server instance. Therefore, use of scratch storage is not recommended for long-running or mission-critical workloads.

| Supported configurations | Not recommended configurations |
| ----------------- | ---------- |
| Storage (VSI) | Storage (VSI) + Compute (VSI) + Protocol (BareMetal) + AFM (VSI) |
| Storage (VSI) + AFM (VSI) | Storage (VSI) + Compute (VSI) + Protocol (BareMetal) + Client + AFM (VSI) |
| Storage (VSI) + Compute (VSI) + AFM (VSI) | Storage (VSI) + Compute (BareMetal) + Protocol (BareMetal) + Client + AFM (VSI) |
| Storage (VSI) + Compute (VSI) + Protocol + AFM (VSI) | Storage (VSI) + Compute (BareMetal) + Protocol (BareMetal) + Client (BareMetal) + AFM (VSI) |
| Storage (VSI) + Compute (VSI) + Protocol + Client + AFM (VSI) | Storage (BareMetal) + Compute (BareMetal) + Protocol (BareMetal) + Client (BareMetal) + AFM (VSI) |
| Storage (VSI) + Compute (VSI) + Protocol (VSI) + Client (VSI) + AFM (VSI) + KMS | Storage (BareMetal) + Compute (VSI) + Protocol (VSI) + Client (VSI) + AFM (VSI) |
| Storage (VSI) + Compute (VSI) + Protocol (VSI) + Client (VSI) + AFM (VSI) + GKLM (VSI) | Storage (BareMetal) + Compute (VSI) + Protocol (VSI) + Client (VSI) + AFM (VSI) + GKLM (BareMetal) |
| Storage (VSI) + Compute (VSI) + Protocol (VSI) + Client (VSI) + AFM (VSI) + LDAP | |
| Storage (VSI) + Compute (VSI) + Protocol (VSI) + Client (VSI) + AFM (VSI) + KMS + LDAP | |
| Storage (VSI) + Compute (VSI) + Protocol (VSI) + Client (VSI) + AFM (VSI) + GKLM (VSI) + LDAP | |
| Storage (VSI) + Compute (VSI) + Protocol (VSI) + Client (VSI) + AFM (VSI) + KMS + LDAP + Colocation | |
| Storage (VSI) + Compute (VSI) + Protocol (VSI) + Client (VSI) + AFM (VSI) + GKLM (VSI) + LDAP + Colocation | |
{: caption="Scratch storage configurations" caption-side="bottom"}

## Persistent storage
{: #persistent-storage}

A persistent configuration uses bare metal servers with locally attached NVMe storage. In addition to higher resilience, persistent storage provides higher performance and capacity than scratch storage.

{{site.data.keyword.scale_full_notm}} supports both Sapphire Rapids (x3 and x3d) profiles and Cascade Lake (x2 and x2d). For more information, see [x86-64 bare metal server profiles](/docs/vpc?topic=vpc-bare-metal-servers-profile&interface=ui).

| Supported configurations | Not recommended configurations |
| ----------------- | ---------- |
| Storage (BareMetal) | Storage (BareMetal) + Compute (VSI) + Protocol (VSI) + AFM (BareMetal) |
| Storage (BareMetal) + AFM (BareMetal) | Storage (BareMetal) + Compute (BareMetal) + Protocol (VSI) + Client + AFM (BareMetal) |
| Storage (BareMetal) + Compute (VSI) + AFM (BareMetal) | Storage (BareMetal) + Compute (VSI) + Protocol (BareMetal) + Client (BareMetal) + AFM (BareMetal) |
| Storage (BareMetal) + Compute (VSI) + Protocol (BareMetal) + AFM (BareMetal) | Storage (BareMetal) + Compute (BareMetal) + Protocol (BareMetal) + Client (BareMetal) + AFM (BareMetal) |
| Storage (BareMetal) + Compute (VSI) + Protocol (BareMetal) + Client + AFM (BareMetal) | Storage (VSI) + Compute (BareMetal) + Protocol (BareMetal) + Client (BareMetal) + AFM (BareMetal) |
| Storage (BareMetal) + Compute (VSI) + Protocol (BareMetal) + Client (VSI) + AFM (BareMetal) + KMS | Storage (VSI) + Compute (VSI) + Protocol (VSI) + Client (VSI) + AFM (VSI) |
| Storage (BareMetal) + Compute (VSI) + Protocol (BareMetal) + Client (VSI) + AFM (BareMetal) + GKLM (VSI) | Storage (VSI) + Compute (VSI) + Protocol (BareMetal) + Client (VSI) + AFM (BareMetal) |
| Storage (BareMetal) + Compute (VSI) + Protocol (BareMetal) + Client (VSI) + AFM (BareMetal) + LDAP (VSI) | |
| Storage (BareMetal) + Compute (VSI) + Protocol (BareMetal) + Client (VSI) + AFM (BareMetal) + KMS + LDAP (VSI) | |
| Storage (BareMetal) + Compute (VSI) + Protocol (BareMetal) + Client (VSI) + AFM (BareMetal) + GKLM (VSI) + LDAP (VSI) | |
| Storage (BareMetal) + Compute (VSI) + Protocol (BareMetal) + Client (VSI) + AFM (BareMetal) + KMS (VSI) + LDAP (VSI) + Colocation | |
| Storage (BareMetal) + Compute (VSI) + Protocol (BareMetal) + Client (VSI) + AFM (BareMetal) + GKLM (VSI) + LDAP (VSI) + Colocation | |
| Storage (BareMetal) + Compute (VSI) + Protocol (BareMetal) + Client (VSI) + AFM (BareMetal) + KMS (VSI) + LDAP (VSI) + Colocation + Boot Drive Encryption | |
| Storage (BareMetal) + Compute (VSI) + Protocol (BareMetal) + Client (VSI) + AFM (BareMetal) + GKLM (VSI) + LDAP (VSI) + Colocation + Boot Drive Encryption | |
{: caption="Persistent storage configurations" caption-side="bottom"}

## Evaluation storage
{: #evaluation-storage}

Evaluation storage is based on {{site.data.keyword.scale_short}} Developer Edition. This option supports all advanced features of {{site.data.keyword.scale_short}} Data Management Edition but is limited to 12TB of storage.

With evaluation storage, you can try out the {{site.data.keyword.scale_short}} solution on {{site.data.keyword.cloud}} without a license. The automated deployment is done on virtual server instances with instance storage, with {{site.data.keyword.scale_short}} Developer Edition packages, and is available only for prototyping and testing purposes.

| Supported configurations | Not recommended configurations |
| ----------------- | ---------- |
| Storage (VSI) | Storage (VSI) + Compute (VSI) + Protocol (BareMetal) + AFM (VSI) |
| Storage (VSI) + AFM (VSI) | Storage (VSI) + Compute (VSI) + Protocol (BareMetal) + Client + AFM (VSI) |
| Storage (VSI) + Compute (VSI) + AFM (VSI) | Storage (VSI) + Compute (BareMetal) + Protocol (BareMetal) + Client + AFM (VSI) |
| Storage (VSI) + Compute (VSI) + Protocol + AFM (VSI) | Storage (VSI) + Compute (BareMetal) + Protocol (BareMetal) + Client (BareMetal) + AFM (VSI) |
| Storage (VSI) + Compute (VSI) + Protocol + Client + AFM (VSI) | Storage (BareMetal) + Compute (BareMetal) + Protocol (BareMetal) + Client (BareMetal) + AFM (VSI) |
| Storage (VSI) + Compute (VSI) + Protocol (VSI) + Client (VSI) + AFM (VSI) + KMS | |
| Storage (VSI) + Compute (VSI) + Protocol (VSI) + Client (VSI) + AFM (VSI) + GKLM (VSI) | |
| Storage (VSI) + Compute (VSI) + Protocol (VSI) + Client (VSI) + AFM (VSI) + LDAP | |
| Storage (VSI) + Compute (VSI) + Protocol (VSI) + Client (VSI) + AFM (VSI) + KMS + LDAP | |
| Storage (VSI) + Compute (VSI) + Protocol (VSI) + Client (VSI) + AFM (VSI) + GKLM (VSI) + LDAP | |
| Storage (VSI) + Compute (VSI) + Protocol (VSI) + Client (VSI) + AFM (VSI) + KMS + LDAP + Colocation | |
| Storage (VSI) + Compute (VSI) + Protocol (VSI) + Client (VSI) + AFM (VSI) + GKLM (VSI) + LDAP + Colocation | |
{: caption="Evaluation storage configurations" caption-side="bottom"}

## Storage type comparison
{: #storage-type-comparison-table}

|      | Scratch (default) | Persistent | Evaluation |
| ---- | ----------------- | ---------- | ---------- |
| Storage cluster nodes | Virtual server instances | Bare metal servers | Virtual server instances |
| Storage cluster node count | Min 2  \n Max 64 | Min 2  \n Max 32 | Min 2  \n Max 64 |
| Compute cluster node count | Min 3  \n Max 64 | NA | Min 3  \n Max 64 |
| Protocol node count | Min 2  \n Max 32 | Min 2  \n Max 32 | Min 2  \n Max 32 |
| Client node count | Min 2  \n Max 2000 | NA | Min 2  \n Max 2000 |
| AFM node count | Min 1  \n Max 16 | Min 1  \n Max 16 | Min 1  \n Max 16 |
| Storage cluster OS support | RHEL 8.10  \n (custom or stock images) | RHEL 8.10  \n (custom or stock images) | RHEL 8.10  \n (custom image) |
| Compute cluster OS support | RHEL 8.10  \n (custom or stock images) | RHEL 8.10  \n (custom or stock images) | RHEL 8.10  \n (custom image) |
| Protocol nodes | RHEL 8.10  \n (custom image) | RHEL 8.10  \n (custom image) | RHEL 8.10  \n (custom image) |
| Client nodes | RHEL 8.8, 8.10 | RHEL 8.8, 8.10 | RHEL 8.8, 8.10 |
| Storage Scale edition and version | Storage Scale Data Management Edition v5.2.3.2 | Storage Scale Data Management Edition v5.2.3.2 | Storage Scale Developer Edition v5.2.3.0 |
| IBM Customer Number required? | Yes | Yes | No |
| Customer support available? | Yes | Yes | No |
{: row-headers}
{: caption="Storage Scale storage types comparison" caption-side="bottom"}
{: summary="The first row of the table describes a Storage Scale feature, and the first column describes the specifics of that feature as it pertains to scratch storage. The second column describes the specifics of persistent storage, and the third column describes the specifics of evaluation storage, which map to the Storage Scale feature in each row."}

For the storage instance profiles, "d" profile is mandatory. But for other profiles, this is not required.
{: tip}

For more information about {{site.data.keyword.scale_short}} editions, see [{{site.data.keyword.scale_full_notm}} product editions](https://www.ibm.com/docs/en/storage-scale/5.2.3?topic=overview-storage-scale-product-editions){: external}.
