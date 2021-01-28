---
title: "Local Replication"
linkTitle: "Local"
description: "How to use the TrueNAS Wizard to quickly back up new snapshots to another location on the local system."
weight: 1
tags: ["ZFS", "replication"]
---

## Process Summary

* Requirements: Storage pools and datasets created in **Storage > Pools**.

* **Tasks > Replication Tasks > ADD**
  * Choose Sources
    * Set the source location to the local system
    * Use the file browser or type paths to the sources
  * Define a Destination path
    * Set the destination location to the local system
  	* Select or manually define a path to the single destination location for the snapshot copies.
  * Set the Replication schedule to run a single time
  * Define how long the snapshots will be stored in the Destination
  * Clicking "START REPLICATION" immediately snapshots the chosen Sources and copies those snapshots to the Destination
    * Replication causes data in the destination to match the data in the source. As part of the process, snapshots that no longer exist in the source may need to be deleted in the destination. If this is required, a dialog will ask for confirmation to delete existing snapshots from the destination. Be sure that all important important data is protected before continuing.
* Clicking the task *State* shows the logs for that replication task.

## Quick Backups with the Replication Wizard

TrueNAS provides a wizard that is useful to quickly configure different simple replication scenarios.

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
<br><br>

TrueNAS suggests a default name for the task based on the selected source and destination locations, but you can type your own name for the replication.
Every saved replication task can be loaded into the wizard to make creating new replication schedules even easier.

You can define a specific schedule for this replication or choose to run it immediately after saving the new task.
Unscheduled tasks are still saved in the replication task list and can be run manually or edited later to add a schedule.

## Snapshot lifetime

Snapshots themselves take very little space, but may contain old data that no longer exists in the current pool. Obsolete data may accumulate in very old snapshots, and take up space in the destination pool. ZFS prefers pools to be under around 75% - 90% full, and may run with reduced inefficiency when nearly full.

For these reasons, it is usually recommended to define a destination snapshot lifetime. The lifetime specifies how long copied snapshots are stored in the destination before they are deleted. Choosing to keep snapshots indefinitely may require you to manually remove old snapshots in the destination pool, if the destination becomes too full.


<img src="/images/TasksReplicationTasksAddLocalSourceLocalDestCustomLife.png">
<br><br>

## Saving and executing the task

Clicking **START REPLICATION** saves the new task and immediately attempts to replicate snapshots to the destination.
When TrueNAS detects that the destination already has unrelated snapshots, it will ask to delete the unrelated snapshots and do a full copy of the new snapshots.
This can delete important data, so be sure any existing snapshots can be deleted or are backed up in another location.

The simple replication is added to the Replication task list and will show that it is currently running.
Clicking the task state shows the replication log with an option to download the log to your local system.

<img src="/images/TasksReplicationTasksLocalLogs.png">
<br><br>

To confirm that snapshots have been replicated, go to **Storage > Snapshots** and verify the destination Dataset has new Snapshots with correct timestamps.

<img src="/images/TasksReplicationTasksLocalSnapshots.png">
