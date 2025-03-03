---
id: backgroundcheck-boilerplate-self-hosted-worker
title: Customize Client options
sidebar_label: Self-hosted Client options
description: Configure the Temporal Client with the specific IP Address of the Temporal Server on your network.
tags:
- worker
- self-hosted
- developer guide
---

<!-- DO NOT EDIT THIS FILE DIRECTLY.
THIS FILE IS GENERATED from https://github.com/temporalio/documentation-samples-python/blob/main/backgroundcheck_boilerplate/self_hosted_worker/main_dacx.py. -->

Set IP address, port, and Namespace in the Temporal Client options.

:::copycode Sample application code

The following code sample comes from a working and tested sample application.
The code sample might be abridged within the guide to highlight key aspects.
Visit the source repository to [view the source code](https://github.com/temporalio/documentation-samples-python/blob/main/backgroundcheck_boilerplate/self_hosted_worker/main_dacx.py) in the context of the rest of the application code.

:::

```python
import asyncio

from temporalio.client import Client
from temporalio.worker import Worker

from activities.ssntraceactivity_dacx import ssn_trace_activity
from workflows.backgroundcheck_dacx import BackgroundCheck



async def main():
    client = await Client.connect(
        "172.18.0.4:7233"  # The IP address of the Temporal Server on your network.
    )

    worker = Worker(
        client,
        task_queue="background-check-boilerplate-task-queue",
        workflows=[BackgroundCheck],
        activities=[ssn_trace_activity],
    )
    await worker.run()


if __name__ == "__main__":
    asyncio.run(main())
```
