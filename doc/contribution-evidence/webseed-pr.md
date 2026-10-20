# WebUI: Preserve web seed URLs when editing

The WebUI web seed editor decodes the URL twice when opening the dialog. For example, `https://example.com/a%2Fb/?key=100%25` is displayed as `https://example.com/a/b/?key=100%`. Editing a URL can therefore change its meaning or fail even when the user leaves it unchanged.

There is also a mismatch when submitting the original URL: `newUrl` is percent-encoded before being added to the form, but `origUrl` is not. The WebAPI explicitly percent-decodes both values in `validateWebSeedUrl()`, so an escaped original path can be looked up as a different URL.

Use the value already decoded by `URLSearchParams` directly in the input, and encode `origUrl` in the same way as `newUrl` before submission.

Validation:
- Added seven regression cases executing the actual dialog script, covering unchanged and edited submissions, escaped path/query characters, percent signs and Unicode.
- `npm test`: 14 tests passed.
- `npm run lint` and `npm run format`: passed; formatting leaves the patch unchanged.
- Chromium: the actual editor preserves the displayed URL; captured form bodies preserve both URLs on unchanged submission and on an edited submission using Enter. Browser checks used local fixtures and a stub HTTP endpoint.

Before/after from the local browser fixture:

![Web seed URL before and after the fix](https://raw.githubusercontent.com/c8dhjp4tyv-bit/qBittorrent/contribution-evidence-webui-20261020/doc/contribution-evidence/webseed-before-after.png)

Prepared with AI assistance. This draft is awaiting human review.
