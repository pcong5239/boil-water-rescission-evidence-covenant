# Boil Water Rescission Evidence Covenant

An auditable GenLayer contract that turns multi-source public evidence about a boil-water advisory into a bounded, validator-checked clearance decision.

## Live deployment

- Network: GenLayer Studio Dev, Chain ID `61997`
- Contract: [`0x40b8a22210420FEED97F3E54782d9Ee5f13afD3d`](https://explorer-studio-dev.genlayer.com/address/0x40b8a22210420FEED97F3E54782d9Ee5f13afD3d)
- Deploy transaction: [`0x630c14a39d45ba8ea44b27658ad6ebb79aaf4db41762f32a11067b9819e6180c`](https://explorer-studio-dev.genlayer.com/tx/0x630c14a39d45ba8ea44b27658ad6ebb79aaf4db41762f32a11067b9819e6180c)
- Deployer: actor7, `0x8581c4a532dd3f9b163b12809b1bd089f367147f`
- Final assessment: [`0x614bec7179e572ceea7530a8c1c9b4bd48835d5d2993abbad391bb3bcca25478`](https://explorer-studio-dev.genlayer.com/tx/0x614bec7179e572ceea7530a8c1c9b4bd48835d5d2993abbad391bb3bcca25478)
- Counterexample, wrong service area: [`0x997be343bcd3353978cf9512b4f5345572294615de07e987e532d8bdd7acc157`](https://explorer-studio-dev.genlayer.com/tx/0x997be343bcd3353978cf9512b4f5345572294615de07e987e532d8bdd7acc157)
- Authorization failure, non-owner seal attempt: [`0x45bb4f9247f27c30284794a8212f2e67049cb3058c7cc3c29758beda25fb0802`](https://explorer-studio-dev.genlayer.com/tx/0x45bb4f9247f27c30284794a8212f2e67049cb3058c7cc3c29758beda25fb0802)

The final authoritative readback is version 5, sealed, `CLEARABLE`, with all five decision predicates true. `can_clear_alert()` and `safe_to_clear_alert()` both return `true`; `get_review_count()` returns `5`. Replaying the same final review ID returns index `4` without appending a sixth review.

## Problem and why GenLayer

A public-health operator should not clear an advisory merely because one page or one model says it is over. The contract binds an advisory identity, service area, authority origins, issue and rescission notices, and optional testing evidence. GenLayer consensus evaluates the retrieved evidence independently and stores the agreed predicates, digests, state, and explanation on-chain.

This is not a replacement for an emergency response system, a laboratory information system, or a source-of-truth publication workflow. A conventional backend is preferable when the inputs are already trusted, deterministic, and controlled by one operator.

## How it works

1. The owner registers a bounded advisory record with distinct identity and area hashes, allowed HTTPS origins, and two or three source URLs.
2. The owner seals the evidence set, making the advisory active.
3. Any caller can submit a review ID after sealing. The leader retrieves the configured pages, hashes their bodies, and asks for five explicit predicates.
4. Validators independently retrieve and evaluate the same evidence. The contract accepts only a matching, schema-valid result.
5. The state becomes `CLEARABLE`, `STILL_ACTIVE`, or `UNRESOLVED`; all decision fields, source count, digests, and a bounded explanation are stored.

Retrieved pages are treated as hostile data. Their contents are escaped and delimited before entering the prompt, and page text cannot issue contract instructions.

## State model and invariants

`EMPTY -> REGISTERED -> ACTIVE -> {CLEARABLE, STILL_ACTIVE, UNRESOLVED} -> SUPERSEDED`.

- Only the owner can register, seal, or supersede.
- Registration must use two distinct 64-character hexadecimal hashes and HTTPS origins that authorize every configured source URL.
- Assessment is public only after sealing.
- A `CLEARABLE` result requires all five predicates to be true.
- A review ID is idempotent within its version and cannot be reused across advisory versions.
- Failed external retrieval or malformed/differential consensus output does not append a review or mutate the active advisory.

## Public API

### Writes

- `register_advisory(...)`: owner-only registration of the advisory identity, area, authorities, and sources.
- `seal_evidence()`: owner-only transition from `REGISTERED` to `ACTIVE`.
- `assess_rescission(review_id) -> uint32`: public consensus assessment after sealing; returns the stored review index.
- `supersede()`: owner-only closeout before a new version is registered.

### Views for integrators

- `get_advisory() -> Advisory`: current version, lifecycle state, sources, hashes, owner-controlled metadata, and sealed flag.
- `get_review(index) -> ReviewRecord`: stored validator-bound predicates, status, explanation, source count, and evidence digests.
- `get_review_count() -> uint32`: review history length.
- `can_clear_alert() -> bool` and `safe_to_clear_alert() -> bool`: downstream read-only gates; both are true only for `CLEARABLE`.

## Consensus design and failure behavior

The leader and validator both run the same evidence retrieval and decision validation path. The validator compares every stored decision predicate, evidence state, and evidence digest. JSON shape, booleans, allowed states, explanation length, hash format, and cross-field consistency are validated before storage.

The contract fails closed for malformed model output, invalid digests, contradictory predicates, unreachable sources, unauthorized origins, invalid lifecycle transitions, and non-owner administration. The caller must treat `ACCEPTED` as transport/consensus acceptance and use the finalized semantic readback as the source of the downstream decision.

## Consensus Binding Matrix

| Input/evidence | Leader output | Validator binding | Stored consequence |
| --- | --- | --- | --- |
| Advisory reference and configured sources | `advisory_identity_match` | Exact boolean and digest agreement | Identity predicate in `ReviewRecord` |
| Configured service area and notice language | `area_match` | Exact boolean agreement | Area predicate; false prevents clearance |
| Explicit lift/rescission language | `rescission_declared` | Exact boolean agreement | Rescission predicate |
| Explicit testing/sampling basis | `sampling_basis_present` | Exact boolean agreement | Sampling predicate |
| Allowed authority origin | `authority_match` | Exact boolean agreement | Authority predicate |
| All five predicates | `evidence_state` | Allowed state plus consistency rule | `CLEARABLE`, `STILL_ACTIVE`, or `UNRESOLVED` |
| Retrieved bodies | SHA-256 digests | Three digest fields compared | Auditable evidence identity |

## Security and edge cases

- Source URLs must be HTTPS, authority-matched, fragment-free, non-duplicated, and bounded in length.
- Source bodies are bounded before prompt insertion and escaped as JSON strings.
- Explanations are truncated to 512 characters before storage; non-string explanations fail closed.
- A source HTTP failure preserves the prior state and review count.
- Review replay is idempotent, while the same ID on a later version is rejected.
- Hashes identify the configured advisory and area; they are not presented as proof by themselves.

## Tests and verification

Install the pinned requirements, then run:

```text
python -m pytest tests -q
genvm-lint check contracts/boil_water_rescission_evidence_covenant.py
pip check
```

The verified result for this revision is `13 passed`, lint validation passed for 9 public methods, and `pip check` passed. The complete Studio Dev matrix and exact source/deployment hashes are in [`verification/test-summary.md`](verification/test-summary.md) and [`docs/deployment-evidence.md`](docs/deployment-evidence.md).

## Consensus engineering lessons

- Store the predicates that drive the consequential state, not only a free-form model explanation.
- Treat external pages as hostile evidence and isolate their text from contract instructions.
- Bind every source body to a digest so a later reviewer can see exactly which evidence was evaluated.
- Distinguish finalized execution from a transport-level accepted response before reading downstream state.
- Make replay and version boundaries explicit so an old review cannot silently affect a new advisory.

## Reusable integrations

- A public-health dashboard can read `safe_to_clear_alert()` before removing a notice.
- An incident workflow can use `get_review()` to show which predicates and sources caused an unresolved result.
- A governance process can register a new advisory version, seal it, and preserve the prior review history without trusting one operator's private database.

## Limitations

The contract depends on the availability and content of configured public pages and on validator agreement about their meaning. It does not prove that a source is truthful beyond the authority and evidence checks encoded here, and it does not automatically discover new URLs. Registration and sealing remain administrative actions controlled by the owner.

## Repository structure

```text
contracts/boil_water_rescission_evidence_covenant.py
tests/test_boil_water_rescission_evidence_covenant.py
samples/e2e-inputs.json
docs/deployment-evidence.md
verification/test-summary.md
requirements.txt
LICENSE
README.md
```

## License

MIT. See [`LICENSE`](LICENSE).
