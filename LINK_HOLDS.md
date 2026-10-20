# Deferred Link Items

Items identified by the ravencoin.org link audit (1,214 link instances across 91 files) and by the
exchange verification passes that followed it, which were **not** changed in this work. They are kept
as-is deliberately and are listed here so they can be revisited later.

## 0. Exchange list accounting

The exchange list was checked against the official exchange APIs, the exchange websites, the
CoinGecko RVN ticker list and the CoinMarketCap market pairs endpoint. These are the actual counts,
item by item:

| Stage | Exchanges |
|---|---|
| `af93e6a` (before this work) | 38 |
| `2a7ce6d` (link cleanup) | 36 — removed Bitni and UnicornX, renamed OKEx to OKX |
| `7c6adb9` (exchange verification) | 35 — removed Bitvavo, DEX Trade and Upbit, added MEXC and Pionex |
| `5c08b79` (venue sweep) | 48 — added 13 venues that were confirmed to trade RVN spot |
| current branch | **32** — 27 spot exchanges and 5 instant swap services |

The last step removed 19 entries from the active list and added 3. None of the removals is a
statement that the venue closed down permanently: 6 were removed because RVN trading could not be
confirmed, and 13 were removed because they could not be verified from the audit host. All 19 are
listed in sections 3 and 4 with the reason and with what would have to change to add them back.

## 1. Server errors — service may only be temporarily unavailable

These URLs responded with 5xx / Cloudflare errors during the audit. A transient server error is not
evidence that a service is gone, so the links were left untouched. Re-check before changing.

| # | URL | Observed status | Location |
|---|---|---|---|
| E1 | `https://squawker.app` | 500 | `_data/projects.yml` |
| E2 | `https://rvnbay.com`, `https://rvnbay.com/category/rvnft/` | 525 (Cloudflare) | `_data/projects.yml`, `_pages/assets.md` |
| E3 | `https://graviex.net/markets/rvnbtc` | 522 | removed from `_data/exchanges.yml`, see section 4 |
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
| `https://www.digifinex.com/en-ww/trade/BTC/RVN` | Returns 451 (geo-blocked) from every location tested, so the current locale prefix could not be confirmed. The exchange itself still lists an RVN/USDT market and stays listed. |
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

## 3. Services that were removed because RVN trading could not be confirmed

These were removed from the buying guide. That is not a statement that the venue closed down, it
means RVN could not be traded there any more, or could not be verified at all.

| Service | What was removed | Note |
|---|---|---|
| Testnet faucet (`https://testnet.ravencoin.network/`) | The "Testnet Faucet" entry in the WCC reference list and the "How to use the faucet?" section in `_pages/wcc.md` | The faucet host no longer resolves in DNS and no live replacement faucet was found (`faucet.ravencoin.org` and `testnet.ravencoin.org` do not resolve either). The remaining testnet links (API and block explorer at `testnet-api.ravencoin.org`) still work. If a current faucet exists, its URL can be added back here. |
| Raven Trader (`https://raventrader.net/`) | The "Find RVNFT NFTs on Raven Trader" social link in `_data/projects.yml` and the Raven Trader sentence in `_pages/assets.md` | DNS NXDOMAIN — service shut down. The RIP-15 standard reference was kept and now describes the process directly. |
| Bitvavo | `_data/exchanges.yml` entry | Officially delisted RVN on 18 September 2026 (deposits and withdrawals closed, balances converted to EUR). Their own API reports the asset with `depositStatus: DELISTED` and `withdrawalStatus: DELISTED`. |
| Upbit | `_data/exchanges.yml` entry | Officially delisted all RVN pairs (RVN/KRW, RVN/BTC, RVN/USDT) on 12 October 2026. Their market list no longer contains any RVN market and `api.upbit.com/v1/orderbook?markets=RVN-KRW` returns 404. |
| DEX Trade | `_data/exchanges.yml` entry | Their public symbol list contains 33 active pairs and none of them is RVN, so there is no RVN market to link to. |
| ATAIX | `_data/exchanges.yml` entry | Their own price feed (`api.ataix.com/api/prices/`) still lists RVN/BTC, RVN/ETH and RVN/USDT, but every one of them has volume 0, a zero last price and no ask, and neither CoinGecko nor CoinMarketCap lists the venue for RVN. Add it back only if the RVN markets start trading again. |
| Qortal | `_data/exchanges.yml` entry | Qortal runs its own chain and its own token. No RVN market was found on its market page and neither aggregator lists RVN for it. |
| tZero | `_data/exchanges.yml` entry | No RVN mention anywhere on the site, the market page returns 404, and neither aggregator lists RVN for it. |
| FLASHIFT | `_data/exchanges.yml` entry | No RVN mention anywhere on the site, the market page returns 404, and neither aggregator lists RVN for it. |
| NOWPayments | `_data/exchanges.yml` entry | This is a payment gateway rather than an exchange, and no RVN market was found for it. |
| ChangeAngel | `_data/exchanges.yml` entry | The site answers with a "Coming Back" placeholder, so the exchange is currently paused and cannot be presented as an active venue. Add it back if the service is relaunched with RVN support. |

