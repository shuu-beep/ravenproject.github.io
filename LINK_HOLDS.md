# Deferred Link Items

Items identified by the ravencoin.org link audit (1,214 link instances across 91 files) and by the
exchange and new-service verification pass that followed it, which were **not** changed in this work.
They are kept as-is deliberately and are listed here so they can be revisited later.

## 0. Exchange list accounting

The exchange list was checked three times against the official APIs, the exchange websites and the
CoinGecko RVN ticker list. These are the actual counts, item by item:

| Stage | Exchanges |
|---|---|
| `af93e6a` (before this work) | 38 |
| `2a7ce6d` (link cleanup) | 36 — removed Bitni and UnicornX, renamed OKEx to OKX |
| `7c6adb9` (exchange verification) | 35 — removed Bitvavo, DEX Trade and Upbit, added MEXC and Pionex |
| current branch | 48 — added 13 more venues that were confirmed to trade RVN spot |

Across the whole branch 5 exchanges were removed and 15 were added, so the list went from 38 to 48.
24 exchanges are still on hold because their RVN support could not be confirmed: 19 in section 4 and
5 in section 5.

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
| `https://www.digifinex.com/en-ww/trade/BTC/RVN` | Returns 451 (geo-blocked) from every location tested, so the current locale prefix could not be confirmed. The exchange itself still lists an RVN/USDT market. |
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
| Bitvavo | `_data/exchanges.yml` entry | Officially delisted RVN on 18 September 2026 (deposits and withdrawals closed, balances converted to EUR). Their own API reports the asset with `depositStatus: DELISTED` and `withdrawalStatus: DELISTED`. |
| Upbit | `_data/exchanges.yml` entry | Officially delisted all RVN pairs (RVN/KRW, RVN/BTC, RVN/USDT) on 12 October 2026. Their market list no longer contains any RVN market and `api.upbit.com/v1/orderbook?markets=RVN-KRW` returns 404. |
| DEX Trade | `_data/exchanges.yml` entry | Their public symbol list contains 33 active pairs and none of them is RVN, so there is no RVN market to link to. |

## 4. Exchanges whose RVN support could not be confirmed

These entries are still listed, because RVN support could not be decided either way from the
verification host. They should be re-checked from an unblocked network before anything is removed.

| Exchange | Why it is unconfirmed |
|---|---|
| ATAIX | The site is a single page app and every path returns the same shell; the public API path that lists symbols no longer exists, and no RVN market appears in the CoinGecko ticker list. |
| BC Bitcoin (now BCXPro Malta) | `bcbitcoin.co.uk` redirects to `bcxpro.io`, whose market pages are client side rendered; no RVN market could be confirmed. |
| Bitladon | Every request is answered with a Cloudflare challenge (403). |
| Bittrex | `api.bittrex.com` does not resolve from the verification host and no RVN market appears in the CoinGecko ticker list. |
| ChangeHero | Every request is answered with a Cloudflare challenge (403). |
| CoinSwitch | The coin list is only available through an API that could not be reached, and the pages are client side rendered. |
| CoinoSwap | The coin list is only available through an API that could not be reached, and the pages are client side rendered. |
| Evorese | The market pages are client side rendered and no public API is exposed; the old `exchange.evorese.com/market/...` path returns 404. |
| Exolix | The public API paths return 404 or a Cloudflare challenge; only the exchange page loads. |
| Graviex | The site returns 522 (also listed in section 1) and the API returns 403. |
| NOWPayments | This is a payment gateway rather than an exchange, and the coin list is not reachable. |
| ProBit | Every API request is answered with a Cloudflare challenge (403); no RVN market appears in the CoinGecko ticker list. |
| Qortal | Qortal runs its own chain and token, and no RVN market was found on its market page. |
| SimpleSwap | The coin list is only available through an API that could not be reached, and the pages are client side rendered. |
| StealthEX | The coin list is only available through an API that could not be reached, and the pages are client side rendered. |
| SwapSpace | Every request is answered with a Cloudflare challenge (403). |
| SwapSwop | Every request is answered with a Cloudflare challenge (403). |
| Swapzone | The coin list is only available through an API that could not be reached, and the pages are client side rendered. |
| tZero | The market pages are client side rendered; no RVN market was confirmed and none appears in the CoinGecko ticker list. |

