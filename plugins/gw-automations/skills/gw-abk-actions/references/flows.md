# Flows

Flow lifecycle and subflow execution actions. Place Start at the top and Finish at the bottom of every flow to enable execution tracing. Use Execute Subflow to dynamically invoke a subflow or to run subflows in scheduled paths where the native Subflow element does not work.

## Execute Subflow

**Action class:** `GWFXExecuteSubflowAction`

**Note:** This is an advanced action. If your subflow is not optimized or this action
is used repeatedly (e.g. in a loop), your Flow may run poorly or fail.

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
| `flowApiName` | `String` | Yes | The API name of the Flow to execute. |
| `executionDelay` | `Integer` | No | If the Flow is scheduled, the number of minutes to wait before invoking it. |
| `executionSchedule` | `String` | No | One of `immediate` or `scheduled`. Controls whether the Flow is invoked immediately or scheduled to run later. |
| `flowInputData` | `String` | No | Data that describes the Flow inputs. See action overview for more information about supported inputs. |
| `flowInterviewGuid` | `String` | No | Set this to `$Flow.InterviewGuid`. |

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
| `flowFaultMessage` | `String` | Yes | Set this to `$Flow.FaultMessage`. |
| `flowInterviewGuid` | `String` | Yes | Set this to `$Flow.InterviewGuid`. |
| `flowApiName` | `String` | No | The API name of the flow. |

### Outputs

| Field | Type | Description |
|-------|------|-------------|
| `flowFaultMessage` | `String` | The Flow fault path message. |

---

## Finish

**Action class:** `FinishFlowAction`

Pair this action with StartFlowAction. Use it at the very end of the Flow
to ensure proper Flow execution tracing.

### Inputs

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `flowInterviewGuid` | `String` | Yes | Set this to `$Flow.InterviewGuid`. |
| `flowApiName` | `String` | No | The API name of the flow. |

### Outputs

| Field | Type | Description |
|-------|------|-------------|
| `flowInterviewGuid` | `String` | The Flow interview GUID. |

---

## Resume

**Action class:** `ResumeFlowAction`

Pair this with StartFlowAction and FinishFlowAction. Use this action after a
Screen element in a Screen Flow to resume execution tracing. Screen elements
cause a new transaction to begin, which breaks the tracing started by
StartFlowAction. This action reconnects the trace so the full Flow execution
lifecycle can be tracked.

### Inputs

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `flowInterviewGuid` | `String` | Yes | Set this to the `flowInterviewGuid` output from the Start action. |
| `flowApiName` | `String` | No | The API name of the flow. |

### Outputs

| Field | Type | Description |
|-------|------|-------------|
| `flowInterviewGuid` | `String` | The Flow interview GUID. |

---

## Start

**Action class:** `StartFlowAction`

Use this action at the very beginning of a Flow to turn on execution tracing
for individual executions of the Flow ("interviews" in Flow-speak). Make sure
to also place a FinishFlowAction at the end of your Flow.

### Inputs

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `flowInterviewGuid` | `String` | Yes | Set this to `$Flow.InterviewGuid`. |
| `flowApiName` | `String` | No | The API name of the flow. |
| `recordId` | `Id` | No | The Id of the record being used in the Flow. |

### Outputs

| Field | Type | Description |
|-------|------|-------------|
| `flowInterviewGuid` | `String` | The Flow interview GUID. |

---