## 4. Exchanges removed from the active list because they could not be verified

RVN support could not be decided either way from the audit host, so these venues are no longer
presented as places to trade RVN. None of them is known to be closed down. They should be re-checked
from an unblocked network, and each one goes back into the buying guide as soon as an RVN market is
confirmed on the venue's own site or API.

| Exchange | Why it could not be verified | What would confirm it |
|---|---|---|
| BC Bitcoin (now BCXPro Malta) | `bcbitcoin.co.uk` redirects to `bcxpro.io`, whose market pages are client side rendered; no RVN market could be confirmed. | An RVN market on `bcxpro.io` |
| Bitladon | Every request is answered with a Cloudflare challenge (403). | An RVN rate on the Bitladon API |
| Bittrex | `api.bittrex.com` does not resolve from the verification host and no RVN market appears in either aggregator. | `BTC-RVN` in the Bittrex market list |
| ChangeHero | Every request is answered with a Cloudflare challenge (403). | RVN in the ChangeHero currency list |
| CoinSwitch | The coin list is only available through an API that could not be reached, and the pages are client side rendered. | RVN in the CoinSwitch currency list |
| CoinoSwap | The coin list is only available through an API that could not be reached, and the pages are client side rendered. | RVN in the CoinoSwap currency list |
| Evorese | The market pages are client side rendered and no public API is exposed; the old `exchange.evorese.com/market/...` path returns 404. | An RVN market on `evorese.com` |
| Graviex | The site returns 522 (also listed in section 1) and the API returns 403. | An RVN market on `graviex.net` |
| ProBit | Every API request is answered with a Cloudflare challenge (403); no RVN market appears in either aggregator. | RVNUSDT in the ProBit market list |
| SimpleSwap | The coin list is only available through an API that could not be reached, and the pages are client side rendered. | RVN in the SimpleSwap currency list |
| StealthEX | The coin list is only available through an API that could not be reached, and the pages are client side rendered. | RVN in the StealthEX currency list |
| SwapSpace | Every request is answered with a Cloudflare challenge (403). | RVN in the SwapSpace currency list |
| SwapSwop | Every request is answered with a Cloudflare challenge (403). | RVN in the SwapSwop currency list |

## 5. RVN venues found during the venue sweep

The CoinGecko RVN ticker list and the CoinMarketCap market pairs endpoint together cover a much
wider set of venues than the buying guide listed. Each candidate was checked on the venue's own
site or public API before it was added.

| Venue | RVN market | Evidence | Result |
|---|---|---|---|
| Binance US | `https://www.binance.us/trade/pro/RVN_USDT` | `api.binance.us/api/v3/exchangeInfo?symbol=RVNUSDT` returns the symbol | listed |
| Bittime | `https://www.bittime.com/en/trade/rvn-idr` | market page returns the RVN/IDR pair | listed |
| BYDFi | `https://www.bydfi.com/exchange/RVN_USDT` | market page title "Trade RVN/USDT RavenCoin on BYDFi Exchange" | listed |
| Bybit | `https://www.bybit.com/trade/spot/RVN/USDT` | `api.bybit.com/v5/market/instruments-info` returns RVNUSDT | listed |
| Crypto.com Exchange | `https://crypto.com/exchange/trade/spot/RVN_USD` | `api.crypto.com/v2/public/get-ticker?instrument_name=RVN_USD` returns the ticker | listed |
| Gate | `https://www.gate.com/trade/RVN_USDT` | `api.gateio.ws/api/v4/spot/currency_pairs` lists RVN_USDT | listed |
| LATOKEN | `https://latoken.com/exchange/USDT-RVN` | `api.latoken.com/v2/ticker?symbol=RVN/USDT` returns the ticker | listed |
| LBank | `https://www.lbank.com/trade/rvn_usdt` | `api.lbkex.com/v2/currencyPairs.do` lists rvn_usdt | listed |
| TokoCrypto | `https://www.tokocrypto.com/trade/RVNUSDT` | market page returns the RVNUSDT pair | listed |
| WEEX | `https://www.weex.com/spot/RVN-USDT` | market page title "Trade RVN/USDT spot" | listed |
| WhiteBIT | `https://whitebit.com/trade/RVN_USDT` | `whitebit.com/api/v4/public/ticker` lists RVN_USDT | listed |
| XT.COM | `https://www.xt.com/en/trade/rvn_usdt` | market page title "RVN USDT | RVNUSDT Charts & Trade Info" | listed |
| ZebPay | `https://www.zebpay.com/exchange/RVN-INR` | market page returns the RVN-INR pair | listed |
| XXKK | `https://www.xxkk.com/en/spot/RVN_USDT` | market page returns the RVN_USDT pair, CoinMarketCap volume about 66k USD | listed |
| Binance TR | `https://www.binance.tr/en/trade/RVN_TRY` | market page payload carries `"symbol":"RVNTRY"` with base asset Ravencoin quoted in Turkish lira, CoinMarketCap volume about 426k USD | listed |
| CoinDCX | `https://coindcx.com/trade/RVNUSDT` | `api.coindcx.com/exchange/ticker` returns RVNUSDT and RVNINR with a fresh volume | listed |
| CoinUp.io | `https://www.coinup.io/en_US/trade/RVN_USDT` | Every request to the site and to `api.coinup.io` is answered with a Cloudflare challenge (403). CoinGecko and CoinMarketCap both list RVN/USDT there, with CoinMarketCap reporting about 1.78M USD of volume. | on hold |
| CoinW | `https://www.coinw.com/spot/rvnusdt` | `www.coinw.com` does not respond from the verification host. Both aggregators list RVN/USDT there, with CoinMarketCap reporting about 308k USD of volume. | on hold |
| Mudrex | `https://mudrex.com/` | The site and `api.mudrex.com` are not reachable from the verification host. CoinGecko lists RVN/USDT with about 53k USD of volume. | on hold |
| SafeTrade | `https://safetrade.com/exchange/RVN-BTC` | The site and `api.safetrade.com` are answered with a Cloudflare challenge (403). CoinGecko lists RVN/BTC with about 20k USD of volume and flags it as an anomaly. | on hold |
| Nonkyc.io | `https://nonkyc.io/market/RVN_USDT` | The site and the public API are not reachable from the verification host. Both aggregators list RVN/USDT, with a very small reported volume. | on hold |

