# AI security evaluation methodology

This lab uses deterministic regression tests so security controls can be evaluated without relying on a live external model. When extending the project, keep evaluation scenarios explicit, reproducible, and tied to a control.

## Test categories

### Input validation
- oversize prompts and malformed payloads are rejected consistently
- unexpected tool names fail closed
- control characters and unusual Unicode do not bypass validation

### Sensitive-data handling
- representative secret-like values are redacted before model processing
- redaction does not log the original secret
- output filtering catches secret-like values introduced by the model adapter

### Tool authorization
- only allowlisted tools can be requested
- unknown or disallowed tools are denied by default
- authorization decisions are made outside model-generated text

### Prompt-injection resistance
- instruction-like user content cannot change the tool allowlist
- requests to reveal hidden policy or secrets remain data, not authority
- malicious content embedded in retrieved context is tested separately from direct user prompts

### Failure behavior
- model-adapter errors return controlled responses without leaking internals
- security controls continue to run when the model is unavailable
- event identifiers are emitted without storing raw sensitive prompts

## Evaluation records

For each security regression case, record the control being tested, synthetic input, expected decision, observed decision, and whether any sensitive value reached logs or output. Avoid vague claims such as "prompt injection proof"; document exactly which attack patterns were tested.

## Production extensions

A production evaluation suite should add model-specific datasets, retrieval/tenant authorization cases, multilingual/adversarial inputs, rate-limit abuse, tool-call chains, human-approval scenarios, and periodic re-evaluation when models, prompts, tools, or policies change.
