# PR Handoff — Ravencoin.org website link and service maintenance

This document is written for an independent reviewer who has **no context from the earlier
conversation**. Every statement below was taken from the repository itself (git history and the
current file contents) or from a command that can be re-run. Anything that could not be verified
is marked as such.

The deferred / hold documentation is kept in [`LINK_HOLDS.md`](LINK_HOLDS.md). That file was not
reproduced here — read it directly.

---

## A. Repository information

| Item | Value |
|---|---|
| Upstream repository | `https://github.com/RavenProject/ravenproject.github.io` (remote name `upstream`) |
| Fork used for the PR | `https://github.com/shuu-beep/ravenproject.github.io` (remote name `origin`) |
| Local clone | `/Users/jeongho/projects/ravenproject.github.io` |
| Working branch | `fix/website-broken-links` |
| Base commit (upstream `master` / `origin/master`) | `af93e6a092f551dc1e373d95f1ab0e8c10101eb3` — "Merge pull request #288 from m4r1m0/master" |
| Current HEAD | `bbb1cdbf54e1ab00e73f1cc2a17f31e9c6239f1b` — "Rework the buying guide around verified RVN spot markets" |
| Pushed to the fork? | **No.** `git ls-remote --heads origin fix/website-broken-links` returns nothing; the branch exists only locally. |
| PR created? | **No.** No pull request exists for this branch yet. |

The branch is 4 commits ahead of `af93e6a` and contains no merge commits.

### Local preview

The preview server reproduces the GitHub Pages plugin set as closely as the local Ruby allows
(see section D for the limitation).

```bash
export GEM_HOME=/tmp/raven-gems GEM_PATH=/tmp/raven-gems
export JEKYLL_ENV=production
cd /tmp/raven-preview            # a directory WITHOUT the repo Gemfile, otherwise Bundler takes over
/tmp/raven-gems/bin/jekyll serve \
  --source ~/projects/ravenproject.github.io \
  --destination /tmp/raven-preview/site \
  --config ~/projects/ravenproject.github.io/_config.yml,/tmp/raven-preview/gh-pages.yml \
  --host 0.0.0.0 --port 4000 --force_polling
```

* URL: <http://127.0.0.1:4000/> (also reachable on the LAN at `http://192.168.35.191:4000/`)
* The override config `/tmp/raven-preview/gh-pages.yml` lives **outside** the repository and only
  enables the GitHub Pages plugin set (`jekyll-sitemap`, `jekyll-optional-front-matter`,
  `jekyll-readme-index`, `jekyll-default-layout`, `jekyll-titles-from-headings`,
  `jekyll-relative-links`, `jekyll-commonmark-ghpages`). The repository `_config.yml` is untouched.
* The preview server was running with this exact command when this document was written.
* Do **not** use `--incremental`: with it, a change to a `_data/*.yml` file does not always
  regenerate the pages that consume it, and the preview silently serves stale HTML.

---

## B. All changes (base `af93e6a` → HEAD `bbb1cdb`)

Overall: **61 files changed, 497 insertions(+), 328 deletions(-)** (57 modified, 4 added).

### B.1 The four commits

| Commit | Files | Subject | What it did |
|---|---|---|---|
| `2a7ce6d` | 57 | Fix broken links across pages, data files and mobile wallet support | Replaced dead/outdated URLs with verified working ones and removed listings for services that no longer exist. Touched `_pages/`, `_data/`, `_includes/footer.html`, `README.md`, `CODE_OF_CONDUCT.md` and 45 `mobilewallet/support/**` HTML files. Created `LINK_HOLDS.md`. |
| `7c6adb9` | 10 | Add verified RVN services and drop delisted exchanges | Removed three exchanges whose RVN listing ended, added MEXC and Pionex, added RavenVault/Corepool and six community projects, added the three new logo assets, and added a second Electrum download button. |
| `5c08b79` | 3 | List 13 more RVN venues that trade RVN spot | Added 13 exchanges that were previously held back only because no 450×150 banner logo could be retrieved; introduced the logo-less text card in the template. |
| `bbb1cdb` | 4 | Rework the buying guide around verified RVN spot markets | Applied the full exchange audit: removed 19 unconfirmed entries, added 3 confirmed ones, split the buying guide into three sections, added the CoinGecko/CoinMarketCap market information links and the ChainStrap project. |

### B.2 Files added

| File | Purpose |
|---|---|
| `LINK_HOLDS.md` | Every deferred item: server errors, links intentionally left alone, removed services, the 19 exchanges removed from the active list, the venues still on hold and the venues checked and deliberately not added. |
| `assets/img/pages/exchanges/mexc.png` | Official MEXC brand banner, cropped to the site convention (450×150). |
| `assets/img/pages/exchanges/pionex.png` | Official Pionex brand banner, cropped to the site convention (450×150). |
| `assets/img/pages/wallet/ravenvault.png` | Official RavenVault app icon, 450×450 (the site already had one square wallet logo, `mango-farm-logo.png`). |

