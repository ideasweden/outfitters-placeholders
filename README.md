# IDEA OUTFITTERS — Placeholder Tile Hosting

This repository hosts the category placeholder tiles used in IDEA OUTFITTERS
eBay listings whose product photography is not yet available (the "mule"
placeholder strategy for VeRO-risk lines: listings go live with a neutral
category tile instead of a source/brand photo).

## Contents

- `<item_id>.jpg` — one tile per eBay listing, named by the listing's eBay
  item ID. Each tile is a 1600×1600 image carrying the listing title, its
  category label, an "Image coming soon" notice, and a category illustration.
- `manifest.csv` — maps every tile to its listing: `item_id`, `filename`,
  `title`, `category_label`, and `status` (`verified` = already live on the
  listing, `pending` = generated, awaiting revision onto the listing).
- `push_instructions.md` — how this repo gets published and how the tiles
  get applied to the listings afterwards.

## URL format

Once this repo is pushed to GitHub as a public repository, every tile is
permanently reachable at:

```
https://raw.githubusercontent.com/<owner>/<repo>/main/<item_id>.jpg
```

`raw.githubusercontent.com` URLs are permanent direct image links, suitable
for use as eBay `PictureURL` values — unlike temporary file-hosting links,
they do not expire.

## Artwork provenance

The tiles are our own generated artwork: original vector-style illustrations
and public-domain icon composites (OpenClipart / freesvg.org sources,
registered in the build manifest), rendered with the listing's title and
category text. No brand or retailer photography is used in any tile.

## Background

Wave 1 of the placeholder retrofit (2026-11-02) revised 21 listings onto
temporary hosted URLs that expire 2026-11-04; 98 further listings are
checkpointed pending. This repo is the durable end-state: once pushed, all
119 tiles get (re-)applied to their listings from the permanent raw URLs.
