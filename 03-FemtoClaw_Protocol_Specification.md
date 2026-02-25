# FemtoClaw Protocol Specification

Version: 1.0.0
Status: Normative Engineering Specification
Date: 2026-02-25
License: Apache License 2.0

---

# 1. Purpose

This document defines the FemtoClaw Runtime Execution Protocol.

The FemtoClaw Protocol establishes a strict, structured interface between probabilistic inference systems and the deterministic FemtoClaw runtime execution authority.

The protocol ensures:

* Deterministic execution mediation
* Structured execution intent representation
* Capability-gated operating system interaction
* Execution safety and authorization enforcement
* Elimination of natural language execution ambiguity

This protocol prevents inference systems from directly executing operating system commands.

Inference systems express intent.

FemtoClaw authorizes execution.

FemtoClaw performs execution.

This document is normative.

All FemtoClaw runtime implementations MUST conform to this protocol specification.

---

# 2. Design Goals

The FemtoClaw Protocol is designed to achieve the following goals.

## 2.1 Deterministic Execution Mediation

All execution intent MUST be expressed in structured form.

Natural language MUST NOT trigger execution.

Execution intent MUST be machine-verifiable.

---

## 2.2 Safety by Structure

Protocol structure MUST eliminate ambiguity.

Protocol MUST allow strict validation.

Protocol MUST prevent unauthorized execution.

---

## 2.3 Capability-Gated Execution

Protocol MUST express execution intent only through registered capabilities.

Protocol MUST NOT allow arbitrary execution.

Protocol MUST require explicit capability identification.

---

## 2.4 Deterministic Validation

Protocol MUST allow deterministic validation.

Validation MUST produce identical results for identical inputs.

---

## 2.5 Auditability

Protocol messages MUST be auditable.

Protocol messages MUST be preserved in execution history.

Protocol messages MUST allow reconstruction of execution intent.

---

# 3. Protocol Overview

FemtoClaw Protocol defines two mutually exclusive output forms:

1. Message Form
2. Tool Call Form

Only one form may be present per protocol message.

Protocol messages MUST be valid JSON.

Protocol messages MUST conform to schema defined in this specification.

---

# 4. Protocol Message Structure

All protocol messages MUST be valid UTF-8 encoded JSON objects.

Top-level structure MUST be exactly one of:

* message
* tool_call

Protocol messages MUST NOT contain both.

Protocol messages MUST NOT contain unknown execution fields.

---

# 5. Message Form

## 5.1 Definition

Message Form represents human-readable output.

Message Form MUST NOT trigger execution.

Message Form MUST NOT invoke capabilities.

---

## 5.2 Message Schema

```
{
  "message": {
    "content": "string"
  }
}
```

---

## 5.3 Field Definitions

content
Type: string
Description: Human-readable output

Content MUST be UTF-8 encoded.

Content MUST NOT be interpreted as executable command.

---

## 5.4 Runtime Behavior

Runtime MUST:

* Accept Message Form
* Record message in memory
* Emit observability telemetry

Runtime MUST NOT execute Message Form.

---

# 6. Tool Call Form

## 6.1 Definition

Tool Call Form represents execution intent.

Tool Call Form requests capability execution.

Tool Call Form MUST be validated and authorized before execution.

---

## 6.2 Tool Call Schema

```
{
  "tool_call": {
    "tool": "string",
    "args": {}
  }
}
```

---

## 6.3 Field Definitions

tool
Type: string
Description: Capability identifier

args
Type: object
Description: Capability arguments

Arguments MUST conform to capability specification.

Arguments MUST be structured JSON.

Arguments MUST NOT contain executable code.

---

# 7. Capability Identifier Requirements

Capability identifier MUST:

* Match registered capability name
* Be exact match
* Be case-sensitive
* Be registered with runtime

Unknown capability identifiers MUST be rejected.

---

# 8. Argument Validation

Arguments MUST:

* Be valid JSON object
* Match capability argument schema
* Contain valid types
* Contain valid structure

Invalid arguments MUST cause protocol rejection.

---

# 9. Protocol Validation Requirements

Protocol Validator MUST enforce:

* Valid JSON structure
* Correct protocol form
* Mutual exclusivity of message and tool_call
* Valid capability identifier
* Valid argument structure

Invalid protocol messages MUST be rejected.

Invalid protocol messages MUST NOT execute.

---

# 10. Protocol Validation Algorithm

Protocol validation MUST execute following steps:

Step 1: Parse JSON
Step 2: Verify top-level structure
Step 3: Verify protocol form
Step 4: Verify capability identifier (if present)
Step 5: Verify argument structure
Step 6: Accept or reject message

Validation MUST be deterministic.

Validation MUST be complete before authorization.

---

# 11. Execution Authorization Interaction

Protocol validation occurs before capability authorization.

Protocol validation ensures structural correctness.

Capability authorization ensures execution permission.

Both validation and authorization MUST succeed before execution.

---

# 12. Protocol Failure Handling

Protocol failures include:

Invalid JSON
Malformed structure
Unknown capability
Invalid arguments
Protocol ambiguity

Protocol failure MUST:

* Prevent execution
* Emit observability telemetry
* Record validation failure

Protocol failure MUST preserve runtime safety.

---

# 13. Protocol Security Model

Protocol prevents execution vulnerabilities including:

Prompt injection
Natural language execution interpretation
Unauthorized capability execution
Execution ambiguity

Protocol ensures execution intent is explicit and verifiable.

---

# 14. Protocol Determinism Requirements

Protocol validation MUST be deterministic.

Identical protocol messages MUST produce identical validation results.

Protocol interpretation MUST NOT depend on runtime state outside defined inputs.

---

# 15. Protocol Versioning

Protocol version MUST remain backward compatible.

Protocol changes MUST:

* Preserve compatibility
* Introduce explicit version identifiers if breaking

Protocol versioning ensures long-term stability.

---

# 16. Observability Requirements

Runtime MUST emit observability telemetry for protocol events including:

Protocol received
Protocol validated
Protocol rejected
Capability requested

Observability ensures auditability.

---

# 17. Compliance Requirements

Conforming FemtoClaw implementations MUST:

Implement protocol validator
Enforce protocol structure
Reject invalid protocol messages
Prevent unauthorized execution
Record protocol messages in audit log

Non-conforming implementations are not FemtoClaw compliant.

---

# 18. Protocol Guarantees

FemtoClaw Protocol guarantees:

Explicit execution intent
Deterministic execution mediation
Capability-gated execution
Execution safety enforcement
Full auditability

Protocol ensures inference cannot directly execute operating system commands.

---

# 19. Reference Examples

## Valid Message Form

```
{
  "message": {
    "content": "Operation complete."
  }
}
```

## Valid Tool Call Form

```
{
  "tool_call": {
    "tool": "shell",
    "args": {
      "bin": "echo",
      "argv": ["Hello"]
    }
  }
}
```

## Invalid Protocol Example

```
{
  "execute": "rm -rf /"
}
```

This MUST be rejected.

---

# 20. Conclusion

This specification defines the FemtoClaw Runtime Execution Protocol.

The protocol establishes the secure, deterministic interface between inference systems and execution authority.

The protocol ensures execution safety, capability authorization, and runtime correctness.

All FemtoClaw implementations MUST conform to this protocol specification.