### B.3 Files modified (57)

Root / docs:
`README.md`, `CODE_OF_CONDUCT.md`, `LINK_HOLDS.md`

Data:
`_data/exchanges.yml`, `_data/wallets.yml`, `_data/pools.yml`, `_data/projects.yml`

Pages / includes:
`_pages/wallet.md`, `_pages/buy-ravencoin.md`, `_pages/community.md`, `_pages/wcc.md`,
`_pages/assets.md`, `_includes/footer.html`

Mobile wallet support (45 files, all under `mobilewallet/support/`):
`index.html`, `details.html`, plus every HTML file in `FAQ/`, `Troubleshooting/`,
`app-settings/`, `cryptocurrency-basics/`, `miscellaneous/`, `security/`, `send-receive/`.

### B.4 Removals and why

| Removed | Where | Reason |
|---|---|---|
| Bitni | `_data/exchanges.yml` | `bitni.com` expired (redirects to an expired-domain parking page). |
| UnicornX | `_data/exchanges.yml` | `unicornx.exchange` domain expired, same parking page. |
| SimpleHold | `_data/wallets.yml` | Service discontinued; the domain answers with Cloudflare error 1000 ("DNS points to prohibited IP"). |
| RavencoinHelper OS | `_data/projects.yml` | `ravencoinhelper.com` no longer resolves in DNS. |
| Raven Trader social link | `_data/projects.yml` (RVNFT entry) | `raventrader.net` no longer resolves in DNS. |
| Testnet faucet entry and "How to use the faucet?" section | `_pages/wcc.md` | `testnet.ravencoin.network` no longer resolves; no replacement faucet was found. |
| GetRavencoin.org card | `_pages/community.md` | `getravencoin.org` has no A record. |
| Raven Trader sentence | `_pages/assets.md` | `raventrader.net` dead. The RIP-15 reference was kept and now describes the process directly. |
| App store / Play store badges for the official mobile wallet | `_pages/wallet.md` | Both official apps are retired: the iTunes lookup returns zero results and the Google Play URL 404s. |
| 5 pool entries (Bsod, Mole-Pool, Skypool, CryptoRUSH, Kaww.io) | `_data/pools.yml` | Domains dead or unusable (`bsod.pw` fails DNS resolution on every resolver tried). |
| 19 exchanges from the active buying guide | `_data/exchanges.yml` | See section C.3 — the reason for each is in `LINK_HOLDS.md` sections 3 and 4. |

### B.5 Additions and why

| Added | Category | Evidence used |
|---|---|---|
| MEXC (`https://www.mexc.com/exchange/RVN_USDT`) | exchange | `api.mexc.com/api/v3/exchangeInfo` lists `RVNUSDT` and `RVNUSDC`. |
| Pionex (`https://www.pionex.com/trade/RVN_USDT`) | exchange | Market page renders the RVN/USDT pair. |
| Binance US, Bittime, BYDFi, Bybit, Crypto.com Exchange, Gate, LATOKEN, LBank, TokoCrypto, WEEX, WhiteBIT, XT.COM, ZebPay | exchange (13) | Each confirmed on the venue's own site or public API; see `LINK_HOLDS.md` section 5 for the exact endpoint. |
| Binance TR (`https://www.binance.tr/en/trade/RVN_TRY`) | exchange | Market page payload carries `"symbol":"RVNTRY"`, base asset Ravencoin, quote Turkish lira. |
| CoinDCX (`https://coindcx.com/trade/RVNUSDT`) | exchange | `api.coindcx.com/exchange/ticker` returns RVNUSDT and RVNINR with a current timestamp and non-zero volume. |
| XXKK (`https://www.xxkk.com/en/spot/RVN_USDT`) | exchange | Market page returns the RVN_USDT pair. |
| RavenVault (`https://ravenvault.ex.erci.se/en/`) | wallet | Non-custodial RVN wallet: web wallet, desktop client that runs Ravencoin Core, Android app on Google Play, install guide page. Site states it is not affiliated with the Ravencoin project. |
| Corepool (`https://corepool.cloud/`) | mining pool | KAWPOW pool with live height, hashrate and payout information. |
| KawTrace | project | Browser based RVN block / transaction / address / asset explorer; its public RPC returned mainnet block height 4,589,723. Release is a beta (`v0.1.0-beta.3`) and is described as such. |
| Raven Network Nodes | project | Node distribution dashboard that measures the P2P network with its own crawler; `data/latest.json` and `data/history.json` return live measurement data. |
| RavenTag | project | Links NTAG 424 DNA NFC chips to Ravencoin asset tokens, protocol published as RTP-1, Android apps, public source. |
| Blackwing | project | Non-custodial multi-chain (Ravencoin and Kaspa) asset platform. The description states it is in public beta and needs a self-hosted node. |
| Ravencoin Node Monitor | project | Open source dashboard for a self-hosted node. The description states explicitly that the hosted page is a public demo whose simulated values are not live network statistics. |
| Ravencoin SDK | project | Developer toolkit with unit, integration and live tests plus a CI workflow. The description states it is a beta distributed as source rather than as a published package. |
| ChainStrap | project | See section B.9. |
| CoinGecko / CoinMarketCap | market information links | See section B.10. |
| Electrum maintained forks | project (Electrum Ravencoin entry) | `Electrum-RVN-SIG/electrum-ravencoin` and `.../electrumx-ravencoin` are marked **archived** on GitHub. `ALENOC/electrum-ravencoin` (v1.3.2, Sept 2026) and `ALENOC/electrumx-ravencoin` (v1.13.13) are the active forks, and their release pages are linked as well. |

