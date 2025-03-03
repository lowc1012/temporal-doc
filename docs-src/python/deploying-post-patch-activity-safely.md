---
id: deploying-post-patch-activity-safely
title: Safe Deployment of post_patch_activity
sidebar_label: Deploy new code
description: Guidelines for safely deploying post_patch_activity by ensuring that all related Workflows are completed.
tags:
- deployment safety
- python sdk
- workflow completion
- best practices
---

<!-- DO NOT EDIT THIS FILE DIRECTLY.
THIS FILE IS GENERATED from https://github.com/temporalio/documentation-samples-python/blob/main/version_your_workflows/workflow_4_patch_complete_dacx.py. -->

You can safely deploy `post_patch_activity` once all Workflows labeled my-patch or earlier are finished, based on the previously mentioned assertion.

:::copycode Sample application code

The following code sample comes from a working and tested sample application.
The code sample might be abridged within the guide to highlight key aspects.
Visit the source repository to [view the source code](https://github.com/temporalio/documentation-samples-python/blob/main/version_your_workflows/workflow_4_patch_complete_dacx.py) in the context of the rest of the application code.

:::

```python
# ...
@workflow.defn
class MyWorkflow:
    @workflow.run
    async def run(self) -> None:
        self._result = await workflow.execute_activity(
            post_patch_activity,
            schedule_to_close_timeout=timedelta(minutes=5),
        )
```
