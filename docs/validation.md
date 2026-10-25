# Validation and limits

## What is checked

- The three workflow JSON files parse.
- Node names are unique and every connection and `$('Node')` reference resolves.
- `active` is `false` in all three workflows.
- No `credentials`, `pinData`, `staticData`, `meta`, `versionId` or `webhookId` fields remain.
- Scans find no private paths, real email addresses, private hosts, numeric chat identifiers or token-shaped values. Public API endpoints remain.
- README links and image paths resolve inside the repository.
- The three JavaScript bodies compile with Node.js without executing workflow logic.

## Import test

The package was imported into a disposable n8n instance using:

```sh
n8n import:workflow --separate --input=/import --activeState=false
```

All three workflows imported with `active: false`. No workflow was activated, run or connected to an external service. No credentials were created.

## Screenshots

The seven PNG captures come from the same isolated editor instance. PNG metadata was checked and the visible node panels were scanned for account data, local paths and identifiers. The images document the editor state; they are not execution evidence.

## Not covered

- End-to-end execution, credentials, external services and publishing side effects.
- The two helper scripts, which are not included.
- Import into a different n8n version or a production instance.
- Performance, reliability, retry behaviour or API rate limits.
- Licensing and third-party rights beyond the statements in the README.

Do not enable the workflows on an instance that holds real accounts just to test them. Use a disposable instance and synthetic content first.