### B.6 Existing links that were corrected (before → after)

| File | Before | After |
|---|---|---|
| `_pages/wallet.md` | `.../releases/download/v4.6.2snapshot/raven-4.7.0-0b91b62a2-win64-setup-unsigned.exe` | `.../download/v4.7.0/raven-4.7.0-0b91b62a2-win64-setup-unsigned.exe` |
| `_pages/wallet.md` | `.../v4.6.2snapshot/raven-4.7.0-0b91b62a2-osx-unsigned.dmg` | `.../v4.7.0/raven-4.7.0-0b91b62a2-osx-unsigned.dmg` |
| `_pages/wallet.md` | `.../v4.6.2snapshot/raven-4.7.0-0b91b62a2-x86_64-linux-gnu.tar.gz` | `.../v4.7.0/raven-4.7.0-0b91b62a2-x86_64-linux-gnu.tar.gz` |
| `_data/exchanges.yml` (BC Bitcoin) | `https://www.bcbitcoin.co.uk/buy/?coin=RVN` | `https://www.bcbitcoin.co.uk/` (entry later removed, see C.3) |
| `_data/exchanges.yml` (Evorese) | `https://exchange.evorese.com/market/BIRAKE.RVN_BIRAKE.BTC` | `https://www.evorese.com/` (entry later removed) |
| `_data/exchanges.yml` (ProBit) | `https://www.probit.com/app/exchange/RVN-USDT` | `https://www.probit.com/exchange/RVN-USDT` (entry later removed) |
| `_data/exchanges.yml` (OKEx → OKX) | `https://www.okex.com/en/markets/spot/rvn_btc` | `https://www.okx.com/en/trade-spot/rvn-usdt` (name changed to OKX; the `rvn-btc` pair does not exist on OKX — the live pairs are RVN-EUR, RVN-USDC and RVN-USDT) |
| `_data/wallets.yml` (Coinpayments) | `https://www.coinpayments.net/apps` | `https://www.coinpayments.net/` |
| `_data/wallets.yml` (Crypto.com) | `https://www.crypto.com/en/index.html` | `https://www.crypto.com/en/` |
| `_data/wallets.yml` (Exodus) | `https://www.exodus.io/download/` | `https://www.exodus.com/download/` |
| `_data/wallets.yml` (tZERO) | `https://www.tzero.com/crypto` | `https://www.tzero.com/` |
| `_data/projects.yml` (RavenTrader Server) | `https://github.com/ben-abraham/raven-trader` | `https://github.com/ben-abraham/raven-trader-server` |
| `_data/projects.yml` (RVNFT) | `https://www.rvnft.art/nft-marketplace/`, `https://www.rvnft.art/rvn-nft-gallery/` | `https://www.rvnft.art/` (both deep paths returned 404) |
| `_data/projects.yml` (SYNTH) | `https://gradesynth.com/ipfs/QmPJeUzChXMSfe3uoXxLTqWP2MR8u1CdFuurUZXRvjed9U/` | `https://gradesynth.com/` (the IPFS gateway path 404s) |
| `_data/projects.yml` (NOWNodes) | `http://bit.do/nownodes-setup` | `https://nownodes.io/` |
| `_data/projects.yml` (NOWNodes) | `https://documenter.getpostman.com/view/13630829/TVmFkLwy` | `https://docs.nownodes.io/rvn/` |
| `_includes/footer.html` | `http://www.ravennodes.com/` | `https://www.ravennodes.com/` |
| `_includes/footer.html` | `https://discordapp.com/invite/jn6uhur` | `https://discord.com/invite/jn6uhur` |
| `_includes/footer.html` | `http://opensource.org/licenses/mit-license.php` | `https://opensource.org/license/mit` |
| `_includes/footer.html` | `href="{{ " / " | relative_url }}"` (rendered as `/%20/`) | `href="{{ "/" | relative_url }}"` |
| `_pages/community.md` | `http://www.ravennodes.com/` | `https://www.ravennodes.com/` |
| `_pages/assets.md` | `https://www.okex.com` + " OKEX" | `https://www.okx.com` + " OKX" |
| `README.md` | `https://github.com/RavenProject/ravenproject.github.io.org/issues/new` | `https://github.com/RavenProject/ravenproject.github.io/issues/new` |
| `README.md` | `https://github.com/RavenProject/ravenproject.github.io.org/compare` | `https://github.com/RavenProject/ravenproject.github.io/compare` |
| `README.md` | `https://help.github.com/articles/setting-up-your-github-pages-site-locally-with-jekyll/` | `https://docs.github.com/en/pages/setting-up-a-github-pages-site-with-jekyll/testing-your-github-pages-site-locally-with-jekyll` |
| `README.md` | `https://`username`.github.io` (backticks broke the URL) | `https://username.github.io` |
| `CODE_OF_CONDUCT.md` | `http://contributor-covenant.org/version/1/4/` and `http://contributor-covenant.org` | `https://www.contributor-covenant.org/version/1/4/` and `https://www.contributor-covenant.org` — the document text still says "version 1.4", so the link was kept on the same version rather than jumped to 2.1. |

