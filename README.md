# Social publishing automations with n8n

Three self-hosted n8n workflows for publishing and monitoring social content. Each workflow is a standalone JSON export and can be imported independently.

- **Social publishing:** receives a Telegram message or callback, drafts the copy, routes text or video, publishes to X and Threads, and records the queue state.
- **Scheduled video publishing:** reads pending rows from a Google Sheet, uploads and shares the video, creates the post, and confirms the result.
- **Page-change watcher:** fetches a list of pages, compares extracted text, drafts a short summary, and sends it to Telegram.

![Social publishing workflow](screenshots/01_overview_social_publishing.png)

## Workflows

| File | Trigger | Function |
| --- | --- | --- |
| `workflows/social-media-publishing.json` | Telegram message or callback | Classify the input, draft copy, route text or video, publish, record queue state |
| `workflows/scheduled-video-publishing.json` | Schedule | Read pending rows, filter, upload and share video, create a post, confirm |
| `workflows/daily-reminder-agent.json` | Schedule | Fetch pages, diff extracted text, draft a message, deliver to Telegram |

Additional graph views: [scheduled publishing](screenshots/02_overview_scheduled_publishing.png) and [change watcher](screenshots/03_overview_change_watcher.png). Generated topology diagrams are in `docs/diagrams/`.

## Repository layout

- `workflows/` — the three n8n workflow exports.
- `screenshots/` — editor captures of the workflow canvas and key nodes.
- `docs/import-notes.md` — import steps, placeholders and required settings.
- `docs/screenshots.md` — what each capture shows and what it does not prove.
- `docs/validation.md` — structural checks, import test and current limits.

## How it works

### Social publishing

1. A Telegram trigger receives text, video or a callback.
2. The workflow classifies the input and prepares the reference text.
3. A drafting node rewrites the copy under the stored rules.
4. A switch routes text posts and video posts to separate branches.
5. Text is published to X and Threads. Video goes through a chunked upload, then a post is created.
6. The queue state is written back to Google Sheets and the result is reported in Telegram.

### Scheduled video publishing

1. A schedule trigger reads pending rows from the queue sheet.
2. Rows are filtered by state and date.
3. The video is downloaded and prepared for upload.
4. The X media upload is started, appended and finalised.
5. A post is created and the queue state is updated.
6. The result is confirmed in Telegram.

### Page-change watcher

1. A schedule trigger loads the configured page list.
2. Each page is rendered or downloaded and its visible text is extracted.
3. The new text is compared with the previous snapshot.
4. A drafting node writes a short change summary.
5. The summary is delivered to Telegram.

## Setup

1. Import the three JSON files into a self-hosted n8n instance. The workflow IDs are generic placeholders; assign new IDs if you merge the files into another instance. There are no sub-workflow references to remap.
2. Create credentials for Telegram, X media upload, Google Sheets, Google Drive and the chat-model provider.
3. Recreate the queue spreadsheet with the columns `Tweet`, `Link`, `Fecha` and `Estado`, then update the Google Sheets nodes.
4. Replace the placeholders listed in `docs/import-notes.md`. Public endpoints are kept; private values are not.
5. Provide the two helper scripts described below, or disable the branches that call them.
6. Keep every workflow inactive until the configuration and side effects have been reviewed.

## Configuration

| Variable | Used by | Purpose |
| --- | --- | --- |
| `TELEGRAM_CHAT_ID` | all flows | Operator chat for status and confirmations |
| `QUEUE_SHEET_ID` | publishing flows | Spreadsheet that holds the queue |
| `THREADS_ACCESS_TOKEN` | both publishers | Threads API access |
| `THREADS_CLIENT_SECRET` | social publishing | App secret used by the token refresh call |
| `THREADS_USER_ID` | both publishers | Threads account identifier |
| `SOCIAL_ACCOUNT_ID` | social publishing | Internal account label |
| `WORK_DIR` | all flows | Directory for downloaded media |
| `SCRIPTS_DIR` | command nodes | Directory holding the helper scripts |
| `REPLACE_WITH_SYNDICATION_TOKEN` | Twitter embed request | Public token for that endpoint |

The exports use `$env` expressions. If environment access is blocked on a hardened instance, keep the values in a private runtime configuration instead of disabling the restriction globally.

## Credentials

- Telegram bot
- X/Twitter OAuth with media upload
- Google Sheets OAuth
- Google Drive OAuth
- DeepSeek or a compatible chat-model provider

## Helper scripts

Two command nodes call scripts that are not distributed in this repository:

| Node | Flow | Expected behaviour |
| --- | --- | --- |
| `Execute Command1` | Social publishing | Accept a post URL and write the video file to `WORK_DIR` |
| `Execute Command` | Page-change watcher | Render a page and return JSON with `stable` text and `url` |

Point `SCRIPTS_DIR` at your own implementations, or disable those branches. The original scripts are not included.

## Editor captures

| Capture | Node | What it shows |
| --- | --- | --- |
| [Telegram trigger](screenshots/04_node_telegram_trigger.png) | Telegram Trigger | Text, video and callback updates |
| [Agent prompt](screenshots/05_node_agent_prompt.png) | Drafting node | Rewrite rules and prompt structure |
| [Queue code](screenshots/06_node_schedule_code.png) | Code | First free publishing slot in the queue |
| [Media upload](screenshots/07_node_media_upload.png) | Upload Twitter Video | Start, append and finalise a chunked upload |

The captures come from an isolated editor instance with the workflows inactive. They document configuration, not a production run. See `docs/screenshots.md` for the capture setup.

## Security and scope

- The exports are sanitized: credentials, account IDs, spreadsheet IDs, private page lists and token-shaped values are removed or replaced.
- All workflows are inactive by default. Publishing nodes have real side effects; review them before activation.
- No end-to-end execution, live API call or production deployment is included.
- The repository contains workflow definitions and documentation only. It does not include media files, credentials or the original instance.
- No licence for reuse is granted. Third-party services and n8n keep their own terms.

## Documentation

- [Import and configuration](docs/import-notes.md)
- [Editor screenshots](docs/screenshots.md)
- [Validation record](docs/validation.md)
