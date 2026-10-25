# Setup and configuration

The workflows import inactive. Nothing runs until credentials, identifiers and the two helper scripts are provided.

## Values to configure

| Placeholder | Where | Replace with |
| --- | --- | --- |
| `TELEGRAM_CHAT_ID` | Status and confirmation nodes | Operator chat ID |
| `QUEUE_SHEET_ID` | Google Sheets nodes | Spreadsheet ID |
| `THREADS_ACCESS_TOKEN` | Token nodes and publish calls | Threads long-lived access token |
| `THREADS_CLIENT_SECRET` | Token refresh call | App secret |
| `THREADS_USER_ID` | Threads endpoints | Account ID |
| `SOCIAL_ACCOUNT_ID` | Set node | Your own account label |
| `WORK_DIR` | File nodes | Directory for downloaded media |
| `SCRIPTS_DIR` | Command nodes | Directory holding the helper scripts |
| `REPLACE_WITH_SYNDICATION_TOKEN` | Twitter embed request | Public token for that endpoint |

If environment access is disabled in n8n, keep these values in a private runtime configuration that is never committed, or use credentials where the node supports them.

## Credentials

- Telegram bot
- X/Twitter OAuth with media upload
- Google Sheets OAuth
- Google Drive OAuth
- DeepSeek or a compatible chat-model provider

## Spreadsheet

Create a sheet with at least these columns: `Tweet`, `Link`, `Fecha`, `Estado`. The flows read and write `Estado` with values such as `PROGRAMADO`. Adjust the column mappings if your names differ.

## Helper scripts

Two command nodes call scripts that are not included:

| Node | Flow | Expected behaviour |
| --- | --- | --- |
| `Execute Command1` | Social publishing | Accept a post URL and write the video file to `WORK_DIR` |
| `Execute Command` | Page-change watcher | Render a page and return JSON with `stable` text and `url` |

Point `SCRIPTS_DIR` at your own implementations, or disable those branches.

## Import steps

1. Import the three JSON files into a disposable instance.
2. Create the credentials and fill the placeholders.
3. Recreate the spreadsheet and update the Google Sheets nodes.
4. Check the two switch nodes against your own routing rules.
5. Keep every workflow inactive. Test text and media branches with synthetic content in a private chat.
6. Review the publishing nodes last: they create real posts.

## Review checklist

- [ ] No real token, chat ID, sheet ID or private host is included.
- [ ] All `$env` expressions resolve in your instance.
- [ ] The queue sheet exists with matching columns.
- [ ] Helper scripts exist, or the command branches are disabled.
- [ ] Workflows stay inactive until the side effects are understood.
