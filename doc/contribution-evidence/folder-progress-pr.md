# WebUI: Fix folder progress for ignored files

Folder progress and availability currently exclude ignored files from their numerators but still divide by the full folder size. A folder containing a completed, available 1 MiB file and an ignored 3 MiB file consequently shows 25% progress and availability, despite having no selected data remaining.

Track the selected byte count separately from the displayed total size, and use it to weight both averages. Carry that count through nested folders so mixed subfolders are weighted by their selected data too. Folders with no selected bytes are complete.

Validation:
- Added eight regression cases covering ordinary size-weighted averages, Normal/High/Maximum priorities, nested mixed and ignored folders, all-ignored data, zero-length files, and recalculation after priority changes.
- `npm test`: 15 tests passed.
- `npm run lint` and `npm run format`: passed; formatting leaves the patch unchanged.
- Chromium: the real file-tree model, table cell renderers and progress-bar component display 100% instead of 25% for the example above. The displayed total remains 4 MiB and remaining data stays 0 B. Browser checks used local fixtures.

Before/after from the local browser fixture:

![Folder progress and availability before and after the fix](https://raw.githubusercontent.com/c8dhjp4tyv-bit/qBittorrent/contribution-evidence-webui-20261020/doc/contribution-evidence/folder-before-after.png)

Prepared with AI assistance. This draft is awaiting human review.
