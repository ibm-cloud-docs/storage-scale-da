---

copyright:
  years: 2025
lastupdated: "2025-12-11"

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

# SSD Defined Performance (SDP)
{: #sdp-intro}

IBM Storage Scale is designed as a dedicated storage file system. In earlier scratch deployments, scale clusters were created using instance storage. While instance storage offers fast, low-cost, temporary disk for cloud-native workloads ideal for scratch space, caching, or replicated data. It is ephemeral. The data is directly tied to the lifecycle of the instance and is automatically deleted when the instance is terminated.

To provide bare-metal storage throughout the cluster lifecycle, the solution uses SSD Defined Performance (SDP), the second-generation IBM Cloud Block Storage. With this enhancement, the scratch version of Scale running on VSIs can now retain data persistently. Customers can also have the option to deploy bare-metal Scale clusters on bare-metal.

In both VSI based and bare-metal deployments, data is now persistent across the entire lifecycle of the cluster.

## SDP Overview
{: #sdp-overview}

The SSD Defined Performance (SDP) is a second-generation IBM Cloud Block Storage profile designed to provide enhanced flexibility in defining performance and capacity characteristics for block volumes. By using the `sdp` profile, you can specify the capacity and the maximum throughput limit.

### Benefits
{: #sdp-benefits}

* You can configure the volume size in the range from 1 GB to 32,000 GB.
* You can specify the volume performance in the range from 3000 IOPS to 64,000 IOPS.
* You can specify the maximum throughput limit in the range from 125 MBps to 1024 MBps (1000-8192 Mbps).
* SDP for VPC provides primary boot volumes and secondary data volumes.
* Boot volumes are automatically created and attached during instance provisioning. Data volumes can be created and attached during instance provisioning, or as stand-alone volumes that you can later attach to an instance.

The SDP provides high I/O throughput required for:

* Metadata operations
* Data input/output heavy workloads
* Large file movement
* Parallel job access patterns

For more information, see [SSD defined performance profile](/docs/vpc?topic=vpc-block-storage-profiles&interface=ui#defined-performance-profile).

### Limitations of using SDP as boot volume
{: #limitations}

* When you create an instance from a custom image, you can specify a boot volume capacity of 10 GB to 250 GB. If the boot volume exceeds 250 GB, the Virtual Server Instance (VSI) fails to boot successfully.

* Boot volume size can only be increased; reducing the size is not supported to maintain data safety and integrity.

Taking advantage of the bare-metal storage volume feature, today all the VSI created through the automation supports attaching SDP volume as:

* Boot volume
* Block or Data volume

## Boot Volume
{: #boot-volume}

Boot volumes are automatically created and attached during VSI provisioning. To simplify deployment and ensure consistent performance, the boot volume uses the SDP profile by default. However, this behavior can be overridden during provisioning by specifying a general-purpose profile when required by the workload. In the `volume_storages` variable, the `boot_volume_profile` is set to `sdp` by default, but users may override it as needed.

The boot volume (default: SDP profile) supports expansion up to **250 GB**. For more information, see [Profiles for boot volumes](/docs/vpc?topic=vpc-block-storage-profiles&interface=ui#vsi-profiles-boot).

When specifying for a general-purpose profile, the `boot_volume_iops`, should either be 0 or null. IOPS settings are not supported for general-purpose volumes.
For example, if the value is not set to 0 or null, the following error message occurs:

```pre
Invalid volume_storages configuration:
    - You can provide only block, or both sections.
    - If boot_volume_profile = "sdp":
         * boot_volume_size, boot_volume_iops (>=3000), and boot_volume_disk_grow are required
    - If boot_volume_profile = "general-purpose":
         * boot_volume_iops must be null or 0
    - If block_volume_capacity is not null or empty:
         * block_volume_iops must be >= 3000 and within correct range for capacity

This was checked by the validation rule at variables.tf:1186,3-13.
```

### Steps to expand an attached boot volume manually
{: #boot-steps}

1. Identify the boot volume.
    The attached boot volume is at `/dev/vda` with the root file system on partition 3.

2. Expand the partition.
    * Run the following command on all the storage node VSIs to expand the data partition to match the resized block volume:

    ```pre
    growpart /dev/vda 3
    ```
    {: codeblock}

3. Resize the volume.
    * The size of the underlying disk has increased.
    * The OS partition and file system needs to be expanded on every storage node VSI.

4. Grow the file system.
    * Once the partitions are expanded, grow the file system. For XFS file systems, run:

    ```pre
    xfs_growfs /
    ```
    {: codeblock}

    or use the appropriate mount point for the block volume (for example, /gpfs/fs1).

## Block (Data) Volume
{: #block-volume}

For IBM Storage Scale deployments, at least one block (data) volume is required. These volumes are typically provisioned using the SDP profile to ensure the performance and durability required for file system operations. If required, the block volume can be expanded up to 32,000 GB (32 TB).

Since Storage Scale requires dedicated storage capacity, the solution utilizes SDP volumes that are created and attached to each scale cluster node. These volumes are used collectively to form a storage cluster. Unlike boot volumes, data volumes do not provide selectable profile options. Every data volume is automatically created using the `sdp` profile by default.

Using the `volume_storages` variable, update the `block_volume_capacity` variable to define the required block volume capacity. Based on this configuration, SDP volumes are automatically created and attached to the provisioned instances.

If the cluster needs additional capacity later, the automation supports expanding data volumes. To apply the change at the cluster level, you must either update the capacity manually or rely on automation to handle it. Automation can perform the full expansion in a single operation, set the `block_volume_disk_grow = true`, and the system will extend the disk inside the Scale cluster to use the newly available capacity.

After expanding the block volume, the Storage Scale cluster does not automatically detect the increased disk size.
{: note}

If each node is provisioned with a 500 GB block volume and the cluster consists of four Storage Scale nodes, the total effective storage capacity available to the cluster is 2 TB.
{: note}

### Steps to expand an attached block volume manually
{: #block-steps}

1. Identify the block volume.
    The attached block volume appears at the path `/dev/vdd` with the root file system on partition 1.

2. Expand the partition.
    * Run the following command on all storage node VSIs to expand the data partition to match the resized block volume:

    ```pre
    growpart /dev/vdd 1
    ```
    {: codeblock}

3. Resize the volume.
    * The size of the underlying disk has increased.
    * The OS partition and file system also need to be expanded on every storage node VSI.

4. Grow the file system.
    * Once the partitions are expanded, grow the file system. For XFS file systems, run:

    ```pre
    xfs_growfs /
    ```
    {: codeblock}

    or use the appropriate mount point for the block volume (for example, /gpfs/fs1).

## Verifying the file system
{: #verify-fs}

1. Ensure you have expanded both partitions and the file system using the appropriate commands (`growpart` and `xfs_growfs` or equivalent).

2. On each storage node VSI, run the following command:

    ```pre
    lsblk
    ```
    {: codeblock}

    This command confirms the updated device sizes, partition growth, and correct mount points.

## Deleting NSD from file system
{: #delete-fs}

Go to the primary node of the Storage Scale nodes, to perform the deletion process.
{: important}

1. Run the following command to get the Network Shared Disk (NSD) device name.

    ```pre
    /usr/lpp/mmfs/bin/mmlsdisk <file-system-name> -L
    ```
    {: codeblock}

2. Run the following command to delete the NSD device name.

    ```pre
    /usr/lpp/mmfs/bin/mmdeldisk <file-system-name> <NSD_DEVICE>
    ```
    {: codeblock}

3. Before modifying or recreating NSD, run the following command to remove the disk from the file system:

    ```pre
    /usr/lpp/mmfs/bin/mmdelnsd <NSD_DEVICE>
    ```
    {: codeblock}

    Replace <NSD_DEVICE> with the NSD name mapped to the resized volume.
    {: note}

4. Resize the backend storage LUN or hypervisor virtual disk. Validate by running the following command:

    ```pre
    lsblk /dev/vdd
    ```
    {: codeblock}

    Replace `/dev/vdd` with the actual device name for your NSD.
    {: note}

5. Create the NSD stanza file. Update the following values in your environment:

    Generic template example:

    a. To get existing stanza file:
    `cd /var/mmfs/tmp`

    b. Locate the stanza file with grep command:
    `ls -ltr /var/mmfs/tmp | grep StanzaFile`

    c. View the **StanzaFile** with the `cat` command.

    ```pre
    [root@hpc-scale -sdp-thu-8-strg-0ea6-001 tmp]# cat StanzaFile. fs1
    %nsd:
    device=/dev/vdd
    nsd=nsd_hpc-scale_sdp_thu_8_strg_0ea6_001_vdd
    servers= hpc-scale-sdp-thu-8-strg-0ea6-001
    usage=dataAndMetadata
    failureGroup=1
    pool=system

    %nsd:
    device=/dev/vdd
    nsd=nsd_hpc-scale_sdp_thu_8_strg_0ea6_002_vdd
    servers= hpc-scale-sdp-thu-8-strg-0ea6-002
    usage=dataAndMetadata
    failureGroup=2
    pool=system

    %nsd:
    device=/dev/vdd
    nsd=nsd_hpc-scale_sdp_thu_8_strg_tie_0ea6_001_vdd
    servers= hpc-scale-sdp-thu-8-strg-tie-0ea6-001
    usage=descOnly
    failureGroup=3
    pool=system
    ```
    {: codeblock}

    **Example:**

    ```pre
    %nsd:
    device=/dev/vdd
    nsd=nsd_hpc-scale_sdp_thu_8_strg_0ea6_001_vdd
    servers= hpc-scale-sdp-thu-8-strg-0ea6-001
    usage=dataAndMetadata
    failureGroup=1
    pool=system
    ```

    Parameters to update:
    * **<DEVICE_NAME>** – OS disk name (example: vdd)
    * **<NSD_NAME>** – Name of the NSD
    * **<SERVER_NAME>** – Node acting as NSD server
    * **<FG_ID>** – Failure group ID
    * **<POOL_NAME>** – System or custom

6. Run the following command to recreate the NSD:

    ```pre
    /usr/lpp/mmfs/bin/mmcrnsd -F /var/mmfs/tmp/<Stanza-file-name.file-system-name>
    ```
    {: codeblock}

    **Example:**

    ```pre
    [root@hpc-scale-sdp-thu-8-strg-0ea6-001 tmp]# /usr/lpp/mmfs/bin/mmcrnsd -F /var/mmfs/tmp/StanzaFile-nsd_ hpc-scale _sdp_thu_8_strg_0ea6_001_vdd.fs1
    mmcrnsd: Processing disk vdd
    mmcrnsd: Propagating the cluster configuration data to all
    affected nodes. This is an asynchronous process.
    [root@hpc-scale-sdp-thu-8-strg-0ea6-001 tmp]#
    ```

    This registers the NSD again in the cluster.

7. Add the disk back to the file system. Add the updated NSD to file system fs1:

    ```pre
    /usr/lpp/mmfs/bin/mmadddisk <file-system-name> -F /var/mmfs/tmp/nsd_update.stanza
    ```
    {: codeblock}

    **Example:**

    ```pre
    [root@hpc-scale-sdp-thu-8-strg-0ea6-001 tmp]# /usr/lpp/mmfs/bin/mmadddisk fs1 -F /var/mmfs/tmp/StanzaFile-nsd_hpc-scale_sdp_thu_8_strg_0ea6_001_vdd.fs1
    The following disks of fs1 will be formatted on node hpc-scale-sdp-thu-8-strg-0ea6-002.strg.com:
    nsd_hpc-scale_sdp_thu_8_strg_0ea6_001_vdd: size 1536000 MB
    Extending Allocation Map
    Checking Allocation Map for storage pool system
    ompleted adding disks to file system fs1.
    mmadddisk: Propagating the cluster configuration data to all
    affected nodes. This is an asynchronous process.
    [root@hpc-scale-sdp-thu-8-strg-0ea6-001 tmp]#
    ```

8. Validate the disk update. Verify that the GPFS recognizes the disk correctly, by running the following command:

    ```pre
    /usr/lpp/mmfs/bin/mmlsdisk <file-system-name> -L | grep <NSD_NAME>
    ```
    {: codeblock}

Terraform dependencies may still delete the resources even if auto-delete option is disabled, therefore always back up your data before deletion.
{: important}

## Updating boot or data volume using UI
{: #create-ui}

1. Log in to the [{{site.data.keyword.cloud_notm}} catalog](https://cloud.ibm.com/catalog){: external} by using your credentials.
2. Go to the **Navigation Menu**.
3. Click **Infrastructure** > **Compute** > **Virtual server instances**.
4. Click the virtual server instances created during cluster provisioning.
5. Click **Storage**.
6. Under Storage volumes, click the required **Boot volume** or **Data volume**.
7. Edit the required **Size**.

## Updating boot or data volume using CLI
{: #update-cli}

1. Run the following command to increase the capacity of the filesystem:

    ```pre
    ibmcloud is volume-update <Volume-ID> --capacity <capacity-in-GB>
    ```
    {: codeblock}

    For example, `ibmcloud is volume-update r010-9796daf5-5f24- 4b12-9a0c-a25456917edf --capacity 180`

2. Run the following command to increase the IOPS when the profile is set as `sdp`.

    ```pre
    ibmcloud is volume-update <Volume-ID> --iops <iops-value>
    ```
    {: codeblock}

    For example, `ibmcloud is volume-update r010-9796daf5-5f24-4b12-9a0c-a25456917edf --iops 3001`

You can perform the steps manually or using CLI, but the recommended way is using automation.
{: tip}
