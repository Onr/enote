# enote (minimal)

Lightweight CLI that emails a short note using saved SMTP settings, with setup driven by a gum-powered TUI.

## Quick start
- Prereqs: Python 3 and [gum](https://github.com/charmbracelet/gum) installed and on PATH.
- Run `./enote` once. It launches the gum setup flow to collect SMTP host/port, auth (optional), recipient, from, and subject prefix. Config is stored at `$XDG_CONFIG_HOME/enote/config.json` (fallback `~/.config/enote/config.json`, permissions 600).
- Send a note: `./enote "message text"`.
- Edit saved settings: `./enote -e` (opens gum setup again). You can also run `./enote-setup` directly.
- Browse sent notes: `./enote -l` opens a gum picker showing the first line of each note; selecting one shows the full text.
- Clear all history: `./enote -l` and choose “Delete all notes” from the list menu; this requires an extra confirmation before deleting.

## Behavior
- First run with no config triggers the gum-based setup before doing anything else.
- `./enote` with no message prints a short usage hint.
- Emails are plain text, using Python’s standard `smtplib` and `email` modules.
- Passwords are stored in plain text in the config file; keep the host secure.

See `DESIGN.md` for the design decisions behind this rewrite.
