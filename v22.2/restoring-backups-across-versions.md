---
title: Restoring Backups Across Versions
summary: CockroachDB's support policy for restoring backups across versions.
toc: true
docs_area: manage
---

This page describes and answers frequently asked questions on the support CockroachDB provides for restoring backups across versions.

## Can I restore a backup that was taken on an older version into a newer version of CockroachDB?

If you took a [backup](backup.html) on major version N, you can [restore](restore.html) it on major version N+1. For example, backups taken on v21.1 can be restored into v21.2. Backups taken on v21.2 can be restored into v22.1.

Backup taken on version | Restorable into version
------------------------+--------------------------
20.1                    | 20.2
20.2                    | 21.1
21.1                    | 21.2
21.2                    | 22.1
22.1                    | 22.2

## I am upgrading from v20.2 to v22.1. I have a backup that was taken on v20.2. How do I restore it into v22.1?

When you upgrade from version N to N+1, you can restore version N backup into version N+1. Then take a new backup on the version you just upgraded to. You then repeat this process until you have fully upgraded to the desired version.

For example, you have taken a backup on v20.2 and you want to upgrade to v22.1. You would upgrade and back up through the following process:

1. Upgrade from v20.2 [to v21.1](upgrade-to-v21.1.html):
    1. Finalize your upgrade.
    1. [Restore your backup](restore.html#examples) from v20.2 into v21.1.
    1. [Take a new backup](backup.html#examples) on v21.1.
1. Upgrade from v21.1 [to v21.2](upgrade-to-v21.2.html).
    1. Finalize your upgrade.
    1. Restore your backup from v21.1 into v21.2.
    1. Take a new backup on v21.2.
1. Upgrade from v21.2 [to v22.1](upgrade-to-v22.1.html).
    1. Finalize your upgrade.
    1. Restore your backup from v21.2 into v22.1.
    1. Take a new backup on v22.1.

## What if I want to archive a backup for the long term? How will I be able to restore in the future when that version of CockroachDB is not supported anymore?

For this scenario, we recommend that you archive the CockroachDB binary of the version that the backup was taken on, alongside the backup. However, this is presuming that the binary can still be run and is able to upgrade to a version that is supported.

For a true archival copy that is not dependent on CockroachDB at all, running an [`EXPORT INTO CSV`](export.html) and archiving the files would be a better approach instead of taking a backup.

<!--TODO In the doc, we acknowledge that this doesn't satisfy all customer needs. We probably don't want to link to the issue in the doc. But, do we want to create a GH issue that is more generic as a tracking issue that can be linked here as a "Known limitation". I could also see leaving this as is! -->

## Can I restore a backup that was taken on a newer version into an older version of CockroachDB?

No, we do not support restoring backups from a newer version into an older version.

## What happens if I try to restore a backup that was taken on v20.1 into my cluster that is running v22.1?

You will see an error message indicating that the backup you are trying to restore is not supported on this version of CockroachDB. We recommend trying to restore this backup on v20.1 and upgrading that cluster to v22.1. You could pass a `WITH unsafe_restore_unsupported_version` option to the restore command to override the warning and proceed with this restore, however, this operation is potentially unsafe and could leave the cluster in an unsupported configuration.

## See also

- [`RESTORE`](restore.html)
- [Take Full and Incremental Backups](take-full-and-incremental-backups.html)
- [Upgrade Policy](upgrade-policy.html)