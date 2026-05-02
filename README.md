# Impact Score Profile v0.1

Impact Score Profile v0.1 is a draft specification for evaluating how strongly a communication-derived trace event contributed to later meaning generation.

It provides a minimal, decomposable scoring profile for trace-aware systems that need a shared way to assess contribution without collapsing everything into a single opaque score.

This repository currently focuses on the Impact Score Profile layer only.

---

## Why this repository exists

Trace-aware systems often record events, references, and transformations, but they still face a core problem:

> How much did a given event actually matter?

A downstream output may reuse, transform, summarize, or structurally depend on earlier events, but without a common assessment profile, contribution remains inconsistent and difficult to compare.

Impact Score Profile v0.1 exists to provide a lightweight, interoperable way to express that contribution.

It is intended to support systems such as:

- attribution pipelines
- audit systems
- trust systems
- gratitude systems
- influence mapping
- royalty candidate review
- adjacent communication trace systems

---

## Scope

Impact Score Profile v0.1 defines a minimal profile for contribution assessment.

It covers:

- normalized total impact scoring
- decomposed scoring dimensions
- disclosed weight profiles
- assessment method disclosure
- assessment scope disclosure
- evaluator metadata
- optional evidence references
- machine-validatable JSON Schema support

It does **not** cover:

- legal ownership determination
- automatic monetary settlement
- full provenance graph serialization
- cryptographic signature infrastructure
- identity verification infrastructure
- mandatory scoring enforcement across all systems

---

## Core idea

Impact Score Profile answers a simple but important question:

> How strongly did this trace event contribute to later meaning generation?

Instead of forcing systems to rely on a single unexplained score, this profile separates contribution into visible dimensions.

That makes impact assessment:

- more transparent
- more auditable
- more comparable
- more reusable across systems

---

## Design principles

- Keep the profile minimal.
- Prefer decomposable scoring over opaque scoring.
- Separate contribution assessment from ownership determination.
- Separate contribution assessment from monetary settlement.
- Support human, AI, hybrid, and rule-based evaluation methods.
- Make score interpretation more auditable by exposing dimensions and weights.
- Allow multiple scopes of assessment.
- Treat scores as evaluative signals, not final truth.

---

## Repository structure

