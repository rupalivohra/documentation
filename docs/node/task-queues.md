---
id: task-queues
title: Task Queues in Node
sidebar_label: Task Queues
---

## What is a Task Queue?

import SharedTaskQueuesBasic from '../shared/task-queues-basic.md'

<SharedTaskQueuesBasic
workflowLink="/docs/java/workflows"
workerLink="/docs/java/workers"
/>

## How to use Task Queues

In Node, a Task Queue is represented in code by name, as a `string`.
There are 3 places where the name of the Task Queue is supplied by the developer.

1. When starting a Workflow, you must pass the `taskQueue` option to the [WorkflowClient's `createWorkflowHandle()` method](https://nodejs.temporal.io/api/classes/client.workflowclient#newworkflowhandle).

```ts
const workflow = workflowClient.createWorkflowHandle(myWorkflow, {
  taskQueue: 'my-task-queue',
});

const result = await workflow.execute();
```

2. When creating a Worker, you must pass the `taskQueue` option to the [`Worker.create()` function](https://nodejs.temporal.io/api/classes/worker.worker-1#create).

```ts
const worker = await Worker.create({
  workDir: __dirname,
  taskQueue: 'my-task-queue',
});
```

3. Optionally, in Workflow code, when calling an Activity, you can specify the task queue by passing the `taskQueue` option to [`createActivityHandle()`](https://nodejs.temporal.io/api/namespaces/workflow#configureactivities). If you do not specify a `taskQueue`, then the Node SDK places Activity Tasks in the same Task Queue as the Workflow Task Queue.

```ts
const { greet } = createActivityHandle<typeof activities>({
  taskQueue: 'my-other-task-queue',
  startToCloseTimeout: '1s',
});
```
