# Verification Summary

## Exact revision

- Contract source SHA-256: `DB5A512A00EBD6C120A988C1EF0831494335B2B2957707BE81FE8C112CF593DA`
- Test source SHA-256: `29574141281349892C4A6C8ABBF05ACAB659B1563AF0C85846069E35B9B5F190`
- Requirements SHA-256: `C023A5E2685EA5A49CE475FAAF43ADE6D8E2FF3E68BEFF2276ECC8E8152B7131`
- GenLayer runtime family: `v0.6.0-rc5`
- Pinned packages: `genlayer-py==0.19.0rc2`, `genlayer-test==0.30.0rc2`, `genvm-linter==0.11.1rc2`

## Reproducible verification

Install the pinned requirements, then run these commands from the repository
root:

```text
python -m pytest tests -q
genvm-lint check contracts/boil_water_rescission_evidence_covenant.py
pip check
```

Results: `13 passed`; validation passed for 9 public methods (5 view, 4
write); no broken requirements.

## Coverage represented by the tests

- owner-only registration, sealing, and supersession;
- public assessment only after sealing;
- all five decision fields bound into the stored result;
- clearable and unresolved outcomes;
- bounded explanations and malformed-output rejection;
- hostile evidence escaping and content digests;
- external source failure without state mutation;
- validator differential rejection;
- review-ID replay idempotency and version binding;
- registration hash, origin, URL, and lifecycle invariants.
