# Deferred Link Items

Items identified by the ravencoin.org link audit (1,214 link instances across 91 files) that were
**not** changed in this pass, with the reason for each. They are kept as-is deliberately and are
listed here so they can be revisited later.

## 1. Server errors — service may only be temporarily unavailable

These URLs responded with 5xx / Cloudflare errors during the audit. A transient server error is not
evidence that a service is gone, so the links were left untouched. Re-check before changing.

| # | URL | Observed status | Location |
|---|---|---|---|
| E1 | `https://squawker.app` | 500 | `_data/projects.yml` |
| E2 | `https://rvnbay.com`, `https://rvnbay.com/category/rvnft/` | 525 (Cloudflare) | `_data/projects.yml`, `_pages/assets.md` |
| E3 | `https://graviex.net/markets/rvnbtc` | 522 | `_data/exchanges.yml` |
| E4 | `https://pool.wallstreetv2.com/` | 522 | `_data/pools.yml` |
| E5 | `https://ravencoin.flypool.org/` | 522 | `_data/pools.yml` |
| E6 | `https://www.crypticwizardry.com/` | 502 | `_data/pools.yml` |
| E7 | `https://rvn-dashboard.com/` | 503 | `_pages/index.md` |
| E8 | `https://safepay.safecoin.org/` | 503 | `_data/wallets.yml` |
| E9 | `https://explorer.ravencoin.org/block/0000000000004a7a699167d242dad3e18271cb9b75fdab9fe749291d9c59305d` | 522 | `_pages/halving.md` |
| E10 | `https://raven-nightly-builds.ravencoin.org` | 522 | `_pages/wcc.md` |
| E11 | `https://api.ravencoin.org` | 522 | `mobilewallet/support/Troubleshooting/Ravencoin-missing.html` |

## 2. Working links that were left alone

| Item | Reason |
|---|---|
| `https://twitter.com/...` (all occurrences) | Works via redirect to `x.com`. The report treats this as normal operation, not a defect. |
| `https://changeangel.io/` | Returns 200 but the site shows a "Coming Back" placeholder — the exchange is temporarily paused, not broken. |
| `https://www.digifinex.com/en-ww/trade/BTC/RVN` | Returns 451 (geo-blocked) from every location tested, so the current locale prefix could not be confirmed. Changing it would be a guess. |
| `https://support.ledger.com/hc/en-us/articles/360002731113-Update-Ledger-Nano-S-firmware` | Returns 200. Documents the older Ledger Nano S; keep until the current device doc is known. |
| `plus.google.com/...` (`_includes/social.html`) | Google+ is shut down, but the block is conditional on `site.googleplus_username`, which is not set in `_config.yml`, so it is never rendered. |
| Trailing-slash differences (`/halving`, `/wallet`, `/community`, `/devupdates`) | GitHub Pages redirects these automatically; they resolve. |
| `https://travis-ci.org/RavenProject/Ravencoin.svg?branch=master` (`README.md`) | Travis CI is shut down, but `RavenProject/Ravencoin` has only one workflow (`build-raven.yml`), which triggers on `release*` pushes and PRs and has a single historical run with an `action_required` conclusion. Its badge renders "no status" permanently, so it is not a valid replacement yet. Keep the badge until the Ravencoin repository has a workflow that runs on `master`. |
| `https://www.kisspng.com/png-watercolor-crow-6713/` (`_pages/attribution.md`) | The line is inside an HTML comment and is not rendered; the crow image is not used anywhere in the repository, and no valid replacement image page exists on the new host (cleanpng.com). Left as-is. |

Note on verification: `https://www.ravennodes.com/` and `https://www.rvnft.art/` both responded normally
during the audit (302 -> 200 and 200 respectively), but stopped answering HTTPS requests from the
verification host afterwards — TCP on port 443 still succeeds, so this looks like server-side
throttling of the auditing IP rather than a broken link. Both URLs are the canonical addresses of
their services and were kept.

## 3. Removed services with no working replacement

| Service | What was removed | Note |
|---|---|---|
| Testnet faucet (`https://testnet.ravencoin.network/`) | The "Testnet Faucet" entry in the WCC reference list and the "How to use the faucet?" section in `_pages/wcc.md` | The faucet host no longer resolves in DNS and no live replacement faucet was found (`faucet.ravencoin.org` and `testnet.ravencoin.org` do not resolve either). The remaining testnet links (API and block explorer at `testnet-api.ravencoin.org`) still work. If a current faucet exists, its URL can be added back here. |
| Raven Trader (`https://raventrader.net/`) | The "Find RVNFT NFTs on Raven Trader" social link in `_data/projects.yml` and the Raven Trader sentence in `_pages/assets.md` | DNS NXDOMAIN — service shut down. The RIP-15 standard reference was kept and now describes the process directly. |

## 4. Unused image assets

Removing a listing leaves its logo file behind. These files are no longer referenced by any data
file or page and can be deleted whenever convenient:

- `assets/img/pages/exchanges/bitni.png`
- `assets/img/pages/exchanges/UnicornX.png`
- `assets/img/pages/wallet/simplehold.png`