### B.7 Changes per category

* **Wallets (`_data/wallets.yml`, 22 entries)** — 4 links fixed, SimpleHold removed, RavenVault added.
* **Mining pools (`_data/pools.yml`, 17 entries)** — 5 dead pools removed, Corepool added.
* **Projects (`_data/projects.yml`, 16 entries)** — RavencoinHelper OS removed; RavenTrader server link fixed; RVNFT deep paths fixed; SYNTH IPFS path fixed; NOWNodes links fixed; Electrum maintained forks added; ChainStrap, KawTrace, Raven Network Nodes, RavenTag, Blackwing, Ravencoin Node Monitor and Ravencoin SDK added.
* **Community (`_pages/community.md`)** — GetRavencoin.org card removed; Ravencoin Node Map link switched to https.
* **Exchanges (`_data/exchanges.yml`, 32 entries)** — see section C.
* **Buying guide (`_pages/buy-ravencoin.md`)** — restructured into three sections (section B.11).
* **Wallet page (`_pages/wallet.md`)** — download links moved to the real `v4.7.0` release tag with matching asset names; retired app store badges removed; a second button for the maintained Electrum build added.
* **WCC page (`_pages/wcc.md`)** — dead testnet faucet entry and section removed.
* **Assets page (`_pages/assets.md`)** — dead Raven Trader sentence replaced by a direct RIP-15 description; OKEx → OKX.
* **Footer (`_includes/footer.html`)** — 3 links updated, schema.org `u-url` fixed.
* **README / CODE_OF_CONDUCT** — see B.6.

### B.8 Mobile wallet support fixes

The `mobilewallet/support/` tree is served as plain HTML under `/mobilewallet/support/`, so every
relative link is resolved against the file's own directory.

* **CSS paths (43 files, 81 references).** Every file in a sub-directory referenced
  `css/elements-style.css` (38 files) and `css/font-awesome.css` (43 files) although the stylesheet
  lives in `mobilewallet/support/css/`. Changed to `../css/...`. The 5 HTML files directly in
  `mobilewallet/support/` legitimately use `css/...` and were left alone.
* **`mobilewallet/support/index.html`** — removed `<script src="js/init.js"></script>`; no `js/`
  directory exists anywhere in `mobilewallet/`, so the tag could only ever 404.
* **`mobilewallet/support/details.html`** — document links that pointed at non-existent files in the
  same directory now point at the real pages:
  `recovery-phrase.html` → `security/recovery-phrase.html`;
  `receive-wallet.html` → `FAQ/receive-wallet.html` (receive question),
  `FAQ/raven-address-change.html` (address change question),
  `FAQ/ravencoin-transaction.html` (transaction cost question).

### B.9 ChainStrap

* Website: <https://chainstrap.com> — publishes compressed snapshots of raw chain data to IPFS so a
  new node can reach the current height quickly. `python3 getchain.py RVN` fetches the mainnet
  snapshot, `RVN --testnet` the testnet one.
* Source: <https://github.com/chainstrap/chainstrap.github.io> — the only public repository of the
  `chainstrap` organisation; it contains `getchain.py`, `savechain.py`, `chains.json`, the `RVN/`
  metadata folder and the site itself.
* Script: <https://chainstrap.com/getchain.py> — 21,668 bytes, HTTP 200.
* **No release is published** (the repository has zero releases and zero tags), so no release or
  version download link was created and no version number is hard-coded on the site.
* Live RVN snapshot metadata: `https://chainstrap.com/RVN/RVN-mainnet.json` reported block height
  4,588,724, updated 2026-10-20, at the time of checking.
* **Accuracy of the technical description.** The project description states that the snapshot only
  shortens the initial download and that Ravencoin Core still checks every block header, proof of
  work and transaction itself. This matches the project's own README, which says the data is still
  validated by the core clients and that a fully verifying client checks every transaction and
  signature including the merkle root in every block header. The wording deliberately does **not**
  claim that ChainStrap replaces block validation.
* Duplicate check: no existing entry in `_data/projects.yml` refers to ChainStrap.

