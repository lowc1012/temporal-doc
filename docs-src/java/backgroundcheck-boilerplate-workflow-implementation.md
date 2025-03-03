---
id: backgroundcheck-boilerplate-workflow-implementation
title: Boilerplate Workflow Implementation
sidebar_label: Workflow code
description: In the Temporal Java SDK, a Workflow Definition is an interface and its implementation.
tags:
- java sdk
- developer guide
- workflow
- code sample
---

<!-- DO NOT EDIT THIS FILE DIRECTLY.
THIS FILE IS GENERATED from https://github.com/temporalio/documentation-samples-java/blob/main/backgroundcheck/src/main/java/backgroundcheckboilerplate/BackgroundCheckBoilerplateWorkflowImpl.java. -->

Now that you've defined your Workflow Interface you can define its implementation.

:::copycode Sample application code

The following code sample comes from a working and tested sample application.
The code sample might be abridged within the guide to highlight key aspects.
Visit the source repository to [view the source code](https://github.com/temporalio/documentation-samples-java/blob/main/backgroundcheck/src/main/java/backgroundcheckboilerplate/BackgroundCheckBoilerplateWorkflowImpl.java) in the context of the rest of the application code.

:::

```java
import io.temporal.activity.ActivityOptions;
import io.temporal.workflow.Workflow;

import java.time.Duration;


public class BackgroundCheckBoilerplateWorkflowImpl implements BackgroundCheckBoilerplateWorkflow {

  // Define the Activity Execution options
  // StartToCloseTimeout or ScheduleToCloseTimeout must be set
  ActivityOptions options = ActivityOptions.newBuilder()
          .setStartToCloseTimeout(Duration.ofSeconds(5))
          .build();

  // Create an client stub to activities that implement the given interface
  private final BackgroundCheckBoilerplateActivities activities =
      Workflow.newActivityStub(BackgroundCheckBoilerplateActivities.class, options);

  @Override
  public String backgroundCheck(String socialSecurityNumber) {
    String ssnTraceResult = activities.ssnTraceActivity(socialSecurityNumber);
    return ssnTraceResult;
  }

}
```
