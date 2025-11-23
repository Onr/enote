# enote redesign – minimal plan

## Goals
- Keep the tool as small as possible while meeting the requested flows.
- First run of `enote` should guide the user through setup.
- Subsequent runs `enote "some text"` send that text to a predefined email with a predefined subject.
- `enote -e` (or `--edit`) re-opens the setup flow to change the saved email/subject (and SMTP details).

## Technology choices
- Language: **Python 3** – comes preinstalled in most environments and includes `smtplib`/`email` in the standard library, so no extra dependencies are needed.
- Setup UI: **gum** – shell-based TUI for gathering configuration during install/edit.
- Entry point: a single executable script named `enote` with a `#!/usr/bin/env python3` shebang.
- No external packages or services beyond an SMTP server the user provides.

## Configuration & storage
- Config file lives at `$XDG_CONFIG_HOME/enote/config.json` (fallback `~/.config/enote/config.json`).
- File permissions set to `0o600` to limit exposure of SMTP credentials.
- Stored fields: `smtp_host`, `smtp_port`, `smtp_user`, `smtp_password`, `use_starttls` (bool), `to_email`, `from_email`, `subject_prefix`.
- Password is stored in plain text for simplicity (documented as such).

## CLI behavior
- `enote` (no config yet): launch gum-based setup asking for the fields above (defaults: port 587, STARTTLS on, subject prefix “Note”, from defaults to “to”).
- `enote` with no message but config present: print a short help/usage hint.
- `enote "message"`: load config and send an email whose subject is `<subject_prefix>` and body is the provided message.
- `enote -e` / `enote --edit`: rerun the gum setup to update config values.
- Errors (missing config, SMTP failures) are reported with clear messages and non-zero exit codes.

## Email sending
- Build the email with `email.message.EmailMessage`.
- Connect via `smtplib.SMTP(host, port)`, optionally `starttls()` when `use_starttls` is true, then login if `smtp_user` is provided.
- Send the message and close the connection; print a success confirmation.

## Out of scope for this pass
- Attachments, HTML email, retries, multi-recipient support, or keychain-level secret storage.
