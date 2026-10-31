# Smart Render archives

Smart Render Windows project: `D:\blender\smart_render`

## Retention policy (GitHub Releases assets)

- **Windows:** retain the 2 most recently completed successful render jobs.
- **GitHub:** retain **at most 10 Smart Render Releases**, each with a downloadable ZIP archive asset. Smart Render tags have the form `sr-render-YYYYMMDD-HHMMSS-id`.
- Archive the old job's files, upload them using the GitHub CLI, **download them again and verify their SHA-256**, and only then delete the corresponding local job via Smart Render API.
- GitHub retention deletes the oldest Smart Render Release **and its tag**, including the associated Release asset. It never deletes unrelated GitHub Releases, source code, or commits.
- Queued/running jobs are never cleaned. Old failed jobs may be archived only after their error-aging window.
- Any upload, authentication, downloaded checksum, or API failure preserves the local job.

**Important:** This is a **public** GitHub repository. Files uploaded as Release assets are public. Use a private repository for private character models and development assets.

**Why Releases instead of Git commits?** Removing file paths from a Git branch does not remove the older Git blobs from commit history. ZIP archives stored as Release assets are not inserted into ordinary Git commit history, and the Release asset can be deleted via GitHub's release API. GitHub CDN caching may have propagation delays; do not treat removal as an immediate privacy erasure.

## Setup and commands

1. Stop old `cleanup_render_jobs.py --watch` instances. The installed guard also disables direct old cleanup.
2. Install/authenticate the GitHub CLI locally: `winget install --id GitHub.cli -e`, then `gh auth login`.
3. Run `python archive_render_jobs.py --check`.
4. Run `python archive_render_jobs.py --once`.
5. When successful, run one monitor: `python archive_render_jobs.py --watch --interval 60`.

The ZIP contains `_archive_manifest.json` with SHA-256 values for source files. The archive script currently leaves separately stored generated GLBs alone unless explicitly included, because another job may still need them.

The legacy `render_archive/` Git-commit layout is retired. If large binary archives were ever committed to Git history, their historic blobs require a separate, reviewed Git-history rewrite; deleting Releases does not affect those historic objects.
