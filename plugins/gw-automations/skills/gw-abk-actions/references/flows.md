# Flows

Lifecycle actions that bookend a Gradient Works flow. Place Start at the top and Finish at the bottom of every flow to enable execution tracing.

## Execute Subflow

**Action class:** `GWFXExecuteSubflowAction`

<aside class="warning">
This is an advanced action. If your subflow is not optimized or this action
is used repeatedly (e.g. in a loop), your Flow may run poorly or fail.
</aside>

Flow provides the ability to run a subflow natively through the [Subflow
element](https://help.salesforce.com/articleView?id=sf.flow_ref_elements_subflow.htm&type=5).
However, this element does not always work in scheduled paths of Flows, especially
Record-Triggered Flows. This action provides the ability to invoke a Flow immediately
or schedule it to run at a future date.

The native Subflow element also requires you to select the Flow from a list
of known Flows, preventing you from using logic in the calling Flow to choose
a specific subflow to execute. This action provides the ability to invoke a
Flow using the value of a resource in the calling Flow. This allows you to
dynamically execute a subflow based on the result of a decision or formula
in the calling Flow.

Please note the following limitations:

* Subflows must be active and auto-launched without a trigger
* Only Text, Number and Boolean types are supported for inputs
* Subflow outputs are not currently accessible from this action

### Inputs

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `flowApiName` | `String` | Yes | The API name of the Flow to execute |
| `executionDelay` | `Integer` | No | If Flow is scheduled, number of minutes to wait before invoking Flow |
| `executionSchedule` | `String` | No | One of 'immediate' or 'scheduled'. Should the Flow be invoked immediately or scheduled to run later. |
| `flowInputData` | `String` | No | Data that describes the Flow inputs. See action overview for more information about supported inputs. |
| `flowInterviewGuid` | `String` | No | Automatically provided as $Flow.InterviewGuid. |

### Outputs

_None._

---

## Failed

**Action class:** `FailedFlowAction`

Pair this action with StartFlowAction. Use it inside a fault path in a Flow
to ensure proper Flow execution error tracing.

### Inputs

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `flowFaultMessage` | `String` | Yes | Automatically provided as $Flow.FaultMessage |
| `flowInterviewGuid` | `String` | Yes | Automatically provided as $Flow.InterviewGuid. |
| `flowApiName` | `String` | No | The API name of the flow. Automatically provided if available. |

### Outputs

| Field | Type | Description |
|-------|------|-------------|
| `flowFaultMessage` | `String` | The Flow fault path message |

---

## Finish

**Action class:** `FinishFlowAction`

Pair this action with StartFlowAction. Use it at the very end of the Flow
to ensure proper Flow execution tracing.

### Inputs

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `flowInterviewGuid` | `String` | Yes | Automatically provided as $Flow.InterviewGuid. |
| `flowApiName` | `String` | No |  |

### Outputs

| Field | Type | Description |
|-------|------|-------------|
| `flowInterviewGuid` | `String` | The interview GUID |

---

## Resume

**Action class:** `ResumeFlowAction`

Pair this with StartFlowAction and FinishFlowAction. Use this action after a
Screen Flow action to resume execution tracing for individual executions of
the Flow ("interviews" in Flow-speak). Screen Flow actions can cause the
transaction for the Flow execution to change which makes execution tracing
difficult. Resuming tracking of the original transaction from the
StartFlowAction will allow us to continue to track the full Flow execution
lifecycle.

### Inputs

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `flowInterviewGuid` | `String` | Yes | The StartFlowAction interview GUID. |
| `flowApiName` | `String` | No | The API name of the flow |

### Outputs

| Field | Type | Description |
|-------|------|-------------|
| `flowInterviewGuid` | `String` | The interview GUID |

---

## Start

**Action class:** `StartFlowAction`

Use this action at the very beginning of a Flow to turn on execution tracing
for individual executions of the Flow ("interviews" in Flow-speak). Make sure
to also place a FinishFlowAction at the end of your Flow.

### Inputs

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `flowInterviewGuid` | `String` | Yes | Automatically provided as $Flow.InterviewGuid. |
| `flowApiName` | `String` | No | The API name of the flow. Automatically provided if available. |
| `recordId` | `Id` | No | Record Id of the record being used in the Flow. |

### Outputs

| Field | Type | Description |
|-------|------|-------------|
| `flowInterviewGuid` | `String` | The interview GUID |

---