### B.10 CoinGecko and CoinMarketCap

Added to the new **RVN Market Information** section of `_pages/buy-ravencoin.md` as plain links,
not as exchange cards:

| Link | Text |
|---|---|
| `https://www.coingecko.com/en/coins/ravencoin#markets` | "CoinGecko" — "Compare RVN spot markets, trading pairs, volume, spreads, and liquidity." |
| `https://coinmarketcap.com/currencies/ravencoin/#Markets` | "CoinMarketCap" — "Explore RVN markets, trading pairs, and reported trading volumes." |

### B.11 Buying guide structure

`_pages/buy-ravencoin.md` now has three sections, replacing the previous
"Non-Custodial exchanges" / "Custodial exchanges" split:

1. **RVN Spot Exchanges** — entries where `swap` is not `true` (27 cards).
2. **RVN Swap / Instant Exchange Services** — entries where `swap == true` (5 cards), with a note
   that these have no order book and set the price themselves.
3. **RVN Market Information** — the two aggregator links.

The card markup is unchanged apart from two additions: a `swap` filter, and a fallback that renders
the exchange name as text when an entry has no `logo` key. The card grid was given `w-full` so that
text cards and logo cards have the same width, and the text fallback has `min-height:112px` so both
card types have the same height. The `kyc` field is still present in the data but is no longer used
for the section split, because after the cleanup every remaining spot entry is custodial and every
remaining non-custodial entry is a swap service.

---

## C. Exchange verification

### C.1 Final spot exchanges (27)

Binance, Binance TR, Binance US, Bitrue, Bittime, Bybit, BYDFi, CoinDCX, CoinSpot,
Crypto.com Exchange, Digifinex, Digitalexchange.id, Gate, Huobi, Indodax, LATOKEN, LBank, MEXC, OKX,
Paribu, Pionex, TokoCrypto, WEEX, WhiteBIT, XT.COM, XXKK, ZebPay

### C.2 Instant swap services (5)

Changelly, ChangeNow, Exolix, Guardarian, Swapzone — all carry `swap: true`.

### C.3 Removed from the active list (19)

| Group | Entries | Reason |
|---|---|---|
| Could not be confirmed (5) | ATAIX, Qortal, tZero, FLASHIFT, NOWPayments | ATAIX still lists RVN/BTC, RVN/ETH and RVN/USDT in its own price feed but all three have volume 0, a zero last price and no ask, and neither aggregator lists the venue for RVN. Qortal runs its own chain and token and has no RVN market. tZero, FLASHIFT and NOWPayments have no RVN mention anywhere on their sites and no RVN market in either aggregator. |
| Service paused (1) | ChangeAngel | The site answers with a "Coming Back" placeholder. |
| Could not be verified from the audit host (13) | BC Bitcoin, Bitladon, Bittrex, ChangeHero, CoinoSwap, CoinSwitch, Evorese, Graviex, ProBit, SimpleSwap, StealthEX, SwapSpace, SwapSwop | Each is blocked by a Cloudflare challenge, has an unresolvable API host, or is a client-side rendered single page app whose coin list could not be read. **None of them is stated to be permanently delisted**; the exact re-verification condition for each is written in `LINK_HOLDS.md` section 4. |

### C.4 Added in this work (3)

XXKK, Binance TR, CoinDCX — see B.5 for the evidence.

### C.5 Still on hold, and the condition to re-add them

| Venue | Condition to list it |
|---|---|
| CoinUp.io | An RVN market confirmed on the venue's own site or API. The site and `api.coinup.io` currently answer with a Cloudflare challenge. Both aggregators list RVN/USDT there. |
| CoinW | The venue has to answer at all. `www.coinw.com` does not respond. Both aggregators list RVN/USDT there. |
| Mudrex | The site and `api.mudrex.com` have to respond. |
| SafeTrade | The site and `api.safetrade.com` have to respond. |
| Nonkyc.io | The site and the public API have to respond. |

### C.6 Spot versus derivatives — the rule that was applied

* Only a **spot** market counts as RVN trading. Perpetual / futures / leveraged tokens do not.
* The CoinMarketCap market pairs endpoint separates the two explicitly: it reports
  `category: "spot"` and `category: "perpetual"` per pair. Of the 78 RVN pairs it reports, 47 are
  spot and 31 are perpetual.
* CoinGecko's RVN ticker endpoint only reports spot markets.
* Concrete exclusions that followed from this rule:
  * **BingX** — the official symbol list contains `RVN-USDT`, but the entry has `"status": 0` and
    both the 24 hour ticker and the order book answer `symbol is not found`, so the market is not
    active. Not listed.
  * **Bitget** — the official spot API returns 3,601 symbols and none of them is a plain RVN pair;
    only RVN derived tokens such as `RVNTUSDT`, `RVNETUSDT`, `RVNOUSDT` exist. Not listed.
  * **KuCoin** — the official symbol list returns 968 spot symbols and no RVN. Not listed.
  * **HitBTC** — the official RVNUSDT ticker answers with `volume: 0` and a stale price. Not listed.
  * **Giottus, StakeCube, KTX, Dzengi, CRMClick, CriptoSwaps, Picol** — zero or negligible volume,
    or no RVN market found. Not listed.

