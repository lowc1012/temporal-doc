---
id: how-to-create-a-schedule-in-go
title: How to create a Schedule in Go
sidebar_label: Create Schedule
description: To create a Schedule in Go, use `Create()` on the Client.
tags:
- go sdk
- code sample
- schedule
- create
---

<!-- DO NOT EDIT THIS FILE DIRECTLY.
THIS FILE IS GENERATED from https://github.com/temporalio/documentation-samples-go/blob/main/schedule/create/main_dacx.go. -->

Schedules are initiated with the `create` call.
The user generates a unique Schedule ID for each new Schedule.

To create a Schedule in Go, use `Create()` on the [Client](/concepts/what-is-a-temporal-client).
Schedules must be initialized with a Schedule ID, [Spec](/concepts/what-is-a-schedule#spec), and [Action](/concepts/what-is-a-schedule#action) in `client.ScheduleOptions{}`.

:::copycode Sample application code

The following code sample comes from a working and tested sample application.
The code sample might be abridged within the guide to highlight key aspects.
Visit the source repository to [view the source code](https://github.com/temporalio/documentation-samples-go/blob/main/schedule/create/main_dacx.go) in the context of the rest of the application code.

:::

```go
func main() {
// ...
	scheduleID := "schedule_id"
	workflowID := "schedule_workflow_id"
	// Create the schedule.
	scheduleHandle, err := temporalClient.ScheduleClient().Create(ctx, client.ScheduleOptions{
		ID:   scheduleID,
		Spec: client.ScheduleSpec{},
		Action: &client.ScheduleWorkflowAction{
			ID:        workflowID,
			Workflow:  schedule.ScheduleWorkflow,
			TaskQueue: "schedule",
		},
	})
// ...
}
// ...
```
