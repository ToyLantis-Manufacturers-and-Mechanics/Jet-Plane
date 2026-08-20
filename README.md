# Jet Plane

Jet Plane is a macOS security utility that scans and neutralizes malicious URLs and files, and provides a robust application uninstallation tool to remove unwanted or potentially harmful apps.

[![License](https://img.shields.io/badge/license-MIT-blue.svg)]() [![Build](https://img.shields.io/badge/build-passing-brightgreen)]() [![Release](https://img.shields.io/badge/release-v0.0.0-lightgrey)]()

Table of Contents
- [Features](#features)
- [Requirements](#requirements)
- [Installation](#installation)
- [Quick Start](#quick-start)
- [Configuration](#configuration)
- [How scanning works](#how-scanning-works)
- [Uninstallation tool](#uninstallation-tool)
- [Permissions & Privacy](#permissions--privacy)
- [Troubleshooting](#troubleshooting)
- [Contributing](#contributing)
- [Security](#security)
- [License](#license)
- [Acknowledgements](#acknowledgements)
- [Changelog](#changelog)

## Features
- URL scanning: detect and quarantine or block known malicious URLs before they are opened.
- File scanning: scan downloads, attachments, and user-selected files for known threats.
- Neutralization: safely quarantine or remove infected files while preserving user data when possible.
- App uninstallation: deep uninstall tool that removes application files, launch agents/daemons, preferences, and containers.
- Configurable actions: per-threat decision (quarantine, delete, ignore), and logging.
- Lightweight macOS-native UI and an optional CLI interface for automation.

## Requirements
- Minimum: macOS Monterey 12.0 (12.x).
- Supported: Intel and Apple Silicon (universal build recommended).
- Administrator privileges are required for system-level uninstall operations.

## Installation
Official releases are distributed from the Releases page as signed installers (DMG or PKG). Follow the instructions on each release for installation.

### Installation & Setup Guide
Because Jet Plane is distributed outside the Apple App Store, modern macOS Gatekeeper may occasionally show a security warning. Follow these steps to install and launch safely.

Step 1 — Download & Move
1. Download the latest Jet Plane release from the Releases page.
2. Move Jet Plane.app into your Applications folder (or leave it on your Desktop).

Step 2 — Clear the macOS Quarantine Flag (If Needed)
If macOS shows a warning that the app is "damaged" or cannot be opened because it's from an unidentified developer, open Terminal and run:

```
xattr -cr /Applications/Jet\ Plane.app
```

If you placed the app on the Desktop instead, replace the path:

```
xattr -cr ~/Desktop/Jet\ Plane.app
```

Step 3 — Launch Jet Plane
After clearing the quarantine flag (if needed), double-click Jet Plane.app to open it. If you see a Gatekeeper warning when first opening the app, open it via Finder → right-click → Open and confirm.

Notes
- Some operations (system-level uninstall, removing LaunchDaemons/LaunchAgents) require administrator approval.
- Use the app's dry-run or quarantine options before performing destructive removals.

## Quick Start
1. Launch Jet Plane from Applications.
2. Go to Settings → Scanning and enable Real-time URL scanning.
3. To scan a file: File → Scan file… or right-click → Services → Scan with Jet Plane.
4. CLI example (if applicable):

```
/usr/local/bin/jetplane scan --file ~/Downloads/suspicious.zip --quarantine
```

## Configuration
Example config (YAML)

```
scan:
  realtime_url: true
  realtime_file: true
  quarantine_path: ~/JetPlane/Quarantine
actions:
  on_malware: quarantine
  on_pua: prompt
logging:
  level: info
```

Configuration can be edited from Preferences or via the config file in the user configuration directory.

## How scanning works
- URL scanning: Jet Plane checks URLs against local signatures and an optional remote blocklist (if enabled). Caching and privacy options are available for remote lookups.
- File scanning: signature-based matching plus optional heuristic/static analysis. Any third-party engines or signature sources used are disclosed in the app or documentation.
- Neutralization: suspected files are moved to a quarantine directory and may be removed only after user confirmation. Recovery steps are available in the app.

## Uninstallation tool
Jet Plane's deep-uninstall removes both system-wide and user-specific artifacts. Behavior:
- System (/Library) removals (require admin):
  - /Library/Application Support/<app-related>
  - /Library/LaunchAgents/<...>
  - /Library/LaunchDaemons/<...>
  - Other system-level files created by the app
- User (~/Library) removals:
  - ~/Library/Preferences/<app-related>.plist (preferences)
  - ~/Library/Containers/<app-related> (app containers)
  - ~/Library/Application Support/<user-specific files>

Example UI flow: App → Uninstall → choose app → Jet Plane lists files to delete → confirm.

CLI uninstall (example — no bundle identifier required in README):

```
sudo /usr/local/bin/jetplane uninstall --app "Jet Plane" --dry-run
```

Caution: System-level removals modify /Library and require administrator confirmation. Use the dry-run or quarantine options before destructive actions.

## Permissions & Privacy
- Required permissions: Full Disk Access may be requested for scanning user files; Network access is used for optional blocklist lookups/updates.
- Data handling: The app documents what telemetry (if any) is collected, what is sent remotely (e.g., hashes or URLs), opt-in/opt-out options, and retention policies.
- Provide a privacy policy link if user data is collected or transmitted.

## Troubleshooting
- If scanning fails: Check Preferences → Logs for details.
- Quarantine recovery: Open Jet Plane → Quarantine → Select file → Restore.
- If uninstall fails: use the app's dry-run option and inspect logs.

## Contributing
Contributions, bug reports, and feature requests are welcome. Please open issues or pull requests following the repository's contribution guidelines (see CONTRIBUTING.md).

## Security
If you discover a vulnerability, please DO NOT post exploit details in a public issue. Preferred: open a GitHub Security Advisory for this repository at:

https://github.com/ToyLantis-Manufacturers-and-Mechanics/Jet-Plane/security/advisories

If you cannot access Security Advisories, open a new issue titled "SECURITY: <short summary>" and we will follow up privately. Do not include exploit code, credentials, or other sensitive data in a public issue.

Include in your report:
- A short description of the issue
- Steps to reproduce (minimal, safe reproduction preferred)
- Impact (what an attacker can do)
- Any suggested mitigations

We will acknowledge incoming reports via the Security Advisory or issue thread; please allow up to 5 business days for a response.

## License
This project is licensed under the MIT License — see the LICENSE file for details.

## Acknowledgements
- List libraries, tools, or signature providers you used.
- Thank contributors and testers.

## Changelog
See CHANGELOG.md for release notes and version history.
