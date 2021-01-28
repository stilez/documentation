---
title: "Quick Backups with the Replication Wizard"
linkTitle: "Replication Wizard"
description: "How to use the TrueNAS Replication Wizard to create quick backups."
weight: 100
tags: ["ZFS", "replication"]
---

TrueNAS provides a replication wizard that is useful to quickly configure different simple replication scenarios.

{{% alert title=NOTE color=primary %}}
Please read the introductory page about replication, before using the wizard. as some it contains important information.
{{% /alert %}}

<img src="/images/TasksReplicationTasksAdd.png">
<br><br>

While regularly scheduled replications to a remote location are recommended as the optimal backup scenario, the wizard can be used to very quickly create and copy ZFS snapshots to another location on the same system.
This is useful when no remote backup locations are available or when there is an immediate danger of disk failure.

The only thing you'll need before creating a quick local replication are datasets or zvols in a storage pool to use as the replication source and (preferably) a second storage pool to use for storing replicated snapshots.
Setting up the local replication is done entirely in the Replication Wizard.

To open the Replication Wizard, go to **Tasks > Replication Tasks** and click **ADD**.
Set the source location to the local system and pick which datasets to snapshot.
The wizard takes new snapshots of the sources when no existing source snapshots are found.

<img src="/images/TasksReplicationTasksAddLocalSource.png">
<br><br>

Set the destination to the local system and define the path to the storage location for replicated snapshots.
When manually defining the destination, be sure to type the full path to the destination location.

<img src="/images/TasksReplicationTasksAddLocalSourceLocalDest.png">
