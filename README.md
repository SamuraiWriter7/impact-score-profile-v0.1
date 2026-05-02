# Trace Interoperability Profiles v0.1

This repository contains two draft interoperability profiles for trace-aware communication systems:

- **CTR-ID Unified Protocol v0.1**  
  A minimal identifier profile for connecting communication trace records across events, conversations, structures, and derived artifacts.

- **Impact Score Profile v0.1**  
  A minimal scoring profile for evaluating how strongly a communication-derived trace event contributed to later meaning generation.

Together, these profiles provide a lightweight foundation for trace-aware systems that need both:

1. **stable identifiers**, and  
2. **decomposable contribution assessment**

without forcing a full provenance graph or a full settlement system into the same layer.

---

## Why this repository exists

Trace-oriented systems often fail for two reasons:

- they do not identify things consistently
- they do not evaluate contribution consistently

In practice, different systems may confuse:

- the serialized record
- the traceable event
- the communication object
- the larger conversation
- the semantic structure
- the derived artifact

And even when those are distinguishable, systems still need a shared way to ask:

> How much did this event actually contribute to later meaning generation?

This repository exists to address those two problems separately but compatibly:

- **CTR-ID** handles identifier interoperability
- **Impact Score Profile** handles contribution assessment interoperability

---

## Scope

This repository currently focuses on two lightweight interoperability layers.

### Included

- identifier interoperability for trace-aware systems
- decomposable impact assessment for trace events
- schema validation
- sample records
- CI validation for schema/sample consistency

### Not included

- legal ownership determination
- monetary settlement logic
- full provenance graph serialization
- cryptographic signature infrastructure
- identity verification infrastructure
- mandatory global registry behavior

---

## Core idea

A trace-aware system usually needs at least two layers:

### Layer 1: Identity
What exactly is being identified?

### Layer 2: Evaluation
How strongly did that identified event contribute to later meaning generation?

CTR-ID answers the first question.  
Impact Score Profile answers the second.

That makes this repository useful as a minimal foundation for systems such as:

- attribution pipelines
- audit systems
- trust systems
- gratitude systems
- influence mapping
- royalty candidate review
- communication trace standards

---

## Design principles

- Keep each profile minimal.
- Separate identity from evaluation.
- Do not overload one ID with multiple semantic roles.
- Do not collapse contribution assessment into a single opaque score.
- Keep both formats machine-readable and human-auditable.
- Prefer interoperability over premature complexity.
- Do not equate trace with ownership.
- Do not equate impact with payment.

---

## Repository structure

