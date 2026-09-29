# Federated evidence reference

[`pointers.md`](pointers.md) is the generated map from this list to the
canonical Bravia hardware claims it consumes. Each pointer pins one semantic
claim hash and its expected authority state; the claim body remains upstream.

The verifier fails if an upstream claim changes, disappears, is corrected or
superseded, or moves from supported to untested (or the reverse) without an
explicit downstream update.
