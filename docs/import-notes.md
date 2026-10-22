# Setup and configuration

This copy keeps the workflow logic and removes the private values. It imports
inactive; nothing runs until credentials, identifiers and two helper scripts are
provided.

## Values that must be configured

| Placeholder | Where | Replace with |
|---|---|---|
| `TELEGRAM_CHAT_ID` | status and confirmation nodes | Operator chat ID |
| `QUEUE_SHEET_ID` | Google Sheets nodes | Spreadsheet ID |
| `THREADS_ACCESS_TOKEN` | token nodes and publish calls | Threads long-lived access token |
| `THREADS_CLIENT_SECRET` | token refresh call | App secret |
| `THREADS_USER_ID` | Threads endpoints | Account ID |
| `SOCIAL_ACCOUNT_ID` | Set node | Your own account label |
| `WORK_DIR` | file nodes | Directory for downloaded media |
| `SCRIPTS_DIR` | command nodes | Directory holding the helper scripts |
| `REPLACE_WITH_SYNDICATION_TOKEN` | Twitter embed request | Public token of that endpoint |

If environment access is disabled in n8n, put these values in a private copy
that is never committed, or use credentials where the node supports them.

## Credentials to create

- Telegram bot
- X/Twitter OAuth (media upload)
- Google Sheets OAuth
- Google Drive OAuth
- DeepSeek or compatible chat model

## Spreadsheet

Recreate a sheet with at least these columns: `Tweet`, `Link`, `Fecha`,
`Estado`. The flows read and write `Estado` with values such as `PROGRAMADO`.
Adjust the column mappings if your names differ.

## Helper scripts

Two command nodes call scripts that are not distributed here:

| Node | Flow | Needs |
|---|---|---|
| Execute Command1 | social publishing | A downloader that accepts a post URL and writes the video file |
| Execute Command | change watcher | A renderer that returns JSON with `stable` text and `url` |

Point `SCRIPTS_DIR` at your own implementations, or disable those branches. Do
not assume the original scripts are included or licensed.

## Import steps

1. Import the three JSON files into a disposable instance.
2. Create the credentials and fill the placeholders.
3. Recreate the spreadsheet and update the Google Sheets nodes.
4. Check the two switch nodes against your own routing rules.
5. Keep every workflow inactive. Test text and media branches with synthetic
   content in a private chat.
6. Review the publishing nodes last: they create real posts.

## What was removed on purpose

- Credential references.
- Numeric chat IDs, account IDs, spreadsheet IDs, user IDs.
- Cached spreadsheet titles and URLs.
- Private page list in the change watcher (replaced by example URLs).
- Example posts inside the agent prompts (replaced by synthetic examples).
- The commercial scheduler name in a message template.
- A canvas sticky note.

## Review checklist

- [ ] No real token, chat ID, sheet ID or private host remains.
- [ ] All `$env` expressions resolve in your instance.
- [ ] The queue sheet exists with matching columns.
- [ ] Helper scripts exist, or the command branches are disabled.
- [ ] Workflows stay inactive until the side effects are understood.