19 exchanges are listed in this section. None of them was removed, because their RVN support could
not be decided either way from the verification host.

## 5. RVN venues found during the venue sweep

17 venues were confirmed by the CoinGecko RVN ticker list as trading RVN spot. 13 of them could
also be confirmed on their own site or public API and are now listed on the buying guide. The
exchange card falls back to the exchange name when there is no banner logo, so a missing logo is no
longer a reason to leave a working venue out.

| Venue | RVN market | Evidence |
|---|---|---|
| Binance US | `https://www.binance.us/trade/pro/RVN_USDT` | `api.binance.us/api/v3/exchangeInfo?symbol=RVNUSDT` returns the symbol |
| Bittime | `https://www.bittime.com/en/trade/rvn-idr` | market page returns the RVN/IDR pair |
| BYDFi | `https://www.bydfi.com/exchange/RVN_USDT` | market page title "Trade RVN/USDT RavenCoin on BYDFi Exchange" |
| Bybit | `https://www.bybit.com/trade/spot/RVN/USDT` | `api.bybit.com/v5/market/instruments-info` returns RVNUSDT |
| Crypto.com Exchange | `https://crypto.com/exchange/trade/spot/RVN_USD` | `api.crypto.com/v2/public/get-ticker?instrument_name=RVN_USD` returns the ticker |
| Gate | `https://www.gate.com/trade/RVN_USDT` | `api.gateio.ws/api/v4/spot/currency_pairs` lists RVN_USDT |
| LATOKEN | `https://latoken.com/exchange/USDT-RVN` | `api.latoken.com/v2/ticker?symbol=RVN/USDT` returns the ticker |
| LBank | `https://www.lbank.com/trade/rvn_usdt` | `api.lbkex.com/v2/currencyPairs.do` lists rvn_usdt |
| TokoCrypto | `https://www.tokocrypto.com/trade/RVNUSDT` | market page returns the RVNUSDT pair |
| WEEX | `https://www.weex.com/spot/RVN-USDT` | market page title "Trade RVN/USDT spot" |
| WhiteBIT | `https://whitebit.com/trade/RVN_USDT` | `whitebit.com/api/v4/public/ticker` lists RVN_USDT |
| XT.COM | `https://www.xt.com/en/trade/rvn_usdt` | market page title "RVN USDT | RVNUSDT Charts & Trade Info" |
| ZebPay | `https://www.zebpay.com/exchange/RVN-INR` | market page returns the RVN-INR pair |

The remaining 5 venues could not be confirmed on their own site and stay unlisted:

| Venue | Why it is still on hold |
|---|---|
| CoinUp.io | Every request to the site and to `api.coinup.io` is answered with a Cloudflare challenge (403), so the market could not be confirmed first party. |
| Mudrex | The site and `api.mudrex.com` are not reachable from the verification host. |
| Nonkyc.io | The site and the public API are not reachable from the verification host. |
| CoinW | `www.coinw.com` does not respond from the verification host. |
| SafeTrade | The site and `api.safetrade.com` are answered with a Cloudflare challenge (403). |

Binance delisted RVN for residents of Brazil from 27 October 2026. The global Binance markets are
still live and were kept.

## 6. Unused image assets

Removing a listing leaves its logo file behind. These files are no longer referenced by any data
file or page and can be deleted whenever convenient:

- `assets/img/pages/exchanges/bitni.png`
- `assets/img/pages/exchanges/UnicornX.png`
- `assets/img/pages/exchanges/bitvavo.png`
- `assets/img/pages/exchanges/dextrade.png`
- `assets/img/pages/exchanges/upbit.png`
- `assets/img/pages/wallet/simplehold.png`
