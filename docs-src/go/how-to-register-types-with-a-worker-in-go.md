---
id: how-to-register-types-with-a-worker-in-go
title: How to register types with a Worker in Go
sidebar_label: Register Types
description: The `RegisterWorkflow()` and `RegisterActivity()` calls create an in-memory mapping between the Workflow Types and their implementations.
tags:
  - developer-guide-doc-type
  - go sdk
  - worker
  - how-to-doc-type
  - workflow and activity types
---

The `RegisterWorkflow()` and `RegisterActivity()` calls essentially create an in-memory mapping between the Workflow Types and their implementations, inside the Worker process.

**Registering Activity `structs`**

Per [Activity Definition](/go/how-to-develop-an-activity-definition-in-go) best practices, you might have an Activity struct that has multiple methods and fields.
When you use `RegisterActivity()` for an Activity struct, that Worker has access to all exported methods.

**Registering multiple Types**

To register multiple Activity Types and/or Workflow Types with the Worker Entity, just make multiple Activity registration calls, but make sure each Type name is unique:

```go
w.RegisterActivity(ActivityA)
w.RegisterActivity(ActivityB)
w.RegisterActivity(ActivityC)
w.RegisterWorkflow(WorkflowA)
w.RegisterWorkflow(WorkflowB)
w.RegisterWorkflow(WorkflowC)
```
