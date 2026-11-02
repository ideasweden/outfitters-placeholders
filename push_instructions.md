# Push Instructions — outfitters-placeholder-hosting

This repo is fully prepared locally (119 tiles + `manifest.csv` + README,
single commit). All that remains is authenticating `gh` as Russell's GitHub
account and pushing. **Time pressure: the 21 wave-1 listings currently point
at temporary URLs that expire 2026-11-04 ~14:35–14:48 UTC — push and re-host
before then if at all possible.**

## 1. Authenticate GitHub CLI (Russell's account)

```bash
gh auth login
#   → GitHub.com → HTTPS → log in as Russell's GitHub account (ideasweden)
gh auth status        # confirm the active account before continuing
```

## 2. Create the public repo and push

Run from this directory (`~/workspace/outfitters-placeholder-hosting`):

```bash
gh repo create outfitters-placeholders --public --source=. --remote=origin --push
```

That creates the public repo `outfitters-placeholders` under the
authenticated account, adds it as `origin`, and pushes the existing commit.
If the repo already exists remotely, instead:

```bash
git remote add origin https://github.com/<owner>/outfitters-placeholders.git
git push -u origin main
```

## 3. Verify the URLs resolve

```bash
curl -I https://raw.githubusercontent.com/<owner>/outfitters-placeholders/main/198762490703.jpg
# expect: HTTP 200, content-type image/jpeg
```

Spot-check 2–3 more item IDs from `manifest.csv`. Only proceed once raw
URLs return 200 — eBay will fetch these directly.

## 4. Follow-on: apply the tiles to the listings (checkpointed)

Working files live in
`~/workspace/ebay-sync/outfitters/placeholders/run_artifacts_retrofit_20261102/`
(`retrofit_results_20261102.json` is the checkpoint; `generated/` holds the
same 119 tiles this repo hosts).

1. **Re-host the 21 expiring first** (status `verified` in the manifest):
   their live `picurl` values are the expiring temporary URLs. Revise each
   listing's gallery to its raw GitHub URL using the
   `retrofit_revise.py` pattern (Trading `ReviseFixedPriceItem`, picture-only
   change), then GetItem-verify and update the checkpoint (`picurl` → raw
   URL, drop `picurl_expires`).
2. **Then the 98 pending** (status `pending`): same revise flow from their
   raw URLs, one listing at a time, GetItem-verify each, mark `verified` in
   `retrofit_results_20261102.json` as you go — the checkpoint is the resume
   point if the run is interrupted.
3. Respect the quota governor before the batch (Trading API pool is shared
   across all stores; ~2 calls per listing ≈ 238 calls for all 119). If the
   pool is AMBER/RED, do the 21 expiring first and defer the 98.
4. `retrofit_upload.py` (the old temporary-host uploader) is NOT needed for
   this lane — the raw GitHub URLs replace it entirely.
5. If any revise fails or a raw URL 404s, stop that listing, leave its
   checkpoint status unchanged, and note it in the run report — never leave
   a listing pointing at a dead gallery; fall back to its previous PicURL
   if a revise goes wrong.
