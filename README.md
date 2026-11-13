# WhatsApp Gemini Bot

An inbound-only WhatsApp bot using Baileys and the Gemini API. It replies to plain-text messages in private chats, keeps a short in-memory conversation history per chat, and ignores groups, status updates, and broadcasts.

## Start

The project uses Replit Secrets for credentials. Set `PAIRING_NUMBER` and `GEMINI_API_KEY` in the project’s Secrets; do not put them in source files or commit a `.env` file.

From the workspace root, run:

```sh
pnpm install
pnpm --filter @workspace/whatsapp-bot start
```

On first start, copy the pairing code from the bot logs and enter it in WhatsApp under **Settings → Linked Devices → Link a Device → Link with phone number instead**. The bot saves linked-device credentials in `auth_info_baileys/` so it does not ask for a new code on every restart.

`PAIRING_NUMBER` may include a leading `+` or separators; the bot removes non-digits before requesting the code. The number must include its country code.

## Security and behavior

- Pairing links the WhatsApp number as a WhatsApp Web device. Baileys is an unofficial client and may carry account-restriction risk.
- Only incoming private messages with text are sent to Gemini. The bot does not reply in groups or to status/broadcast messages.
- WhatsApp credentials are stored locally in `auth_info_baileys/`; this directory is excluded from version control and deployment files. Back it up securely if you need to preserve the linked session.
- Conversation history exists only in memory and is cleared when the process restarts.
- Gemini requests use the configured `GEMINI_API_KEY` header and the supported `gemini-3.8-flash` model.
