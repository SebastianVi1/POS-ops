# Validate offline rules with a cross-language contract

Offline checkout must execute in TypeScript while the Java backend must independently validate synchronized business records. Define one documented behavioral contract with shared input/output examples exercised by both implementations, rather than attempting to share executable domain code across these runtimes. This accepts some implementation duplication to preserve offline execution and independent server validation, with contract tests controlling drift.
