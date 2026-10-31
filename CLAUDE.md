# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

Jekyll site for Ariel Chen Realty (https://www.arielchenrealty.com), built on the Atega theme. No tests or linter; a successful `jekyll build` is the check.

## Commands

```bash
bundle install
bundle exec jekyll serve --force_polling   # http://localhost:4000
bundle exec jekyll build                   # outputs to _site/
```

- Ruby version is pinned in `.ruby-version` (3.3.11); the README's "Ruby 2.7" is outdated. The Gemfile pins `logger ~> 1.5.3` and adds `webrick` for compatibility with this Ruby/Jekyll combo.
- `--force_polling` is needed on Apple Silicon: the locked `rb-fsevent` gem ships an x86-only watcher binary, so without it auto-regeneration silently dies and edits never show up.

## Deployment

`.github/workflows/` builds and deploys to GitHub Pages via Actions on every push to `main`. Pushing to `main` is publishing to the live site.

## Architecture

The homepage (`index.html`) is essentially the transactions showcase; the blog post grid is commented out.

- **`_data/transactions.yml`** drives the "Recent Transactions" section. Each entry: `addr`, `description` ("N beds N baths"), `state` (`listing` | `pending` | `coming soon` | `sold`), `represent` (`seller` | `buyer`), `image`, optional `url`. `index.html` renders three groups in order — listing, then pending/coming soon, then sold — each preserving file order, so keep the file sorted newest-first within each state.
- **`_includes/transaction.html`** renders one card. The card links to `url` if set (Redfin pages are used), otherwise to a Google Maps search of `addr`. Images are lazy-loaded (`class="lazy"` + `data-src`, vanilla-lazyload in `js/common.js`). If `image` is omitted it falls back to `/assets/transactions-cover/{{addr}}.jpeg`.
- Cover images live in `assets/transactions-cover/`. Newer entries are named `<MLS#>.jpg` (e.g. `ML82062992.jpg`); older ones are named by address.
- `_config.yml` holds hero text, author, social links, Formspree contact ID, and GA4 ID. `_pages/` are standalone pages (permalink `/:basename`). `_posts/en-US/` holds a few old listing posts; `_archived_posts/` is not built.
- `js/scripts.js` is a Jekyll-processed bundle that `include_relative`s files from `js/vendors/` and `js/common.js`.

## Refreshing transactions from MLS

Data comes from MLSListings Matrix (https://search.mlslistings.com/Matrix/Search/Residential/ResidentialSearch), which requires the user to sign in themselves.

- Ariel's license is DRE 02038464 (also in the footer widget). Search Residential twice: once with "List Agent Lic #" = 02038464 (seller side) and once with "Buyer's Agent Lic #" = 02038464 (buyer side), all statuses. Compare MLS numbers against what's already in the YAML.
- Status mapping: Active → `listing`, Pending/Contingent → `pending`, Sold → `sold`. Canceled/withdrawn/expired listings get removed along with their cover image.
- Full-res photos: opening a listing's photo viewer exposes `MediaServer/GetMedia.ashx?Key=…&Number=0&Size=5&exk=…` URLs; the `exk`-signed URL can be downloaded with plain `curl` (no session cookie needed).
- Redfin URLs for active listings: Redfin's `/stingray/do/location-autocomplete?location=<address>&v=2` endpoint returns the home URL when fetched from a page on redfin.com (direct navigation is blocked).
