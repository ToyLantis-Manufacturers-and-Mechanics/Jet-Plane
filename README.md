# Jet Plane

Jet Plane is a macOS security utility that scans and neutralizes malicious URLs and files, and provides a robust application uninstallation tool to remove unwanted or potentially harmful apps.

[![Release](https://img.shields.io/github/v/release/ToyLantis-Manufacturers-and-Mechanics/Jet-Plane?label=release)](https://github.com/ToyLantis-Manufacturers-and-Mechanics/Jet-Plane/releases) [![License](https://img.shields.io/github/license/ToyLantis-Manufacturers-and-Mechanics/Jet-Plane)](https://github.com/ToyLantis-Manufacturers-and-Mechanics/Jet-Plane/blob/main/LICENSE)

Table of Contents
- [Features](#features)
- [Requirements](#requirements)
- [Installation](#installation)
- [How scanning works](#how-scanning-works)
- [Engine behavior & persistence](#engine-behavior--persistence)
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
- Web defense (scanWebsite): a dedicated scanWebsite() routine inspects and neutralizes malicious URLs and remote threats before they reach the user's environment.
- Global persistence & protection: the engine hooks into application lifecycle events to intercept global quit requests and prevent accidental or malicious shutdowns of core defenses.
- Onboarding & permissions: a polished onboarding sequence requests required permissions (including Full Disk Access) in a clear, privacy-forward flow so the engine can scan effectively.
- Multi-tier scanning: Quick Scan (caches & apps), Deep Scan (user Home directory), and Maximum Sweep (root-level) options to balance speed, coverage, and privacy.
- Update & telemetry controls: configurable updateFrequency (Every Launch / Daily / Weekly) with optional opt-in remote checks and clear privacy controls.
- Smart uninstaller: a residue-conquering uninstaller that locates and removes system-level and user-level leftovers (caches, preferences, containers, launch items).
- System optimizer: performance safeguards and throttling so scanning and background operations remain efficient on supported macOS versions.

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

## How scanning works
- URL scanning: Jet Plane checks URLs against local signatures and an optional remote blocklist (if enabled). Caching and privacy options are available for remote lookups.
- File scanning: signature-based matching plus optional heuristic/static analysis. Any third-party engines or signature sources used are disclosed in the app or documentation.
- Neutralization: suspected files are moved to a quarantine directory and may be removed only after user confirmation. Recovery steps are available in the app.

## Engine behavior & persistence
- scanWebsite(): inspects URLs before they’re opened using local signatures and optional remote blocklist lookup. Configurable caching and privacy controls are available.
- Application lifecycle protection: the engine intercepts global quit events to avoid accidental shutdown; critical services can be restarted automatically if terminated unexpectedly.
- Onboarding: the first-run flow requests permission dialogs (network access, Full Disk Access) in a step-by-step UI and explains what each permission is used for.
- Automatic recovery: critical background services monitor themselves and will attempt to restart or notify the user if halted.

## Uninstallation tool (Residue-Conquering Uninstaller)
Jet Plane’s uninstaller is designed to remove both visible and hidden residues:

- System-level removals (admin required)
  - /Library/Application Support/<app-related>
  - /Library/LaunchAgents/<...>
  - /Library/LaunchDaemons/<...>
  - System-level caches, receipts, and support files created by the app

- User-level removals
  - ~/Library/Preferences/<app-related>.plist (preferences)
  - ~/Library/Containers/<app-related> (app containers)
  - ~/Library/Application Support/<user-specific files>
  - ~/Library/Caches/<app-related>

- CLI and UI modes
  - UI flow: App → Uninstall → choose app → Jet Plane lists files with checkboxes → confirm (dry-run option available)
  - CLI example:

```
sudo /usr/local/bin/jetplane uninstall --app "Jet Plane" --dry-run
```

Caution: System-level removals modify /Library and require administrator confirmation. Use the dry-run or quarantine options before destructive actions.

## Permissions & Privacy
- Required permissions: Full Disk Access may be requested for scanning user files; Network access is used for optional blocklist lookups/updates.
- Data handling: The app documents what telemetry (if any) is collected, what is sent remotely (e.g., hashes or URLs), opt-in/opt-out options, and retention policies.
- Provide a privacy policy link if user data is collected or transmitted.

## Troubleshooting
- If scanning doesn't work: ensure Jet Plane has Full Disk Access (System Settings → Privacy & Security → Full Disk Access) and that the macOS quarantine flag has been cleared for the app (run `xattr -cr /Applications/Jet\ Plane.app` if applicable).
- There is no quarantine recovery feature in this version; quarantined files are handled according to system policies or the configured quarantine directory (if enabled).
- If uninstall fails: make sure the uninstaller is run with administrator privileges or that you provided admin credentials when prompted.

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