Venues that were checked and deliberately not added:

| Venue | Why it is not listed |
|---|---|
| BingX | The official symbol list contains RVN-USDT, but the entry has `status: 0` and both the 24 hour ticker and the order book answer "symbol is not found", so the market is not active. CoinMarketCap only lists the perpetual contract. |
| Bitget | The official spot API has 3,601 symbols and none of them is a plain RVN pair — only RVN derived tokens such as RVNT and RVNO. CoinMarketCap only lists the perpetual contract. |
| KuCoin | The official symbol list has 968 spot symbols and no RVN. CoinMarketCap only lists the perpetual contract. |
| HitBTC | The official ticker for RVNUSDT answers with `volume: 0` and a stale price, and CoinMarketCap reports zero volume. |
| Giottus | The market page carries an RVN/USDT pair, but CoinMarketCap reports zero volume. |
| StakeCube, KTX, Dzengi, CRMClick, CriptoSwaps, Picol | Either report zero or negligible volume, or no RVN market could be found on the venue. |

Binance delisted RVN for residents of Brazil from 27 October 2026. The global Binance markets are
still live and were kept.

## 6. Unused image assets

Removing a listing leaves its logo file behind. These files are no longer referenced by any data
file or page and can be deleted whenever convenient:

- `assets/img/pages/exchanges/ataix.png`
- `assets/img/pages/exchanges/bcbitcoin.png`
- `assets/img/pages/exchanges/bitladon.png`
- `assets/img/pages/exchanges/bittrex.png`
- `assets/img/pages/exchanges/bitni.png`
- `assets/img/pages/exchanges/bitvavo.png`
- `assets/img/pages/exchanges/changeangel.png`
- `assets/img/pages/exchanges/changehero.png`
- `assets/img/pages/exchanges/coinoswap.png`
- `assets/img/pages/exchanges/coinswitch.png`
- `assets/img/pages/exchanges/dextrade.png`
- `assets/img/pages/exchanges/evorese.png`
- `assets/img/pages/exchanges/flashift.png`
- `assets/img/pages/exchanges/graviex.png`
- `assets/img/pages/exchanges/nowpayments.png`
- `assets/img/pages/exchanges/probit.png`
- `assets/img/pages/exchanges/qortal.png`
- `assets/img/pages/exchanges/simpleSwap.png`
- `assets/img/pages/exchanges/stealthex.png`
- `assets/img/pages/exchanges/swapspace.png`
- `assets/img/pages/exchanges/swapswop.png`
- `assets/img/pages/exchanges/tzero.png`
- `assets/img/pages/exchanges/UnicornX.png`
- `assets/img/pages/exchanges/upbit.png`
- `assets/img/pages/wallet/simplehold.png`

The following three were already unused before this work and are only listed for completeness:
`assets/img/pages/exchanges/coinexchange.png`, `assets/img/pages/wallet/flare.png`,
`assets/img/pages/wallet/spend.png`.
