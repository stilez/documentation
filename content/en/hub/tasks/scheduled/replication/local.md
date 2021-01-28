---
title: "Local Replication"
linkTitle: "Local"
description: "How to use the TrueNAS Wizard to quickly back up new snapshots to another location on the local system."
weight: 1
tags: ["ZFS", "replication"]
---

{{% alert title=NOTE color=primary %}}
Please read the [introductory page](/hub/tasks/scheduled/replication) about replication before running a replication task, as some it contains important information.  This page summarises the settings that specifically apply to local replication.
{{% /alert %}}

## Source and destination

  * Define the source(s)
    * Set the source location to the local system
    * Use the file browser or type paths to the source(s)
  * Define a Destination path
    * Set the destination location to the local system
  	* Select or manually define a path to the single destination location for the snapshot copies.
  * Set the Replication schedule to run a single time
  * Define how long the snapshots will be stored in the Destination
  * Clicking "START REPLICATION" immediately snapshots the chosen Sources and copies those snapshots to the Destination. See the warning about snapshot deletion
* Clicking the task *State* shows the logs for that replication task.

## Starting the task

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
