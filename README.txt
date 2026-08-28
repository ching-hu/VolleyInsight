VolleyInsight User Manual Package
=================================

This directory contains the finalized, GitHub Pages-compatible VolleyInsight user manual verified on August 27, 2026.

Files
-----

index.html                  Self-contained responsive and printable manual.
screenshot_manifest.json    Stable IDs and factual states for the published captures.
CAPTURE.md                  Reproducible simulator build/capture notes and discrepancy log.
screenshots/                 Twenty real iPad Simulator PNG captures using synthetic data.

Local preview
-------------

From the repository root:

    python3 -m http.server 8765 -d docs/VolleyInsightUserManual

Then open http://127.0.0.1:8765/.

Publishing
----------

The page uses only relative paths and has no build step or external runtime dependency. Publish the contents of this directory as a static site, or keep it under docs/ for a repository-level GitHub Pages workflow.

Safety
------

All published screenshots use the isolated in-memory VolleyInsight documentation dataset. Do not replace them with real minors, contact details, club records, backups, or private tournament data.

