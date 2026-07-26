# Security

Do not include GitHub tokens, registry credentials, runtime configuration,
customer data, or exploit details in a public issue.

Report suspected vulnerabilities privately to the repository maintainers or
through a private GitHub security advisory. Rotate any credential that may
have been exposed before sharing diagnostic evidence.

`uns-bootstrap` first attempts an anonymous runtime release download. For a
private runtime it accepts a fine-grained GitHub token through a hidden prompt
and uses it only as an `Authorization` header for the current process. It does
not write the token to disk or place it in a repository URL.

Use an expiring token restricted to the required private runtime repository
with read-only Contents permission. Verify that downloaded installers and
binaries match the SHA-256 files published with the immutable release.
