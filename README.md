# go-wasmcontainers-usercontent.github.io

The second origin of the [go-wasmcontainers playground](https://go-wasmcontainers.github.io/playground/):
it will show the web pages of containers whose ports are published with
`wctr run -p 8080:80 …`, on a site of its own (github.io is on the Public
Suffix List), so that a container's page can never read the playground's
storage or script it.

## What is served

Exactly what [`site usercontent`](https://github.com/go-wasmcontainers/playground/tree/main/internal/usercontent)
writes, from a pinned playground release, with the playground's origin
written in:

```
go run github.com/go-wasmcontainers/playground/cmd/site@v0.4.0 \
  usercontent --allow https://go-wasmcontainers.github.io --out _site
```

| file | what it is |
|---|---|
| `index.html` | a short page saying there is nothing to see here directly |
| `shell.html`, `shell.js` | the shell: embedded by the playground only, it shows one published port and relays its requests |
| `sw.js` | the service worker that serves `/s/<session>/<port>/…` through the shell, caching nothing |

The design, the security model and its limits are in the playground's
[README](https://github.com/go-wasmcontainers/playground#readme) and
go-wasmcontainers/playground#19. Nothing of this repository besides that
output is published; the workflow (`.github/workflows/pages.yml`) holds no
secret and asks only for `pages: write` and `id-token: write`, in the deploy
job.

## Status

The shell of playground **v0.4.0** is served. `-p` in the browser is not
usable yet: the playground's view tab and the engine worker's side come with
the engine release that has `portmap.Virtual` (see the playground README).

## Updating

Change `PLAYGROUND` in `.github/workflows/pages.yml` to the new playground
release tag, through a pull request; merging deploys it.
