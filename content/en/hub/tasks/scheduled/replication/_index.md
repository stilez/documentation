---
title: "ZFS Snapshot Replication"
linkTitle: "Replication"
description: "ZFS snapshot replication: Making backups of backups to another pool"
weight: 50
tags: ["ZFS", "replication"]
---

{{% alert title=NOTE color=primary %}}
When upgrading to TrueNAS 11.3, replication now uses a default token. No
reconfiguration is needed. The token works in the background and cannot be changed.
This provides an easy, low maintenance replication experience.
{{% /alert %}}

Replication is the term for partial or total syncing of data between two ZFS pools, in such a way that ZFS properties, structures and snapshots are preserved.

ZFS datasets and volumes in a TrueNAS pool (the 'source'), may be backed up to another pool (the 'destination'), on the same or a different system. By default, the result of replication is a copy of the same data added to the second pool, with identical contents and properties.  

This article provides an overview relevant to all types of replication. Examples on this page show local replication. The linked articles give further details relevant to local, remote and advanced replication tasks.

## Replication Wizard

Optionally, instead of the full replication interface, a Replication Wizard can be used to quickly configure different simple replication scenarios, such as very quickly creating and copying ZFS snapshots to another location or pool on the same system. This is useful when no remote backup locations are available or when there is an immediate danger of disk failure.

## Process Summary

* Requirements: Storage pools and datasets created in **Storage > Pools**.

* **Tasks > Replication Tasks > ADD**
  * Choose the source(s) to be replicated, and the destination that the data is to be replicated to.  These may be local (on this system) or remote (on a different system) - details are described in the linked articles. If the source or destination pool is on another system, SSH will be used to connect and send the necessary ZFS commands to that system, to send or receive the data.
  * Set the Replication schedule to run a single time, or repeatedly on a schedule, as required
  * Define how long the snapshots will be stored in the Destination
  * Clicking "START REPLICATION" immediately snapshots the chosen Sources and copies those snapshots to the Destination
* Clicking the task *State* shows the logs for that replication task.

TrueNAS suggests a default name for the task based on the selected source and destination locations, but you can type your own name for the replication.  The task will be saved for future use, even if only run one time. Previous replication tasks can be used or loaded into the wizard at a later time, to make creating new replication schedules even easier.

{{% alert title=NOTE color=primary %}}
During the replication process, obsolete snapshots may be found in the destination pool that no longer in the source pool. This obsolete data may need to be deleted from the destination, to ensuire the destination is a faithful copy of the source. If required, a dialog will ask for confirmation to delete existing snapshots from the destination. Be sure that any important important data is protected before continuing.
{{% /alert %}}

## Snapshot schedule

Adding a schedule automates the task to run according to your chosen times.
You can define a specific schedule for this replication or choose to run it immediately after saving the new task.
Unscheduled tasks are still saved in the replication task list and can be run manually or edited later to add a schedule.

## Snapshot lifetime

Snapshots themselves take very little space, but may contain old data that no longer exists in the current pool. Obsolete data may accumulate in very old snapshots, and take up space in the destination pool. ZFS prefers pools to be under around 75% - 90% full, and may run with reduced inefficiency when nearly full.

For these reasons, it is usually recommended to define a destination snapshot lifetime. The lifetime specifies how long copied snapshots are stored in the destination before they are deleted. Choosing to keep snapshots indefinitely may require you to manually remove old snapshots in the destination pool, if the destination becomes too full.

<img src="/images/TasksReplicationTasksAddLocalSourceLocalDestCustomLife.png">
<br><br>

## Starting a replication task

**START REPLICATION** saves the new replication task and immediately attempts to replicate snapshots to the destination.
New tasks are enabled by default and activate according to their schedule or immediately when no schedule was chosen.

When TrueNAS detects that the destination pool already has old snapshots that no longer exist in the source pool, it will ask to delete these and do a full copy of the new snapshots. This can delete important data, so be sure any existing snapshots can safely be deleted or if required, are backed up in another location.

The first time a replication task runs, it takes longer because the snapshots must be copied entirely fresh to the destination.
Later replications run faster, as only the subsequent changes to snapshots are replicated.

The task is added to the Replication task list and will show that it is currently running.
Clicking the task state opens the log for that replication. There is an option to download the log to your local system.

<img src="/images/TasksReplicationTasksLocalLogs.png">
<br><br>

To confirm that snapshots have been replicated, go to **Storage > Snapshots** and verify the destination Dataset has new Snapshots with correct timestamps.

<img src="/images/TasksReplicationTasksLocalSnapshots.png">

