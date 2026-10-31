# Smart Render — source and render archives

Repository for the local Windows Smart Render project at `D:\blender\smart_render`.

## Render retention policy

- **Local Windows:** retain the **2 most recently completed** render jobs.
- **GitHub (`main` branch):** retain **at most 10 recent completed jobs** under `render_archive/<job-id>/`, ordered by the timestamp in the job ID.
- When an old local job becomes eligible, the local archiver creates a ZIP, splits it into 20 MiB Git blobs, pushes the archive **and** removes the oldest GitHub archives beyond 10 **in the same commit**, and then **re-clones the repository to check the remote ZIP SHA-256** before deleting the local job through the Smart Render API.
- Render jobs currently queued or running and jobs marked as errors are not automatically deleted. A failed Git push or checksum check does not authorize local deletion.
- The new archive monitor is `python archive_render_jobs.py --watch --interval 60`; the old `cleanup_render_jobs.py --watch` must stay stopped. The cleanup guard prevents direct deletion by the old cleanup script.

**Important privacy warning:** This repository is **public**. Rendered images, models, Blender scenes and logs uploaded here can be read and downloaded by anyone.

**Git storage warning:** Removing `render_archive/<job-id>` from the current Git branch does **not** remove the binary blobs from earlier commits or immediately reclaim the repository's historical storage. A strict storage quota needs a separate Git-history strategy or an artifact/object-storage service with real deletion and retention controls.

## Recovery

Each archive has `manifest.json` listing the ZIP SHA-256 and part SHA-256 values. Restore by downloading all `part-NNNN.bin` files in order, concatenating them into the original ZIP and validating its checksum before extraction.

Render archive status is currently empty; this README documents the intended behavior rather than claiming an archive was uploaded.
