## Tools

This directory will house the Python (and later Rust) tooling that supports the project:

- `firmware/` – firmware extraction and packaging (e.g. a future `iphone_fwextract.py` that wraps `ipsw`/`img4tool`).
- `analysis/` – scripts that parse tool output (IPSW, Image4, DTB, etc.) into structured JSON/Markdown reports.
- `status/` – generators that build/update the subsystem status matrix from collected data.

The initial focus is on small, composable Python CLIs that orchestrate existing tools rather than reimplement low-level parsers.

