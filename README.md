# Editable File Types for Unraid

Choose which file extensions Unraid's built-in **File Manager** opens in its text editor, from a settings page instead of editing a file on the flash drive by hand.

It adds **Settings → Utilities → Editable File Types**, with a checkbox for each common text format (`json`, `yaml`, `toml`, `md`, `env`, `log`, `conf`, and more) and a box for any others.

## Why

Unraid's File Manager only opens a clicked file in its in-browser editor when the file's extension is listed in `/boot/config/editor.cfg`. Everything else downloads or previews as binary. That file is easy to get wrong by hand: it lives on the flash drive, has no page in the web UI, and the format (one extension per line, no dots) is not documented anywhere obvious. This plugin gives it a page.

## What it does

- **Checkboxes** for common text-based extensions, ticked when the extension is already in `editor.cfg`.
- **Additional extensions** box for anything not listed (comma or newline separated, no dots, for example `hcl, rules, tf`). Extensions you added by hand are kept, never silently dropped.
- **Backups before every write.** The first time the page sees your `editor.cfg` it saves an untouched copy as `editor.cfg.orig`, and every save first copies the current file to `editor.cfg.bak`.
  - **Undo Last Change** restores `editor.cfg.bak`.
  - **Restore Original** restores `editor.cfg.orig`, the file exactly as it was before this plugin touched it.
- **Raw editor** at the bottom of the page shows the exact contents of `editor.cfg` and saves them byte for byte, for when the format your Unraid version expects differs from what the page assumes.

It reads and writes one file and nothing else. It runs nothing in the background and makes no network connections.

## Install

In Unraid, go to **Plugins → Install Plugin** and paste:

```
https://raw.githubusercontent.com/joshmans/unraid-text-editor-extensions/main/text.editor.extensions.plg
```

It should work on Unraid 7.0 and newer, but it has only been tested on 7.2 and newer (see *Tested*). There is nothing to configure: open **Settings → Utilities → Editable File Types**, tick what you want and press **Apply**.

## Format

`editor.cfg` holds one extension per line, without the dot:

```
json
yaml
log
```

The page writes that format. As a safety net, a file that has commas and no newlines at all is treated as comma-separated and written back the same way.

## Uninstall

Removing the plugin deletes only its own page. **`/boot/config/editor.cfg` is left as it is**, so the extensions you chose keep working, and the `.bak` and `.orig` backups stay next to it.

## Tested

Only on **Unraid 7.2 and newer**. Nothing in the plugin depends on a 7.2 feature, so 7.0 and 7.1 should work, but it has not been run on them.

- Installs and registers on **Unraid 7.4.0-beta.3** (checked 2026-09-25), and the page's PHP passes `php -l` and renders without warnings.
- Used on an Unraid 7.4.0-beta.2 server: saving from the page wrote `editor.cfg` and created the `.bak` and `.orig` backups.
- The Undo, Restore Original and raw-save paths have not been exercised by hand on every Unraid version. If something behaves differently on yours, please [open an issue](https://github.com/joshmans/unraid-text-editor-extensions/issues) with your Unraid version.

## Limits

- Extensions are written as you type them; the page does not check that the File Manager can actually preview a given format.
- It manages `editor.cfg` only. It does not change any other File Manager setting.

## License

MIT