### C.7 Sources used for verification

* Official exchange public APIs and market pages (see the per-venue endpoints in
  `LINK_HOLDS.md` section 5).
* CoinGecko: `https://api.coingecko.com/api/v3/coins/ravencoin/tickers` — 32 tickers across 27
  venues, pages 2 and 3 are empty. Note: the `trust_score` field came back `null` for every
  ticker, and CoinGecko flags `is_anomaly: true` for Binance, Paribu, Binance US, HTX, Indodax,
  ZebPay and SafeTrade. Its `volume` figures are one to two orders of magnitude above the
  CoinMarketCap ones, so **CoinMarketCap was used as the liquidity reference**.
* CoinMarketCap: `https://api.coinmarketcap.com/data-api/v3/cryptocurrency/market-pairs/latest?id=2577`
  — coin id 2577 is RVN. 78 pairs, 47 spot and 31 perpetual, across 38 spot venues.
* Cross-check result: 24 venues are listed by both, 3 only by CoinGecko (Bittime, Mudrex,
  SafeTrade) and 14 only by CoinMarketCap.

### C.8 Do the report numbers match the YAML?

Yes. The current `_data/exchanges.yml` holds 32 entries: 27 with `swap` unset (spot) and 5 with
`swap: true`. The names in sections C.1 and C.2 above were read directly out of the file, and the
19 names in C.3 were confirmed to be absent from it. A quick re-check:

```bash
cd ~/projects/ravenproject.github.io
ruby -ryaml -e 'd=YAML.safe_load(File.read("_data/exchanges.yml")); \
  puts "total=#{d.size} spot=#{d.count{|e| !e["swap"]}} swap=#{d.count{|e| e["swap"]}}"; \
  puts d.map{|e| e["name"]}.sort.join(", ")'
```

The earlier interim reports contained two counting errors, which are worth knowing about when
reading the history: the hold list was reported as 14 items when it held 19 distinct venues (two
table rows bundled eight names), and the venue sweep was reported as 12 candidates when it covered
17. `LINK_HOLDS.md` section 0 now carries the reconciled counts.

---

## D. Verification results

All of the following were run against the current `bbb1cdb` working tree.

| Check | Result |
|---|---|
| YAML parsing | `ruby -ryaml` parses all five `_data/*.yml` files: exchanges 32, platforms 4, pools 17, projects 16, wallets 22. |
| Duplicate entries | No duplicate `name` and no duplicate `url` inside any single data file, and none across the five files. |
| Deleted entries | All 19 names confirmed absent from `_data/exchanges.yml`. |
| Internal links | The build produces 73 HTML files; a scan of every root-relative `href`/`src` against the build output finds **0** unresolved paths. |
| Image assets | Every `logo:` value in the data files points at a file that exists (38 `logo:` references, 0 missing; 57 asset-style references including `url:` paths that start with `/assets/`, 0 missing). Every rendered `<img>` in the build resolves to a file in the output. |
| Jekyll build | Succeeds in ~0.25 s with no errors and no warnings, using the GitHub Pages plugin set from the override config. |
| External links | Checked individually with `curl` while the work was done, and the results are recorded per venue in `LINK_HOLDS.md`. This was **not** a full crawl of the live site. |
| Desktop rendering | Headless Chrome at 1440 px: the buying guide shows a three-column grid, logo cards and text cards at equal height (112 px), and all three sections render in order. |
| Mobile rendering | Headless Chrome at 390 px: cards stack full width. Measured `docScrollWidth = 375`, `cardW = 343`, `cardRight = 359`, logo rendered 295×98, text card 295×112 — identical to the pre-change values, so the change introduced no horizontal overflow. |

### Pages that were checked

`/` (home, including the announcement modal), `/buy-ravencoin/`, `/wallet/`, `/projects/`,
`/pools/`, `/community/`, `/mobilewallet/support/` and its `details.html`, `security/pin.html` and
`FAQ/receive-wallet.html` sub-pages, `/LINK_HOLDS.html`, `/sitemap.xml`.

### Known limitations and unresolved items

1. **`github-pages` gem could not be installed.** The system Ruby is 2.6.10 and the `github-pages`
   gem needs Ruby ≥ 3.0, so the preview uses plain Jekyll 3.9.5 plus the individual GitHub Pages
   plugins. `jekyll-github-metadata` is the one plugin that is missing (its `net-http` dependency
   requires Ruby ≥ 2.7). Consequence: the preview has **no `<link rel="canonical">` tag**. The
   production site does have one. Nothing else in the output differs.
