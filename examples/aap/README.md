# Sanitized AAP Example

This example shows the kind of operational playbook we found most useful around a private AI application.

The pattern is simple:
- gather cluster version
- gather cluster operator health
- gather app deployment readiness
- gather InferenceService state
- write one summary artifact

That gives you a repeatable pre-demo and pre-change snapshot without embedding product secrets.