```text
.
├── .github/
│   └── workflows/
│       └── validate-specs.yml
├── examples/
│   ├── ctr-id-unified-protocol.sample.json
│   └── impact-score-profile.sample.json
├── schemas/
│   ├── ctr-id-unified-protocol-v0.1.schema.json
│   └── impact-score-profile-v0.1.schema.json
├── LICENSE
├── README.md
├── ctr-id-unified-protocol-v0.1.yaml
└── impact-score-profile-v0.1.yaml
Start here

Read the files in this order:

ctr-id-unified-protocol-v0.1.yaml
Human-readable source specification for CTR-ID v0.1.
schemas/ctr-id-unified-protocol-v0.1.schema.json
Machine-validatable JSON Schema for CTR-ID.
examples/ctr-id-unified-protocol.sample.json
Example CTR-ID record.
impact-score-profile-v0.1.yaml
Human-readable source specification for Impact Score Profile v0.1.
schemas/impact-score-profile-v0.1.schema.json
Machine-validatable JSON Schema for Impact Score Profile.
examples/impact-score-profile.sample.json
Example impact assessment record.
.github/workflows/validate-specs.yml
CI workflow that validates schema/sample consistency.
Profile 1: CTR-ID Unified Protocol v0.1

CTR-ID Unified Protocol v0.1 defines a minimal identifier profile for trace-aware communication systems.

It distinguishes between:

the record instance
the trace event
the communication object
the conversation lineage
the semantic structure
the derived artifact
Main identifier classes
event_record_id
trace_event_id
parent_trace_event_id
derived_from_trace_ids
communication_id
session_id
thread_id
conversation_root_id
artifact_id
structure_refs[].structure_id
Recommended prefixes
ctr_ for record instances
ctid_ for trace events
comm_ for communication objects
conv_ for conversation roots
art_ for artifacts
sid_ for semantic structures

CTR-ID is not a full provenance graph protocol.
It is a small interoperability layer for clean identifier semantics.

Profile 2: Impact Score Profile v0.1

Impact Score Profile v0.1 defines a minimal, decomposable scoring profile for evaluating how strongly a communication-derived trace event contributed to later meaning generation.

It is designed to avoid opaque scoring by exposing scoring dimensions directly.

Core fields
assessment_id
trace_event_id
impact_score
dimensions
assessment_method
assessment_scope
confidence
assessed_at
Core dimensions
reference_strength
transformation_weight
novelty_delta
dependency_weight
downstream_reuse_potential
Optional dimensions
semantic_centrality
influence_scope
originality_support

Impact Score Profile is not a legal ownership system and not a payment protocol.
It is an evaluation layer for contribution assessment.

How the two profiles work together

The intended relationship is simple:

CTR-ID identifies the traceable unit
Impact Score Profile evaluates that identified unit

In other words:

CTR-ID answers what
Impact Score answers how much

This separation keeps the system clean.

Without identifier clarity, scoring becomes unstable.
Without scoring clarity, identifiers remain descriptive but economically and socially inert.

Together, they form a lightweight trace interoperability stack.

Example use cases

These profiles can support workflows such as:

linking multiple trace events across a conversation
distinguishing an event from its serialized record
associating a trace event with a derived artifact
tracking semantic continuity across summary, translation, or reframing
scoring how strongly one event contributed to a downstream artifact
ranking high-impact events for gratitude, trust, audit, or royalty-candidate review
Schema usage
Requirements
Python 3.10+
jsonschema

Install the validator locally:

python -m pip install --upgrade pip
pip install jsonschema
Validate all schemas and samples locally

Run the following command from the repository root:

python - <<'PY'
import json
import os
import sys
from jsonschema import Draft202012Validator

validations = [
    (
        "CTR-ID Unified Protocol v0.1",
        "schemas/ctr-id-unified-protocol-v0.1.schema.json",
        "examples/ctr-id-unified-protocol.sample.json",
    ),
    (
        "Impact Score Profile v0.1",
        "schemas/impact-score-profile-v0.1.schema.json",
        "examples/impact-score-profile.sample.json",
    ),
]

failed = False

for label, schema_path, sample_path in validations:
    print(f"\n=== Validating {label} ===")
    print(f"Schema: {schema_path}")
    print(f"Sample: {sample_path}")

    if not os.path.exists(schema_path):
        print(f"ERROR: Schema file not found: {schema_path}")
        failed = True
        continue

    if not os.path.exists(sample_path):
        print(f"ERROR: Sample file not found: {sample_path}")
        failed = True
        continue

    try:
        with open(schema_path, "r", encoding="utf-8") as f:
            schema = json.load(f)
    except json.JSONDecodeError as e:
        print(f"ERROR: Invalid JSON in {schema_path}: {e}")
        failed = True
        continue

    try:
        with open(sample_path, "r", encoding="utf-8") as f:
            sample = json.load(f)
    except json.JSONDecodeError as e:
        print(f"ERROR: Invalid JSON in {sample_path}: {e}")
        failed = True
        continue

    try:
        Draft202012Validator.check_schema(schema)
    except Exception as e:
        print(f"ERROR: Invalid schema in {schema_path}: {e}")
        failed = True
        continue

    try:
        validator = Draft202012Validator(schema)
        errors = sorted(validator.iter_errors(sample), key=lambda e: list(e.path))

        if errors:
            print(f"ERROR: Validation failed for {label}")
            for err in errors:
                path = ".".join(str(x) for x in err.path) if err.path else "<root>"
                print(f" - {path}: {err.message}")
            failed = True
        else:
            print(f"OK: {label} sample is valid.")
    except Exception as e:
        print(f"ERROR: Exception while validating {label}: {e}")
        failed = True

if failed:
    print("\nValidation failed.")
    sys.exit(1)

print("\nAll validations passed.")
PY
Validate only CTR-ID
python - <<'PY'
import json
from jsonschema import Draft202012Validator

schema_path = "schemas/ctr-id-unified-protocol-v0.1.schema.json"
sample_path = "examples/ctr-id-unified-protocol.sample.json"

with open(schema_path, "r", encoding="utf-8") as f:
    schema = json.load(f)

with open(sample_path, "r", encoding="utf-8") as f:
    sample = json.load(f)

Draft202012Validator.check_schema(schema)
validator = Draft202012Validator(schema)
errors = sorted(validator.iter_errors(sample), key=lambda e: list(e.path))

if errors:
    print("Validation failed:")
    for err in errors:
        path = ".".join(str(x) for x in err.path) if err.path else "<root>"
        print(f" - {path}: {err.message}")
else:
    print("OK: CTR-ID sample is valid.")
PY
Validate only Impact Score Profile
python - <<'PY'
import json
from jsonschema import Draft202012Validator

schema_path = "schemas/impact-score-profile-v0.1.schema.json"
sample_path = "examples/impact-score-profile.sample.json"

with open(schema_path, "r", encoding="utf-8") as f:
    schema = json.load(f)

with open(sample_path, "r", encoding="utf-8") as f:
    sample = json.load(f)

Draft202012Validator.check_schema(schema)
validator = Draft202012Validator(schema)
errors = sorted(validator.iter_errors(sample), key=lambda e: list(e.path))

if errors:
    print("Validation failed:")
    for err in errors:
        path = ".".join(str(x) for x in err.path) if err.path else "<root>"
        print(f" - {path}: {err.message}")
else:
    print("OK: Impact Score Profile sample is valid.")
PY
CI validation

This repository validates schema/sample pairs in GitHub Actions through:

.github/workflows/validate-specs.yml

The workflow is intended to run on push and pull request events so that broken schemas, broken samples, or missing files are detected early.

Validation expectations
For CTR-ID records

A valid CTR-ID record should satisfy at least the following:

event_record_id must match the ctr_ pattern
trace_event_id must match the ctid_ pattern
parent_trace_event_id, when present, must match the ctid_ pattern
derived_from_trace_ids, when present, must contain only ctid_ identifiers
communication_id, when present, must match the comm_ pattern
conversation_root_id, when present, must match the conv_ pattern
artifact_id, when present, must match the art_ pattern
structure_refs[*].structure_id must match the sid_ pattern
structure_refs[*].relation must use only allowed relation values
For Impact Score Profile records

A valid impact assessment should satisfy at least the following:

assessment_id must match the isp_ pattern
trace_event_id must match the ctid_ pattern
impact_score must be between 0.0 and 1.0
confidence must be between 0.0 and 1.0
all required core dimensions must be present
each dimension score must be between 0.0 and 1.0
assessment_method must use an allowed enum value
assessment_scope must use an allowed enum value
structure_refs[*].relation, when present, must use allowed relation values

Some higher-order checks remain implementation-level concerns, such as:

whether the declared lineage is semantically justified
whether the impact score matches the rationale
whether the chosen weight profile is appropriate
whether optional dimensions should alter the total score interpretation
Privacy note

These profiles are designed to be compatible with privacy-aware systems.

Implementations are encouraged to prefer:

scoped identifiers over unnecessary identity exposure
reference-based linkage over raw content retention
privacy-aware retention policies
separation between identifiers, assessments, and personal content

Trace interoperability should not be treated as a justification for unlimited content retention.

Current status
Status: Draft
Version: v0.1

This repository is intended as a minimal working foundation for:

drafting
schema validation
identifier interoperability testing
contribution assessment testing
downstream protocol experimentation

It should be treated as a stable draft for structural use, not as a final institutional or legal standard.

Relationship to adjacent systems

These profiles are designed to work alongside systems such as:

communication trace record formats
attribution pipelines
trust-event systems
gratitude-event systems
audit systems
influence graph builders
royalty candidate review systems

This repository does not attempt to be all of those things at once.

It focuses on two narrow but essential layers:

identity interoperability
impact assessment interoperability
Roadmap

Possible next steps include:

stricter schema refinements
pass/fail compliance test vectors
optional signature and verification fields
richer lineage rules
score-profile comparison guidance
explicit mapping to trust / gratitude / royalty layers
relationship documents to adjacent trace standards
provenance bridge profiles
Contributing

Contributions that improve clarity, interoperability, and validation rigor are welcome.

Especially useful contributions include:

schema corrections
better examples
compliance test vectors
validator improvements
documentation refinements
downstream mapping proposals

When proposing changes, try to preserve the core design principles:

separate identity from evaluation
separate trace from ownership
separate impact from payment

License

This repository is distributed under the terms of the repository-level LICENSE file.

One-line summary

This repository provides two minimal interoperability profiles for trace-aware systems: CTR-ID for identifier clarity, and Impact Score Profile for decomposable contribution assessment.
