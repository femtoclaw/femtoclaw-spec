# FemtoClaw Runtime State Machine Specification

Version: 1.0.0
Status: Normative Engineering Specification
Date: 2026-02-25
License: Apache License 2.0

---

# 1. Purpose

This document defines the FemtoClaw Runtime State Machine.

The Runtime State Machine governs all execution lifecycle transitions within the FemtoClaw runtime.

The state machine ensures:

Deterministic execution behavior
Execution integrity
Failure containment
Authorization enforcement
Protocol enforcement
Auditability of execution state transitions

This specification defines:

* Runtime state definitions
* State transition model
* State transition constraints
* Execution lifecycle states
* Failure state handling
* State integrity requirements
* Compliance requirements

This document is normative.

All FemtoClaw implementations MUST conform to this runtime state machine specification.

---

# 2. State Machine Overview

The FemtoClaw runtime operates as a deterministic finite state machine.

At any moment, the runtime MUST be in exactly one state.

State transitions occur in response to defined runtime events.

State transitions MUST be deterministic.

State transitions MUST preserve runtime integrity.

State transitions MUST NOT occur outside defined state transition rules.

---

# 3. Runtime State Definitions

The FemtoClaw runtime defines the following states:

INITIALIZING
IDLE
INPUT_RECEIVED
PROTOCOL_VALIDATING
AUTHORIZATION_EVALUATING
CAPABILITY_EXECUTING
MEMORY_COMMITTING
COMPLETED
ERROR
SHUTDOWN

Each state has defined behavior and permitted transitions.

---

# 4. INITIALIZING State

INITIALIZING is the runtime startup state.

INITIALIZING responsibilities include:

Loading configuration
Initializing memory subsystem
Initializing capability registry
Initializing policy engine
Initializing observability system

Permitted transitions:

INITIALIZING → IDLE
INITIALIZING → ERROR

INITIALIZING MUST complete before accepting input.

---

# 5. IDLE State

IDLE state represents runtime readiness.

IDLE responsibilities include:

Waiting for input
Maintaining readiness

Permitted transitions:

IDLE → INPUT_RECEIVED
IDLE → SHUTDOWN

IDLE state MUST NOT execute capabilities.

---

# 6. INPUT_RECEIVED State

INPUT_RECEIVED state represents receipt of execution input.

Responsibilities include:

Recording input
Assigning execution identifier
Preparing protocol validation

Permitted transitions:

INPUT_RECEIVED → PROTOCOL_VALIDATING
INPUT_RECEIVED → ERROR

INPUT_RECEIVED MUST NOT execute capabilities.

---

# 7. PROTOCOL_VALIDATING State

PROTOCOL_VALIDATING state validates protocol structure.

Responsibilities include:

Parsing protocol message
Validating capability identifier
Validating argument structure

Permitted transitions:

PROTOCOL_VALIDATING → AUTHORIZATION_EVALUATING
PROTOCOL_VALIDATING → ERROR

Invalid protocol MUST transition to ERROR.

Protocol validation MUST occur before authorization.

---

# 8. AUTHORIZATION_EVALUATING State

AUTHORIZATION_EVALUATING state evaluates execution authorization.

Responsibilities include:

Checking capability registry
Evaluating policy engine
Determining authorization decision

Permitted transitions:

AUTHORIZATION_EVALUATING → CAPABILITY_EXECUTING
AUTHORIZATION_EVALUATING → ERROR

Unauthorized execution MUST transition to ERROR.

Authorization MUST complete before execution.

---

# 9. CAPABILITY_EXECUTING State

CAPABILITY_EXECUTING state executes authorized capability.

Responsibilities include:

Executing capability
Capturing execution result
Capturing execution status

Permitted transitions:

CAPABILITY_EXECUTING → MEMORY_COMMITTING
CAPABILITY_EXECUTING → ERROR

Capability execution MUST be isolated.

Capability execution MUST NOT bypass runtime authority.

---

# 10. MEMORY_COMMITTING State

MEMORY_COMMITTING state records execution results.

Responsibilities include:

Recording execution result
Recording authorization decision
Recording execution telemetry

Permitted transitions:

MEMORY_COMMITTING → COMPLETED
MEMORY_COMMITTING → ERROR

Memory committing MUST preserve audit integrity.

Memory committing MUST be atomic.

---

# 11. COMPLETED State

COMPLETED state represents successful execution completion.

Responsibilities include:

Finalizing execution record
Emitting completion telemetry

Permitted transitions:

COMPLETED → IDLE

COMPLETED state MUST preserve execution auditability.

---

# 12. ERROR State

ERROR state represents execution failure.

Responsibilities include:

Recording error
Emitting error telemetry
Preserving runtime integrity

Permitted transitions:

ERROR → IDLE
ERROR → SHUTDOWN

ERROR state MUST NOT corrupt runtime state.

ERROR state MUST preserve auditability.

---

# 13. SHUTDOWN State

SHUTDOWN state represents runtime termination.

Responsibilities include:

Finalizing memory persistence
Finalizing telemetry
Releasing resources

Permitted transitions:

SHUTDOWN → TERMINATED

SHUTDOWN MUST preserve memory integrity.

---

# 14. State Transition Integrity Requirements

State transitions MUST be valid.

Invalid state transitions MUST NOT occur.

State transitions MUST preserve runtime integrity.

State transitions MUST be deterministic.

---

# 15. State Transition Atomicity

State transitions MUST be atomic.

Partial state transitions MUST NOT occur.

State transitions MUST preserve runtime consistency.

---

# 16. State Machine Determinism Requirements

Given identical input and runtime state, state transitions MUST be identical.

State machine behavior MUST be reproducible.

State machine determinism enables audit verification.

---

# 17. State Isolation Requirements

Each execution cycle MUST operate independently.

State transitions MUST NOT interfere with other execution cycles.

State isolation preserves runtime correctness.

---

# 18. State Observability Requirements

All state transitions MUST emit telemetry.

Telemetry MUST include:

Execution identifier
Previous state
New state
Timestamp

State observability enables audit and diagnosis.

---

# 19. Failure Handling Requirements

Failure MUST transition runtime to ERROR state.

ERROR state MUST preserve runtime integrity.

ERROR state MUST prevent unsafe execution continuation.

Failure MUST NOT corrupt runtime state machine.

---

# 20. Compliance Requirements

Conforming FemtoClaw implementations MUST:

Implement defined runtime states.

Enforce valid state transitions.

Prevent invalid transitions.

Emit state transition telemetry.

Preserve runtime integrity.

Non-compliant implementations are not FemtoClaw compliant.

---

# 21. State Machine Guarantees

FemtoClaw Runtime State Machine guarantees:

Deterministic execution lifecycle.

Execution integrity preservation.

Failure containment.

Authorization enforcement.

Auditability of execution state transitions.

---

# 22. Example Execution State Transition

Example execution lifecycle:

INITIALIZING → IDLE
IDLE → INPUT_RECEIVED
INPUT_RECEIVED → PROTOCOL_VALIDATING
PROTOCOL_VALIDATING → AUTHORIZATION_EVALUATING
AUTHORIZATION_EVALUATING → CAPABILITY_EXECUTING
CAPABILITY_EXECUTING → MEMORY_COMMITTING
MEMORY_COMMITTING → COMPLETED
COMPLETED → IDLE

Execution completes deterministically.

---

# 23. Conclusion

This specification defines the FemtoClaw Runtime State Machine.

The state machine governs all execution lifecycle transitions.

The state machine ensures deterministic execution, authorization enforcement, and runtime integrity.

All FemtoClaw implementations MUST conform to this runtime state machine specification.