```text
.
├── .github/
│   └── workflows/
│       └── validate-specs.yml
├── examples/
│   └── impact-score-profile.sample.json
├── schemas/
│   └── impact-score-profile-v0.1.schema.json
├── LICENSE
├── README.md
└── impact-score-profile-v0.1.yaml
Start here

Read the files in this order:

impact-score-profile-v0.1.yaml
Human-readable source specification for Impact Score Profile v0.1.
schemas/impact-score-profile-v0.1.schema.json
Machine-validatable JSON Schema for the profile.
examples/impact-score-profile.sample.json
Example impact assessment record that should validate successfully against the schema.
.github/workflows/validate-specs.yml
CI workflow that checks schema/sample consistency on every push and pull request.
What this profile evaluates

Impact Score Profile v0.1 is designed for evaluating a trace event or equivalent contribution unit.

A valid assessment typically asks things like:

Was this event actually referenced?
Did it materially transform the downstream result?
Did it introduce meaningful novelty?
Was the downstream result dependent on it?
Is it likely to be reused later?

The profile does not claim that impact automatically implies ownership, entitlement, or payment.

It only standardizes how contribution may be expressed.

Main fields
Required core fields
assessment_id
trace_event_id
impact_score
dimensions
assessment_method
assessment_scope
confidence
assessed_at
Optional context fields
event_record_id
communication_id
conversation_root_id
artifact_id
structure_refs
weight_profile
score_formula
evaluator
rationale
evidence_refs
Core dimensions

Impact Score Profile v0.1 uses these required core dimensions:

reference_strength
transformation_weight
novelty_delta
dependency_weight
downstream_reuse_potential

Optional dimensions may also be included, such as:

semantic_centrality
influence_scope
originality_support

All dimension values are normalized between 0.0 and 1.0.

Interpretation guidance

A total impact_score is a normalized signal, not an absolute truth.

A rough interpretation may look like this:

0.00 - 0.19 → negligible
0.20 - 0.39 → low
0.40 - 0.59 → moderate
0.60 - 0.79 → high
0.80 - 1.00 → critical

These bands are interpretive guidance only.

A high impact score does not imply:

ownership
legal entitlement
mandatory payment
exclusive authorship
Assessment method and scope

The profile makes two distinctions explicit:

Assessment method

Allowed values:

human
ai
hybrid
rule_based
Assessment scope

Allowed values:

local
session
thread
conversation
cross_system

This matters because the same event may have different significance depending on the evaluator and the scope.

A score of 0.78 at conversation scope does not necessarily mean the same thing as 0.78 at cross_system scope.

Example use cases

Impact Score Profile v0.1 can support cases such as:

scoring how strongly one trace event shaped a downstream summary
ranking candidate events for gratitude review
identifying high-impact events for trust evaluation
prioritizing audit attention toward materially influential events
marking potential royalty-candidate events without forcing automatic settlement
comparing local and conversation-level contribution patterns
Schema usage
Requirements
Python 3.10+
jsonschema

Install the validator locally:

python -m pip install --upgrade pip
pip install jsonschema
Validate the schema and sample locally

Run the following command from the repository root:

python - <<'PY'
import json
import os
import sys
from jsonschema import Draft202012Validator

label = "Impact Score Profile v0.1"
schema_path = "schemas/impact-score-profile-v0.1.schema.json"
sample_path = "examples/impact-score-profile.sample.json"

failed = False

print(f"\n=== Validating {label} ===")
print(f"Schema: {schema_path}")
print(f"Sample: {sample_path}")

if not os.path.exists(schema_path):
    print(f"ERROR: Schema file not found: {schema_path}")
    failed = True

if not os.path.exists(sample_path):
    print(f"ERROR: Sample file not found: {sample_path}")
    failed = True

if failed:
    print("\nValidation failed.")
    sys.exit(1)

try:
    with open(schema_path, "r", encoding="utf-8") as f:
        schema = json.load(f)
except json.JSONDecodeError as e:
    print(f"ERROR: Invalid JSON in {schema_path}: {e}")
    sys.exit(1)

try:
    with open(sample_path, "r", encoding="utf-8") as f:
        sample = json.load(f)
except json.JSONDecodeError as e:
    print(f"ERROR: Invalid JSON in {sample_path}: {e}")
    sys.exit(1)

try:
    Draft202012Validator.check_schema(schema)
except Exception as e:
    print(f"ERROR: Invalid schema in {schema_path}: {e}")
    sys.exit(1)

validator = Draft202012Validator(schema)
errors = sorted(validator.iter_errors(sample), key=lambda e: list(e.path))

if errors:
    print("ERROR: Validation failed")
    for err in errors:
        path = ".".join(str(x) for x in err.path) if err.path else "<root>"
        print(f" - {path}: {err.message}")
    sys.exit(1)

print("OK: Impact Score Profile v0.1 sample is valid.")
PY
CI validation

This repository validates the schema/sample pair in GitHub Actions through:

.github/workflows/validate-specs.yml

The workflow is intended to run on push and pull request events so that missing files, invalid JSON, schema drift, and sample mismatches are caught early.

Validation expectations

A valid Impact Score Profile record should satisfy at least the following:

assessment_id must match the isp_ identifier pattern
trace_event_id must match the ctid_ identifier pattern
impact_score must be between 0.0 and 1.0
confidence must be between 0.0 and 1.0
all required core dimensions must be present
each dimension score must be between 0.0 and 1.0
assessment_method must use an allowed enum value
assessment_scope must use an allowed enum value
structure_refs[*].structure_id, when present, must match the sid_ pattern
structure_refs[*].relation, when present, must use allowed relation values
assessed_at must be a valid date-time string

Some higher-order checks remain implementation-level concerns rather than pure schema constraints, for example:

whether the rationale truly supports the score
whether the chosen weight profile is appropriate
whether the declared impact matches actual downstream dependency
whether optional dimensions should materially change interpretation
Current example profile

The current sample demonstrates:

a unique impact assessment record ID
a CTID-style trace event reference
a normalized total impact score
required core dimensions
an optional weight profile
evaluator metadata
rationale text
evidence references
semantic structure references
an assessment timestamp

This sample is intentionally minimal but sufficient for schema validation and downstream experimentation.

Relationship to adjacent systems

Impact Score Profile v0.1 is designed to work alongside systems such as:

communication trace record formats
identifier interoperability layers
attribution systems
audit systems
trust-event systems
gratitude-event systems
influence graph builders
royalty candidate review systems

It is not itself:

a trace recording format
a legal ownership protocol
a settlement engine
a provenance graph format

It is a lightweight assessment layer.

Privacy note

Impact Score Profile is an assessment profile, not a mandate for full content retention.

Implementations are encouraged to prefer:

references over unnecessary raw payload retention
scoped identifiers over excessive identity exposure
privacy-aware storage and retention policies
clear separation between scores, identifiers, and personal content

Impact interoperability should not be treated as a justification for unlimited surveillance or retention.

Current status
Status: Draft
Version: v0.1

This version is intended as a minimal working profile for:

drafting
schema validation
contribution assessment testing
downstream protocol experimentation

It should be treated as a stable draft for structural use, not as a final institutional or legal standard.

Roadmap

Possible next steps include:

stricter schema refinements
pass/fail compliance test vectors
optional signature and verification fields
richer assessment comparison guidance
explicit mapping to trust / gratitude / royalty layers
relationship profiles for trace-record systems
governance guidance for score issuance and reuse
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

separate contribution assessment from ownership
separate contribution assessment from payment
prefer decomposable scoring over opaque scoring

License

This repository is distributed under the terms of the repository-level LICENSE file.

One-line summary

Impact Score Profile v0.1 is a minimal, decomposable scoring profile for evaluating how strongly a communication-derived trace event contributed to later meaning generation.
