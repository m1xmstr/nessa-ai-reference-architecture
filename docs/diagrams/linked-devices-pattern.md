# Linked Devices Pattern Diagram

This diagram is a product-level pattern. It does not describe Secure Connector protocol, pairing, token exchange, heartbeat schema, job payloads, or transport mechanics.

```mermaid
flowchart LR
  User["User"] --> Consent["Approve device participation"]
  Consent --> Device["User-owned private device"]
  Device --> Health["Controlled readiness signal"]
  Device --> Runtime["Local runtime (optional MTP preview)"]
  User --> Request["Request using private compute"]
  Request --> Nessa["Nessa policy and routing"]
  Health --> Nessa
  Runtime --> Health
  Nessa --> Ready{"Device ready and allowed?"}
  Ready -->|yes| Compute["Private local execution"]
  Ready -->|preview unavailable| Fallback["Use allowed local fallback with notice"]
  Ready -->|private route forbidden| Fail["Fail closed"]
  Compute --> Result["Result with route truth"]
  Fallback --> Result
  Fail --> Guidance["Unavailable or setup guidance"]
  Result --> User
  Guidance --> User
```
