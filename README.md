# enote

Lightweight CLI that emails a short note using saved SMTP settings, with setup driven by a gum-powered TUI.

## Requirements
- Python 3
- [gum](https://github.com/charmbracelet/gum) installed and on `PATH` (for setup and the history UI)

## Quick start
- Run `./enote` once. It launches the gum setup flow to collect SMTP host/port, auth (optional), recipient, from, and subject prefix. Config is stored at `$XDG_CONFIG_HOME/enote/config.json` (fallback `~/.config/enote/config.json`, permissions `600`).
- Send a note inline: `./enote "message text"`.
- Send from a file: `./enote -f note.txt`.
- Edit saved settings: `./enote -e` (opens gum setup again). You can also run `./enote-setup` directly.
- Browse sent notes: `./enote -l` opens a gum-powered picker showing the first line of each note; selecting one shows the full text.
- Clear all history: run `./enote -l` and choose “Delete all notes” from the list menu; this requires an extra confirmation before deleting.

## Usage examples

Send a quick note:

```bash
./enote "Don’t forget to review the pull request."
```

Send the contents of a file:

```bash
./enote -f ~/notes/todo.txt
```

List and browse previously sent notes:

```bash
./enote -l
```

Re-run the setup flow to change SMTP settings, recipient, or subject prefix:

```bash
./enote -e
```

## Finding SMTP settings (examples)
- **Gmail**: host `smtp.gmail.com`, port `587`, STARTTLS `yes`, username = your full Gmail address. You must use an app password (or another provider that allows password-based SMTP).
- **Outlook / Office 365**: host `smtp.office365.com`, port `587`, STARTTLS `yes`, username = your full Microsoft 365 email.
- **Yahoo Mail**: host `smtp.mail.yahoo.com`, port `587`, STARTTLS `yes`, username = your full Yahoo email.
Check your email provider’s “SMTP settings” documentation for the exact host, port, and security settings you should enter in the setup TUI.