2. **`safe: true` is off in the preview override**, because in safe mode Jekyll only loads
   whitelisted gems and the `github-pages` gem is not installed, which broke the `seo` tag.
3. **External link checks are point-in-time.** Several venues answer differently depending on the
   requester: CoinUp.io, Mudrex, SafeTrade, ProBit, Graviex, Bitladon, ChangeHero, SwapSpace,
   SwapSwop and CoinDCX return 403 to automated requests, CoinGecko returns 403 to the audit host
   but 200 to a browser, Digifinex and Pionex return 451 (geo block) from this location.
4. **`ravencoin.de` was deliberately NOT added.** It is a German-language independent Ravencoin
   information site (mining calculator, DAG clock, address checker, miner command generator, news,
   guides), running since 2018 and stating that it is not an official project. It answers HTTP 200
   and its news pages independently corroborate the Upbit and Bitvavo delistings used in this
   audit. It is **not** a wallet, exchange or pool, and there is no existing category for a
   language-specific community information site: the closest fits are a `_data/projects.yml`
   entry (like Raven Network Nodes) or a card in `_pages/community.md`. Deciding whether it
   belongs on an English-language official site is left to the reviewer.
5. **`LINK_HOLDS.md` is published as a static page** at `/LINK_HOLDS.html`, because a root level
   `.md` file without front matter is copied as a static file on GitHub Pages (the same way
   `History.markdown` becomes `/History.html`). It is not linked from any menu.
6. **28 logo files are now unreferenced** and are only listed in `LINK_HOLDS.md` section 6; they
   were left in the repository rather than deleted.
7. The Travis CI badge in `README.md` was kept on purpose — the replacement workflow in the
   Ravencoin core repository does not run on `master` and its badge would permanently read
   "no status".

---

## E. Instructions for the independent reviewer

**Do not treat the earlier PASS judgements as evidence.** Re-derive them from the repository.

1. **Read the diff, not the report.** Start with
   `git -C ~/projects/ravenproject.github.io diff af93e6a bbb1cdb --stat` and then the individual
   files. `git show <sha>` for each of the four commits.
2. **Check the data against the claims.** The counts in section C must be reproducible from
   `_data/exchanges.yml`; the removals must really be absent and the additions really present.
3. **Re-verify the exchange removals.** For each of the 19 removed names, decide whether it is
   (a) genuinely delisted, (b) an order-book venue that no longer lists RVN, or (c) only
   unverifiable from the audit host. Category (c) is the weakest justification for removing a
   listing; if you disagree with it, the safe move is to restore the entry and note the reason.
   `LINK_HOLDS.md` sections 3 and 4 state the intended reasoning for each one.
4. **Re-verify the spot claim for the 27 exchanges**, at least for the ones whose evidence was a
   page rather than an API, and check that no derivative-only venue slipped in. Watch for
   leveraged tokens named `RVNT`, `RVNET`, `RVNO`, `RVNOM`, `RVNQ` — those are not plain RVN.
5. **Check the ChainStrap description against its own README** at
   `https://github.com/chainstrap/chainstrap.github.io`. The description must not imply that
   ChainStrap replaces Ravencoin Core's block validation.
6. **Check that nothing else broke.** The mobile wallet support tree has 45 modified HTML files;
   confirm the relative links still resolve from each sub-directory, and that
   `mobilewallet/support/details.html` links to files that exist.
7. **Separate blocking issues from cosmetic ones.** Blocking: a broken page, a wrong or dead link
   presented as working, a claim in the PR body that is not true, a removal that loses a working
   venue. Cosmetic: card ordering, an unused logo file, the missing canonical tag in the local
   preview, the unpublished `LINK_HOLDS.html` page.
8. **Do not modify code during the review.** Report findings; the author will apply fixes.
9. **Verify the build yourself** with the command in section A. Use a full rebuild, not
   `--incremental`.
10. **Decide on `ravencoin.de`** (section D, item 4) and record the decision either way.

---

## F. PR preparation

### F.1 English title draft

```
Fix broken links and refresh Ravencoin services, wallets and exchanges
```

Alternative, more specific:

```
Verify and refresh Ravencoin links, wallets, pools and exchanges on ravencoin.org
```

### F.2 English body draft

