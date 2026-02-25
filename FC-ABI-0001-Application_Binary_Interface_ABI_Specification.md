# FemtoClaw Application Binary Interface (ABI) Specification

Document ID: FC-ABI-0001  
Version: 1.0.0  
Status: Normative Engineering Specification  
Date: 2026-02-25  
License: Apache License 2.0  

---

# 1. Purpose

This specification defines the FemtoClaw Application Binary Interface (ABI).

The ABI defines the binary-level contract between:

• FemtoClaw runtime core  
• Capability modules (Claws)  
• Extension modules  
• Plugin modules  
• External integration modules  

The ABI ensures:

• Binary compatibility  
• Safe runtime extension  
• Capability isolation  
• Deterministic module execution  
• Version compatibility  
• Safe dynamic loading  

This document is normative.

All FemtoClaw runtimes and capability modules MUST conform to this ABI specification.

---

# 2. ABI Scope

The FemtoClaw ABI governs:

• Module loading  
• Capability invocation  
• Data representation  
• Memory ownership boundaries  
• Symbol resolution  
• Version compatibility  
• Runtime-module interaction  

The ABI defines the contract at binary execution level.

The ABI applies to:

• Dynamically loaded modules  
• Statically linked modules  
• Runtime-integrated modules  

---

# 3. ABI Design Principles

The FemtoClaw ABI MUST satisfy the following principles:

Deterministic execution  
Memory safety  
Isolation of modules  
Binary stability across versions  
Explicit ownership boundaries  
Strict runtime authority control  

The ABI MUST prevent undefined behavior.

---

# 4. ABI Compatibility Model

The ABI defines two compatibility levels:

ABI-Compatible  
ABI-Incompatible  

ABI-compatible modules MUST function correctly without recompilation.

ABI-incompatible modules MUST be rejected by the runtime.

Compatibility MUST be verified during module load.

---

# 5. Module Binary Requirements

FemtoClaw modules MUST be compiled as one of the following:

Dynamic library (.so, .dylib, .dll)  
Static library (.a) (if statically linked runtime)  

Modules MUST export required ABI symbols.

Modules MUST NOT depend on undefined symbols.

Modules MUST be verifiable.

---

# 6. Required Exported Symbols

All capability modules MUST export the following symbols:

femtoclaw_module_version  
femtoclaw_module_init  
femtoclaw_module_execute  
femtoclaw_module_shutdown  

Symbol names MUST match exactly.

Missing symbols MUST cause module load failure.

---

# 7. Module Version Symbol

The module MUST export:

femtoclaw_module_version

Signature:

const char* femtoclaw_module_version();

This function MUST return ABI version string.

Example:

"FC-ABI-1.0.0"

The runtime MUST verify version compatibility.

---

# 8. Module Initialization Symbol

Signature:

int femtoclaw_module_init(const RuntimeContext* ctx);

This function initializes module.

Initialization MUST:

Register capability  
Initialize internal state  
Validate runtime compatibility  

Return values:

0 → success  
non-zero → failure  

Failure MUST prevent module loading.

---

# 9. Module Execute Symbol

Signature:

int femtoclaw_module_execute(
    const ExecutionRequest* request,
    ExecutionResponse* response
);

This function executes capability.

Execution MUST:

Use provided request structure  
Write response to response structure  
NOT access runtime memory directly  

Execution MUST remain within defined ABI contract.

---

# 10. Module Shutdown Symbol

Signature:

void femtoclaw_module_shutdown();

This function allows module cleanup.

Shutdown MUST:

Release internal resources  
Preserve runtime integrity  

Shutdown MUST NOT corrupt runtime state.

---

# 11. Runtime Context Structure

RuntimeContext provides runtime interface to module.

Example structure:

struct RuntimeContext {
    uint32_t abi_version;
    uint64_t runtime_id;
    const RuntimeFunctions* functions;
};

RuntimeContext MUST be read-only.

Modules MUST NOT modify RuntimeContext.

---

# 12. ExecutionRequest Structure

ExecutionRequest defines capability invocation request.

Example:

struct ExecutionRequest {
    uint64_t execution_id;
    const char* capability;
    const char* arguments_json;
};

Request MUST be immutable.

Modules MUST NOT modify request.

---

# 13. ExecutionResponse Structure

ExecutionResponse defines execution result.

Example:

struct ExecutionResponse {
    int status_code;
    const char* result_json;
};

Modules MUST populate response.

Response MUST be valid structured data.

---

# 14. Memory Ownership Rules

Memory ownership MUST be explicit.

Rules:

Runtime owns RuntimeContext  
Runtime owns ExecutionRequest  
Module owns internal state  
Response ownership defined by runtime contract  

Modules MUST NOT free runtime memory.

Runtime MUST NOT corrupt module memory.

Ownership violations MUST be prevented.

---

# 15. Isolation Requirements

Modules MUST operate in isolation.

Modules MUST NOT:

Modify runtime internal memory  
Access unauthorized memory  
Bypass runtime authority  

Isolation preserves runtime safety.

---

# 16. Symbol Resolution Requirements

Runtime MUST resolve required symbols during module load.

Missing symbols MUST cause module load failure.

Symbol mismatch MUST cause module rejection.

Runtime MUST verify symbol signatures.

---

# 17. ABI Versioning Model

ABI version MUST follow semantic versioning:

MAJOR.MINOR.PATCH

Example:

1.0.0

Compatibility rules:

Major version mismatch → incompatible  
Minor version mismatch → compatible  
Patch version mismatch → compatible  

Runtime MUST enforce compatibility rules.

---

# 18. Binary Compatibility Requirements

Modules compiled against compatible ABI MUST function correctly.

Binary compatibility MUST NOT require recompilation.

ABI stability MUST be preserved.

Breaking ABI changes MUST increment major version.

---

# 19. Dynamic Loading Requirements

Runtime MAY dynamically load modules.

Runtime MUST verify:

ABI version  
Required symbols  
Compatibility  

Invalid modules MUST be rejected.

---

# 20. Static Linking Requirements

Statically linked modules MUST conform to ABI contract.

Static linking MUST preserve ABI rules.

Static modules MUST behave identically to dynamic modules.

---

# 21. Security Requirements

ABI MUST prevent:

Memory corruption  
Unauthorized execution  
Runtime compromise  

Runtime MUST verify module compatibility.

Runtime MUST enforce execution authority.

---

# 22. Error Handling Requirements

ABI violations MUST cause module rejection.

Runtime MUST emit telemetry for ABI violations.

ABI violations MUST NOT compromise runtime integrity.

---

# 23. Observability Requirements

ABI events MUST emit telemetry:

Module load  
Module init  
Module execution  
Module shutdown  

Telemetry MUST enable auditability.

---

# 24. Compliance Requirements

Compliant FemtoClaw implementations MUST:

Implement ABI contract  
Verify ABI compatibility  
Reject incompatible modules  
Preserve runtime integrity  

Non-compliant implementations are not FemtoClaw compliant.

---

# 25. ABI Guarantees

FemtoClaw ABI guarantees:

Binary compatibility  
Safe module execution  
Runtime integrity preservation  
Secure runtime extension  

The ABI enables safe industrial runtime extensibility.

---

# 26. Conclusion

This specification defines the FemtoClaw Application Binary Interface.

The ABI ensures safe interaction between runtime and capability modules.

The ABI enables binary compatibility, safe extensibility, and runtime integrity.

All FemtoClaw implementations and modules MUST conform to this ABI specification.

End of Specification