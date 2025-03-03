---
id: describeid
title: tctl workflow describeid
sidebar_label: describeid
description: How to show information about a Workflow Execution for a specified Workflow Id and optional Run Id using tctl.
tags:
  - tctl
  - cli reference
---

The `tctl workflow describeid` command shows information about a [Workflow Execution](/workflows#workflow-execution) for the specified [Workflow Id](/concepts/what-is-a-workflow-id) and optional [Run Id](/concepts/what-is-a-run-id).

`tctl workflow describeid <workflow_id> <run_id> <modifiers>`

This command is a shortcut for `tctl workflow describe --workflow_id <workflowid> --run_id <runid>`.

The following modifiers control the behavior of the command.

### --print_raw

Print properties exactly as they are stored.

**Example**

```bash
tctl workflow describeid <workflow_id> <id> --print_raw
```

### --reset_points_only

Show only events that are eligible for reset.

**Example**

```bash
tctl workflow describeid <workflow_id> --reset_points_only
```
