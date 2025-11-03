# Staying Organized — Summary

Clear, consistent documentation and folder structure are essential for every pentest, lab, or course. Keep scan output, logs, scope, tools, evidence (credentials, screenshots, data), and notes separated so artifacts are easy to find and reuse. Maintain a personal knowledge base (cheat sheets, payloads, commands, checklists, report templates, findings database) and keep client data local (don’t auto-sync to the cloud). Learn Markdown for fast, portable notes.

**Suggested folder layout (example)**

```
Projects/
└── <Client-or-Box>/
    ├── EPT|IPT/
    │   ├── scope
    │   ├── scans
    │   ├── logs
    │   ├── tools
    │   └── evidence/
    │       ├── credentials
    │       ├── data
    │       └── screenshots
```

**Note-taking & KB**

* Use a tool you’ll actually use (Notion, CherryTree, VS Code, Obsidian, Evernote, etc.).
* Keep sensitive client data local only (no cloud sync for real engagements).
* Store: quick setup guides, common commands, payloads, PoC snippets, checklists, report templates, and a findings database (title, description, impact, remediation, references).

**Practical tips**

* Capture and name artifacts consistently (YYYYMMDD_target_type).
* Add every useful command/payload to your KB immediately after use.
* Keep reusable report text and detection/remediation advice in templates to speed reporting.
* Start the habit now — practice documenting during the Nibbles walkthrough.
