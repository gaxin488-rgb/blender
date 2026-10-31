# Smart Render archives

This repository is configured as the destination for archived Smart Render jobs from `D:\blender\smart_render`.

**Important:** This repository is **public**: archived renders, GLB/BLEND source files, reports, and image assets committed here will be publicly downloadable.

The local cleanup workflow must keep the two most recently completed jobs and never remove another job until its archive is committed, pushed, and verified by downloading it back from GitHub. Failed uploads must leave source jobs untouched.

Render archives will appear under `render_archive/` after a successful local upload. The archive script uses a short-lived temporary Git clone so old Git objects are not retained in the local project directory. For very large assets, Git LFS has storage/bandwidth limits and should be enabled only after checking the account's spending controls.
