# higgsfield-transient

Transient image host for Alfrada's `video_lab` → Higgsfield routing path.

Higgsfield's API validates `image_url` as a real URL (RFC 2083-char cap), so
session-local images can't be passed inline. Alfrada's `HiggsfieldProvider`
PUTs them here via the GitHub Contents API, hands the
`raw.githubusercontent.com` URL to Higgsfield, and DELETEs the blob in a
`finally` block once the generation task hits any terminal state (completed,
failed, nsfw, cancelled, timeout). Nothing should accumulate under
`higgsfield-transient/` for more than a few minutes during normal operation.

If you see files lingering here, an Alfrada API process crashed mid-task or
lost network connectivity before cleanup. Safe to delete by hand.

**Do not push anything else here.** This repo is configured as the upload
target via:

- `HIGGSFIELD_GITHUB_REPO=strategizelabs/higgsfield-transient`
- `HIGGSFIELD_GITHUB_TOKEN=<fine-grained PAT, Contents: read & write>`
- `HIGGSFIELD_GITHUB_BRANCH=main`
- `HIGGSFIELD_GITHUB_PATH_PREFIX=higgsfield-transient`
