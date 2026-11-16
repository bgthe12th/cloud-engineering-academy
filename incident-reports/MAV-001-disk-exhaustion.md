# Incident MAV-001: Document Processing Failure

**Status:** Training simulation  
**Project:** Maven Document Processing  
**Environment:** Hypothetical Azure Ubuntu Linux VM  
**Incident Type:** Storage capacity / write failure

## 1. Incident Summary

Dispatchers reported that rate confirmations were uploading successfully but were not being processed into new loads.

The application process was still running, but document-processing operations were failing.

## 2. Evidence

The simulated monitoring system reported:

- CPU utilization: 24%
- Memory utilization: 68%
- Disk utilization: 99%
- Application process: Running

The application logs contained:

```text
ERROR: Failed to write temporary document
ERROR: No space left on device
```

## 3. Initial Diagnosis

The most likely cause was disk-space exhaustion preventing the document-processing service from writing temporary files.

This conclusion was based on the high disk utilization and the application's reported write errors.

The diagnosis would need to be confirmed by inspecting the affected filesystem.

## 4. Investigation Plan

I would begin with:

```bash
df -h
df -i
```

These commands would help determine whether filesystem capacity or inode exhaustion was preventing new files from being created.

Next, I would identify which filesystem the application uses for temporary files and logs.

I would use directory-size inspection tools such as `du` to identify likely storage consumers.

I would also inspect application logs to confirm the error and identify which document-processing requests failed.

## 5. Recovery Plan

I would identify files that could be safely deleted, archived, or transferred to free storage space.

Before modifying files, I would verify that they were not required for operations, compliance, auditing, or recovery.

If necessary, I would consider expanding the filesystem rather than deleting important data.

After restoring sufficient space, I would verify that the application could write temporary files and process new documents.

I would then investigate whether failed processing requests needed to be retried or reconciled.

## 6. Prevention

I would configure monitoring alerts to notify operations staff before storage capacity became critically low.

Initial proposed thresholds:

- Warning: 80% disk utilization
- Critical: 90% disk utilization

I would also evaluate log rotation, temporary-file cleanup, storage growth patterns, and additional monitoring for processing failures.

## 7. Lessons Learned

A running application process does not necessarily mean the application is functioning correctly.

Infrastructure metrics and application logs should be evaluated together.

Restoring disk capacity does not automatically guarantee that previously failed business transactions have been recovered.

## 8. Implementation Status

This was a simulated incident-response exercise. No Azure infrastructure was deployed or repaired.