```markdown
## Summary

This PR cleans up dead links across the site, adds community services that are
currently running, and rebuilds the "Buy Ravencoin" guide around venues where RVN
can actually be traded.

### Links

- Point the desktop wallet downloads at the real `v4.7.0` release tag and the matching
  asset names (the old links used a `v4.6.2snapshot` tag that does not exist).
- Remove the app store and Play store badges for the retired official mobile wallet
  apps, the dead testnet faucet section and the dead GetRavencoin.org community card.
- Replace a broken repository name in the README issue and compare links, the retired
  `help.github.com` setup guide and the backticks that broke the `username.github.io`
  URLs.
- Update the Contributor Covenant, Discord, MIT license and Ravencoin Node Map links.
- Fix the schema.org `u-url` in the footer, which rendered as `/%20/`.

### Mobile wallet support pages

- Fix 81 relative stylesheet references in 43 sub-directory pages, which pointed at
  `css/...` instead of `../css/...`.
- Remove a script tag for a `js/init.js` file that does not exist.
- Point the support page links at the FAQ and security pages that actually hold the
  answers.

### Services

- Add RavenVault, a non-custodial RVN wallet with web, desktop and Android clients.
- Add Corepool, an RVN mining pool.
- Add the KawTrace explorer, the Raven Network Nodes dashboard, RavenTag, Blackwing,
  the Ravencoin Node Monitor and the Ravencoin SDK as community projects.
- Add the maintained Electrum and ElectrumX forks, because the previously listed
  Electrum-RVN-SIG repositories are archived.
- Add ChainStrap, which publishes compressed chain snapshots to IPFS so a new node
  reaches its current height quickly. The node still verifies every block itself;
  the script only fills the block and chainstate folders.

### Exchanges

The exchange list was audited against the official exchange APIs, the CoinGecko RVN
ticker list and the CoinMarketCap market pairs endpoint.

- Removed 19 entries that are no longer confirmed to trade RVN spot. Five could not be
  confirmed, one is paused, and thirteen could not be verified from the audit host.
  None of them is announced as permanently delisted; `LINK_HOLDS.md` records the
  reason for each and what would have to change to list them again.
- Added Binance TR, CoinDCX and XXKK, each confirmed on the venue's own site or public
  API, plus 13 venues in an earlier commit of this PR.
- Renamed OKEx to OKX and pointed it at the RVN-USDT market, because RVN-BTC does not
  exist on OKX.
- Split the guide into "RVN Spot Exchanges", "RVN Swap / Instant Exchange Services"
  and "RVN Market Information", and added CoinGecko and CoinMarketCap as market
  information links.
- Venues that only offer perpetuals, or whose RVN market is inactive or has zero
  volume, are deliberately not listed.

### Notes for the reviewer

- `LINK_HOLDS.md` lists every deferred item, including the venues that are still on hold.
- No design or layout change was intended; the card structure is the same one the page
  used before.
```

### F.3 Test results summary

```
YAML parse            OK   (5 files: exchanges 32, platforms 4, pools 17, projects 16, wallets 22)
Duplicate names/urls  OK   (none, within and across files)
Deleted entries       OK   (19 names confirmed absent from _data/exchanges.yml)
Internal links        OK   (73 built HTML pages, 0 unresolved root-relative references)
Image assets          OK   (38 logo references, 0 missing files; 0 broken <img> in the build)
Jekyll build          OK   (GitHub Pages plugin set, ~0.25 s, no errors or warnings)
Desktop rendering     OK   (1440 px, three-column grid, logo and text cards equal height)
Mobile rendering      OK   (390 px, docScrollWidth 375 = unchanged from before the change)
```

### F.4 Scope of what would be pushed

The whole branch, i.e. `af93e6a..bbb1cdb`:

* 61 files: 57 modified, 4 added
* 497 insertions, 328 deletions
* 3 new binary assets (2 exchange banners, 1 wallet icon)

No change is made outside `_data/`, `_pages/`, `_includes/footer.html`, `README.md`,
`CODE_OF_CONDUCT.md`, `mobilewallet/support/**` and `assets/img/pages/**`.

### F.5 Procedure to push the fork and open the PR

```bash
cd ~/projects/ravenproject.github.io

# 1. Make sure the tree is clean and the branch is the expected one
git status -sb                 # expect: ## fix/website-broken-links
git log --oneline -5

# 2. Push the branch to the fork (origin = shuu-beep/ravenproject.github.io)
git push -u origin fix/website-broken-links

# 3. Open the PR against the upstream repository
gh pr create \
  --repo RavenProject/ravenproject.github.io \
  --base master \
  --head shuu-beep:fix/website-broken-links \
  --title "Fix broken links and refresh Ravencoin services, wallets and exchanges" \
  --body-file pr-body.md
```

`pr-body.md` is the text from F.2 saved to a file. After `gh pr create`, check the rendered PR
page for the file list and confirm that the number of changed files matches 61.

**Push and PR creation have NOT been done.** They wait for the reviewer's decision.

---

## Appendix — files referenced by this document

| File | Role |
|---|---|
| `LINK_HOLDS.md` | All deferred items: server errors, links left alone, removed services, the 19 removed exchanges with re-verification conditions, the venues still on hold, venues checked and not added, unused logos. Not reproduced in this document. |
| `_data/exchanges.yml` | 32 entries, 27 spot and 5 swap. |
| `_data/wallets.yml` | 22 entries. |
| `_data/pools.yml` | 17 entries. |
| `_data/projects.yml` | 16 entries, including ChainStrap. |
| `_pages/buy-ravencoin.md` | The three-section buying guide. |
| `mobilewallet/support/` | 45 modified HTML files with corrected relative paths. |
