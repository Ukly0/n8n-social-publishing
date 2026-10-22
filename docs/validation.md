# Validation record

Facts recorded during the final review of this package. Dates are October 2026.

## Scope

- Three workflows: interactive social publishing, scheduled video publishing and
  the page-change watcher.
- The files in `workflows/` are the exact files used for the import test below.
  They carry generic demonstration workflow IDs because the sanitized exports
  intentionally contain no instance identifiers.
- No source repository history, media files, credentials, helper scripts or
  internal manifests are included.

## Sanitization checks

- Credential blocks, pinned data, static data and instance metadata were removed.
- Chat, account, spreadsheet and workflow identifiers were replaced by `$env`
  expressions or placeholders.
- Private hosts were replaced with `example.invalid`; public API endpoints were
  kept.
- JavaScript was inspected before inclusion: no URLs, filesystem paths, email
  addresses or token-shaped strings were found.
- Prompt examples were replaced with synthetic text.
- A literal token from the source was compared in memory with the package. It is
  not present in any file. The value was never printed.
- The source token remains outside this package. Rotate it if the original
  instance still uses it.

## Structural checks

- All three JSON files parse.
- Node names are unique; every connection and `$('Node')` reference resolves.
- `active` is `false` in all three workflows.
- No `credentials`, `pinData`, `staticData`, `meta`, `versionId` or `webhookId`
  fields remain.
- Targeted file scans found no identified private paths, real email addresses,
  private hosts, numeric chat identifiers or token-shaped values. Public API
  endpoints remain intentionally. These checks are not a guarantee that every
  possible secret format has been detected.
- README links and image paths resolve inside the package.
- The three JavaScript bodies passed syntax compilation with Node.js without
  invoking the generated functions or executing workflow logic.

## Import test

- Instance: n8n 2.25.7, local image, new container, new volume, internal Docker
  network, loopback access only.
- Import command: `n8n import:workflow --separate --input=/import --activeState=false`.
- Result: three workflows imported, all reported with `active: false` by a CLI
  export.
- Executions: none. No workflow was activated, run or tested. No credential was
  created and no API was called.
- The demo instance, network and volume were removed after capture.

## Screenshot checks

- Seven PNG files were captured from the same demo instance with the workflows
  inactive.
- PNG chunks were inspected: no `tEXt`, `iTXt` or `zTXt` metadata and no
  hostnames in the file contents.
- Visible text of the node panels was scanned for account data, local paths and
  identifiers; no matches were found.
- The images are editor captures, not execution evidence.
- The coordinator visually reviewed all seven images before publication.

## Not validated

- End-to-end execution, credentials, external services and side effects.
- The excluded helper scripts, which are not distributed.
- Import into a different n8n version or into a production instance.
- Performance, reliability, retry behaviour or rate limits.
- Licensing and third-party rights beyond the statements in the README.

## Reproducing the checks

The sanitized files can be parsed and scanned with standard tooling. The demo
setup is summarized in `docs/screenshots.md`; it is not a production
deployment recipe. Do not enable the workflows to "see if they work" on an
instance that holds real accounts.
