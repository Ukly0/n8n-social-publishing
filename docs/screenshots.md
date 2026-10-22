# Editor screenshots

These are real captures of the n8n editor with the sanitized workflows imported
into an isolated demonstration instance. Nothing was executed. Every image is
labeled *n8n editor, sanitized demonstration copy; not a production execution*.

## Images

| File | View | Shows |
|---|---|---|
| `screenshots/01_overview_social_publishing.png` | Full graph | Telegram intake, two agents, routing, media upload and spreadsheet queue |
| `screenshots/02_overview_scheduled_publishing.png` | Full graph | Scheduled publisher fed by a spreadsheet queue |
| `screenshots/03_overview_change_watcher.png` | Full graph | Page-diff watcher with model summary and Telegram delivery |
| `screenshots/04_node_telegram_trigger.png` | Node panel | Trigger settings; no credential selected |
| `screenshots/05_node_agent_prompt.png` | Node panel | Agent prompt and model selection; examples are synthetic |
| `screenshots/06_node_schedule_code.png` | Node panel | Code that scans the queue for a free slot |
| `screenshots/07_node_media_upload.png` | Node panel | Chunked media upload request with placeholder values |

## Purpose of the captured nodes

| Node | Purpose | Basis |
|---|---|---|
| Telegram Trigger | Receive text, video and callback updates | Documented by type and trigger settings |
| AI Agent | Rewrite the reference text under the stored rules | Prompt is visible; the reason for two variants is inferred |
| Code (schedule) | Try five daily slots over seven days and return the first free one | Documented by the included code |
| Upload Twitter Video | Start, append and finalize a media upload | Documented by the three requests and query parameters |

Node names come from the sanitized exports. One node keeps its Spanish name
(`Confirmación`); the README describes it in English.

## Demonstration setup

- n8n 2.25.7 from the locally available image; no image was downloaded.
- New container and volume, internal Docker network without outbound access, no
  Docker socket, no privileged mode, no host networking.
- One read-only mount containing the three JSON files used for import.
- Fresh encryption key and JWT secret generated for the demo; none are included
  here.
- Telemetry, version notifications, templates, community packages and the
  public API were disabled; environment access from nodes was blocked.
- A fictional owner account was created for the demo. No personal account or
  OAuth provider was used.
- The UI was reached over loopback only. No LAN port or public tunnel was opened.
- The demo container, network and volume were removed after capture.

## What the images do not prove

- They do not show a successful execution, output, error handling or production
  behaviour.
- They do not prove that credentials, external APIs or the excluded helper
  scripts work.
- They are not a substitute for the topology diagrams or for reading the
  workflow JSON.
- Interface details belong to the captured n8n version.

## Metadata

The PNG files carry no text metadata chunks and no hostnames. Their dimensions
are 3116x1804 for the graph views and 3104x1904 for the node panels.
