# Data behind "Pulled, sold back, repackaged"

This folder holds the cleaned dataset behind every number and chart in the [report](../README.md): Courtyard, Collector Crypt and Phygitals pulls from Jun 24 to Sep 30, 2026, what happened to them on-chain, and the reference values they were compared with. Public research by @shady_oak1 ([thread](https://x.com/shady_oak1/status/2106006294721429869)).

Format of every file: CSV, UTF-8 (no byte-order mark), comma separated, one header row, fields quoted per RFC 4180 when they contain a comma, a quote or a line break. Times are ISO 8601 in UTC with a `Z` suffix. Money is in US dollars (USDC on-chain) as plain decimal numbers. Booleans are `true` / `false`. An empty field means missing or not applicable.

## Files

| File | Rows | Size | One row per |
|---|---|---|---|
| `pulls_collectorcrypt.csv` | 100,055 | 34.8 MB | Collector Crypt pull, Jun 26 to Sep 30 |
| `pulls_phygitals.csv` | 46,937 | 16.6 MB | Phygitals pull, Jun 26 to Sep 30 |
| `pulls_courtyard.csv` | 29,704 | 12.2 MB | Courtyard pull, Jun 24 to Sep 30 |
| `sellback_outcomes_solana.csv` | 8,727 | 2.8 MB | Collector Crypt or Phygitals pull whose on-chain outcome was read (samples and the Phygitals $500+ census) |
| `courtyard_first_exit.csv` | 28,009 | 10.9 MB | Courtyard $500+ pull, Jun 24 to Sep 23, with its first on-chain exit |
| `courtyard_block_anchors.csv` | 16 | 1 KB | Polygon reference block used to convert block numbers to time |
| `courtyard_snapshot_oct8.csv` | 7,602 | 0.9 MB | pull in the Oct 8, 2026 six-hour window of Courtyard's live pull feed |
| `phygitals_100pct_buybacks.csv` | 641 | 0.2 MB | Phygitals Black or Diamond Pack buyback |
| `phygitals_value_checks_oct.csv` | 604 | 0.1 MB | Phygitals card checked on Oct 3 (504) or Oct 6 (100) |
| `market_drift.csv` | 813 | 40 KB | $500+ slab with 3+ sales in both Jul 1 to 14 and Sep 10 to 23 |
| `charts/chart01_*.csv` to `charts/chart08_*.csv` | small | | the numbers each chart in `charts/` shows |
| `tables/*.csv` | small | | the numbers each table in the report shows |

The pull file is split by site to keep each file under 50 MB. All three pull files share one set of columns.

## Sources

Each column below is tagged with its source:

- **Site feed**: the public live pull feed each site publishes (card, grade, cert number, the price the site put on the pull, pack, time).
- **CardLadder**: CardLadder's value and recorded sales for the cert, as recorded at pull time. Only per-pull values and summary statistics are published here, never individual sale records.
- **ALT**: ALT's value for the card, as shown on the Phygitals card page ("FMV by ALT" and the "Price data by ALT" price history) or as ALT's own live value.
- **Solana**: public Solana transactions (Collector Crypt and Phygitals), linked on solscan.io.
- **Polygon**: public Polygon token transfers of the Courtyard card contract 0x251be3a17af4892035c37ebf5890f4a4d889dcad, linked on polygonscan.com.
- **Derived**: computed from the columns above, as described.

House wallets that appear in this README or in transaction links: Courtyard buyback 0x66dbff2ce099d19b4e8c5dc8b254ec7aeaf5e642 and redemption 0x64ecc7f2753df33e21f7c4211ea2b68b608bf8f9; Collector Crypt payout GachaNgyXTU3zFogQ8Z5jR2BLXs8215X2AtEH18VxJq3 and vaults miDtj3vgdxVykHzRyFwyG8MXpvK8eQqamSLVdBr7WPt, HiGHqwYddP5N2waqUmXPdaASpMpUEvfqPr2fSawctEb, LGNDfXQFMiRMz3qqTNAREmRFQutMvazqqRrzn5i98uj, epiC3zkqa1RfcPMMM1Kc8m3GZGDwF2RmjbfA3g1BBjn, Low6UekJP3QrFVMfNRTL8CPK2SiGFhvp57sgF2pkmVu; Phygitals house 62Q9eeDY3eM8A5CnprBGYMPShdBjAzdpBdr71QHsS8dS and Phygitals Unvaulted 6VhFyXwkgXmUdy2zNJUSBg5uDat9GTXyPjcCrY8n9DbA. No puller wallet or user name is a column anywhere. Transaction links show the parties of each transaction on the explorer.

## Windows

| What | Window (UTC) |
|---|---|
| Pulls (pull files) | Jun 24 to Sep 30, 2026 (coverage begins Jun 24 on Courtyard and Jun 26 on the other two) |
| Pulled again | base pulls Jun 24 to Sep 15; re-pulls through Sep 30 |
| Forward test (next 7 days) | pulls Jun 24 to Sep 23 |
| Collector Crypt and Phygitals outcomes | pulls Jun 24 to Sep 15, outcomes read Oct 8, 2026 |
| Courtyard first exit | pulls Jun 24 to Sep 23, transfers read Oct 1, 2026 |
| Courtyard snapshot | feed window 07:52:12 to 13:52:20 on Oct 8, 2026 |
| Market drift | sales Jul 1 to 14 against Sep 10 to 23 |
| Phygitals 100% buybacks | the latest 5 activity entries per card, read Oct 3, 2026 (buybacks Jun 9 to Oct 3) |
| Phygitals value checks | Oct 3 and Oct 6, 2026 |

## Columns

### pulls_collectorcrypt.csv, pulls_phygitals.csv, pulls_courtyard.csv

| Column | Type | Unit | Meaning | Source |
|---|---|---|---|---|
| `pull_id` | text | | Stable id for this dataset: `cc-`, `ph-` or `cy-` plus a sequence number in pull-time order. Used to join the other files. | Derived |
| `site` | text | | `collectorcrypt`, `phygitals` or `courtyard` | Site feed |
| `pulled_at_utc` | time | | Time of the pull in the feed | Site feed |
| `pack_name` | text | | Pack the card was pulled from. Phygitals packs that the feed reported only by number are named from the site's pack catalogue: 13 Rookie Pack, 14 Elite Pack, 15 Legend Pack, 27 Platinum Pack, 29 Mythic Pack. Other Phygitals pack names are as the feed spelled them, including its short suffix. | Site feed |
| `card_name` | text | | Card title as the site shows it | Site feed |
| `grader` | text | | `PSA`, `BGS` or `CGC` | Site feed |
| `grade` | number | | Grade on the slab (1 to 10, half grades allowed) | Site feed |
| `cert_number` | text | | Grading cert number, kept as text so leading zeros survive. Re-pulls are matched on it. | Site feed |
| `site_price_usd` | number | USD | The price the site put on the pull (its pull value). Up to 6 decimals, as the site reported it. | Site feed |
| `cardladder_value_usd` | number | USD | CardLadder's value for the cert at pull time; empty when CardLadder had none | CardLadder |
| `last5_sales_median_usd` | number | USD | Median of the 5 most recent CardLadder-recorded sales of the cert at or before the pull time (fewer if fewer exist), taken from every CardLadder sale recorded for that cert at pulls on the same site up to Oct 3, 2026, 04:28 UTC, duplicates removed by sale link, or by date and price | CardLadder, derived |
| `cardladder_sales_count` | integer | sales | Number of recent CardLadder sales recorded at pull time (up to 20) | CardLadder |
| `cardladder_monthly_sales` | integer | sales | CardLadder's count of sales in the last month at pull time | CardLadder |
| `last_sale_price_usd` | number | USD | CardLadder's most recent sale price at pull time | CardLadder |
| `last_sale_date` | time | | Date of that sale | CardLadder |
| `card_page_url` | text | | The card's public page on the site | Site feed |
| `token_id` | text | | The card's token: the Solana asset address (Collector Crypt, Phygitals) or the decimal token id in the Courtyard card contract (Courtyard). On Collector Crypt and Phygitals the same slab keeps its asset across pulls; Courtyard mints a new token per pull. | Site feed |
| `price_tier` | text | | `500+`, or `300-499.99` (Collector Crypt) / `100-499.99` (Phygitals), from `site_price_usd` | Derived |
| `above_cardladder` | boolean | | `site_price_usd` > `cardladder_value_usd`; empty without a CardLadder value | Derived |
| `prior30_sale_count` | integer | sales | Number of CardLadder-recorded sales of the cert in the 30 days up to the pull, among the sales recorded at this pull. Empty when no sale before the pull was recorded at all. | CardLadder, derived |
| `prior30_max_sale_usd` | number | USD | The highest of those sales; if there were none in the 30 days, the last recorded sale before the pull (then `prior30_fallback_last_sale_used` is `true`) | CardLadder, derived |
| `above_every_prior30_sale` | boolean | | `site_price_usd` > `prior30_max_sale_usd` | Derived |
| `prior30_fallback_last_sale_used` | boolean | | `true` when no sale fell in the 30 days and the last earlier sale was used | Derived |
| `pulled_again_by_sep30` | boolean | | For pulls Jun 24 to Sep 15 only: the same cert appears in a later pull on the same site in these files, through Sep 30. Empty for later pulls. | Derived |
| `next7_sale_count` | integer | sales | For pulls Jun 24 to Sep 23 only: number of CardLadder-recorded sales of the cert in the 7 days after the pull (later than the pull, up to 7 x 24 h), from the same per-cert sale set as `last5_sales_median_usd` | CardLadder, derived |
| `next7_sale_median_usd` | number | USD | Median of those sales | CardLadder, derived |
| `above_next7_median` | boolean | | `site_price_usd` > `next7_sale_median_usd` | Derived |

The flags were computed on the unrounded values; recomputing them from the published columns gives the same result for every row.

### sellback_outcomes_solana.csv

Collector Crypt and Phygitals pulls from Jun 24 to Sep 15 whose next change of holder was read from Solana on Oct 8, 2026. Each row joins to a pull file by `pull_id`.

| Column | Type | Unit | Meaning | Source |
|---|---|---|---|---|
| `pull_id` | text | | Joins to the pull files | Derived |
| `site` | text | | `collectorcrypt` or `phygitals` | Site feed |
| `stratum` | text | | Sampling tier: `cc_500_plus` (Collector Crypt $500+), `cc_300_to_499`, `ph_500_plus`, `ph_100_to_499` | Derived |
| `in_sample` | boolean | | Drawn in the fixed-seed random sample (1,000 / 400 / 1,000 / 400 pulls per tier; seed 20261008) | Derived |
| `in_census` | boolean | | Part of the census of all 6,927 Phygitals $500+ pulls | Derived |
| `outcome` | text | | The first change of holder after the pull: `sold_back` (slab back to the house wallet and USDC from the operator to the puller in the same transaction, or for Collector Crypt sold back before delivery, below), `redeemed_or_shipped` (Phygitals: sent to the Phygitals Unvaulted account; Collector Crypt: token burned with a shipment memo), `moved_or_sold_to_user` (any other wallet, including marketplace sales), `still_held`, `returned_no_payout`, `unresolved` | Solana |
| `sold_back_before_delivery` | boolean | | Collector Crypt only: the slab never left the vault; the puller paid for the pack and the payout wallet paid the puller the pull price times the pack rate seconds later | Solana |
| `seconds_to_sellback` | integer | s | For `sold_back`: block time of the sell-back minus block time of the pull transaction, or of the pack payment when `timing_basis` is `pack_payment`. Empty for 8 Collector Crypt sell-backs before delivery whose pack payment was not on the puller's history. | Solana |
| `timing_basis` | text | | `pull_tx` or `pack_payment` | Derived |
| `usdc_paid` | number | USDC | USDC received by the puller in the sell-back transaction | Solana |
| `payout_pct_of_price` | number | % | `usdc_paid` / `site_price_usd` x 100 | Derived |
| `published_rate_pct` | number | % | The pack's published instant buyback rate: Collector Crypt's pack catalogue (85% for $25 and $50 packs, 90% for $100 to $500 packs, 93% for $1,000 to $5,000 packs); Phygitals' per-pack rates as shown on Jul 8, 2026 (85, 90, 92 or 100%), with Black and Diamond Pack at 100% as of Oct 3, 2026 | Site |
| `at_published_rate` | boolean | | `payout_pct_of_price` within 0.5 points of `published_rate_pct` (for Black and Diamond Pack, of 92% or 100%) | Derived |
| `pull_tx_url` | text | | The transaction in which the slab left the house wallet for the puller (within 5 minutes before to 15 minutes after the feed time; closest if several). Empty for sell-backs before delivery. | Solana |
| `outcome_tx_url` | text | | The transaction of the outcome; empty for `still_held` | Solana |
| `pack_payment_tx_url` | text | | Collector Crypt: the order's pack payment transaction, where found | Solana |
| `slab_pulled_again_later` | boolean | | The same cert appears in a later pull in the dataset on the same site, as of Oct 8, 2026 (used for the consistency check) | Derived |
| `independent_recheck` | text | | `match` for the 40 sample results (10 per tier) whose pull and outcome transactions were re-read and decoded independently | Solana |
| `census_recheck` | text | | `match` for the 10 census results re-checked the same way | Solana |

### courtyard_first_exit.csv

Every Courtyard pull priced $500+ in the dataset from Jun 24 to Sep 23 (one row per token). Token transfers read Oct 1, 2026.

| Column | Type | Unit | Meaning | Source |
|---|---|---|---|---|
| `token_id` | text | | Decimal token id in the Courtyard card contract | Polygon |
| `pull_id` | text | | Joins to `pulls_courtyard.csv` | Derived |
| `pulled_at_utc` | time | | Feed time of the pull | Site feed |
| `card_name`, `cert_number` | text | | From the pull file | Site feed |
| `site_price_usd` | number | USD | The site's price on the pull | Site feed |
| `mint_block` | integer | block | Block of the token's mint to the puller | Polygon |
| `first_exit` | text | | `sold_back` (first transfer to either house wallet went to the buyback wallet), `redeemed` (went to the redemption wallet), `still_held` (neither, as of the read) | Polygon |
| `exit_block` | integer | block | Block of that first transfer | Polygon |
| `seconds_mint_to_exit` | number | s | Time from mint to exit, both blocks converted to time by linear interpolation between the reference blocks in `courtyard_block_anchors.csv` (about 2 s accuracy) | Derived |
| `usdc_paid_gross` | number | USDC | USDC sent by the buyback wallet in the sell-back transaction | Polygon |
| `polygonscan_token_url` | text | | The token's page on Polygonscan | Polygon |
| `history_check` | text | | `match` for the 400 random pulls (Jul 1 to Sep 23) whose first-exit reading was compared with Courtyard's own public transaction history for the asset: whether the token ever went to the buyback wallet and whether it ever went to the redemption wallet | Site |

`courtyard_block_anchors.csv`: `block`, `unix_time` (seconds), `utc`. Weekly reference blocks from Jun 23 plus the latest block at the read.

### courtyard_snapshot_oct8.csv

The rolling six-hour window of Courtyard's public live pull feed as it stood at 13:52:27 UTC on Oct 8, 2026, all price tiers.

| Column | Type | Unit | Meaning | Source |
|---|---|---|---|---|
| `created_at_utc` | time | | Time of the pull | Site feed |
| `card_name` | text | | Card or item title | Site feed |
| `site_price_usd` | number | USD | The site's price on the pull | Site feed |
| `price_tier` | text | | `0-25`, `25-100`, `100-500`, `500+` (lower bound included) | Derived |
| `at_least_2h_old` | boolean | | Pulled at or before 11:52:27 UTC, two hours before the snapshot | Derived |
| `outcome` | text | | `sold_back` or `redeemed` as marked in the feed by the snapshot time, else `held` | Site feed |

### phygitals_100pct_buybacks.csv

Black Pack and Diamond Pack buybacks found in the latest 5 activity entries of each card in the dataset pulled from those packs, read Oct 3, 2026.

| Column | Type | Unit | Meaning | Source |
|---|---|---|---|---|
| `card_name`, `cert_number` | text | | The card | Site |
| `pack` | text | | `Black Pack` or `Diamond Pack` | Site |
| `buyback_at_utc`, `buyback_day` | time, date | | Time and UTC date of the buyback | Solana |
| `usdc_paid` | number | USDC | USDC the house wallet paid the seller | Solana |
| `pull_value_usd` | number | USD | The site's price on the pull of the same card that this buyback followed (matched within one hour; 588 of 641 matched); empty when none matched | Site feed |
| `alt_value_that_day_usd` | number | USD | ALT's value on the buyback day from the price history on the card's Phygitals page; when the history ends before that day, its last value (`alt_history_stale` is `true`) | ALT |
| `alt_history_stale` | boolean | | See above | Derived |
| `cardladder_value_usd` | number | USD | CardLadder's value at the matched pull; empty when none matched | CardLadder |
| `group` | text | | `under_90_both`: `usdc_paid` < 90% of both ALT and CardLadder; `over_110_both`: > 110% of both; `neither` otherwise. Only rows where ALT and CardLadder agree within 25% (abs(ALT / CardLadder - 1) < 0.25) can be in the first two groups. | Derived |
| `seconds_pull_to_buyback` | integer | s | `buyback_at_utc` minus the matched pull's feed time; negative (9 rows) when the feed time came after the buyback | Derived |
| `tx_url` | text | | The buyback transaction | Solana |

### phygitals_value_checks_oct.csv

| Column | Type | Unit | Meaning | Source |
|---|---|---|---|---|
| `check` | text | | `page_value_vs_last_pull`: every Phygitals $500+ cert whose latest pull in the dataset was Sep 15 or later (504), checked Oct 3. `live_alt_vs_pull_value`: a random sample of 100 Phygitals certs last pulled in the 10 days before Oct 6 (40 priced $100 to $500, 35 $500 to $2,000, 25 $2,000+), checked Oct 6. | Derived |
| `checked_on` | date | | Date of the check | |
| `card_name`, `cert_number` | text | | The card | Site feed |
| `last_pull_at_utc` | time | | The latest pull in the dataset before the check | Site feed |
| `last_pull_value_usd` | number | USD | The site's price on that pull | Site feed |
| `phygitals_page_value_usd` | number | USD | The value the card's Phygitals page showed as "FMV by ALT" on Oct 3 | ALT |
| `holder_class` | text | | Who held the card on Oct 3, as the card's Phygitals page reported its owner: `house` (the Phygitals house wallet), `phygitals_unvaulted` (the Phygitals Unvaulted account), `user` | Site |
| `alt_live_value_usd` | number | USD | ALT's own live value for the cert on Oct 6 | ALT |

### market_drift.csv

| Column | Type | Unit | Meaning | Source |
|---|---|---|---|---|
| `site` | text | | Site the slab was pulled on | Site feed |
| `cert_number` | text | | The slab | Site feed |
| `jul_sale_count`, `jul_median_usd` | integer, number | sales, USD | CardLadder-recorded sales of the cert from Jul 1 to Jul 14 and their median | CardLadder, derived |
| `sep_sale_count`, `sep_median_usd` | integer, number | sales, USD | Same for Sep 10 to Sep 23 | CardLadder, derived |
| `ratio` | number | | `sep_median_usd` / `jul_median_usd`, rounded to 4 decimals | Derived |

Sales come from every CardLadder sale recorded for the cert at a $500+ pull on that site up to Oct 3, 2026, 04:28 UTC, duplicates removed by sale link, or by date and price. Only slabs with 3 or more sales in each window are listed.

### charts/ and tables/

One file per chart (`chart01` to `chart08`, plus `chart02_sellback_speed_curve.csv` with the plotted curve, seconds below 1 shown at 1 s) and per report table (`sellback_by_site_and_tier`, `courtyard_snapshot_oct8`, `forward_test`, `forward_accuracy`, `market_drift`, `most_pulled_slabs`). Shares and other computed values are given to 4 decimals, so each can be rounded once to the precision the report prints (the report never rounds an already rounded value); every value can be rebuilt from the row-level files as described below. `chart06_priced_above_market.csv` gives both bases of chart 6: all $500+ pulls with a CardLadder value (first bar) and those with a sale recorded before the pull (second bar). `tables/most_pulled_slabs.csv` includes `distinct_accounts`, the number of different receiving wallets (Collector Crypt) or user names (Courtyard) on the most-pulled slab; it is published only as that count.

## Cleaning

Applied to the pull files (and carried into every file that joins to them):

| Step | Rows affected |
|---|---|
| Kept pulls with a feed time from Jun 24, 2026 00:00 to Sep 30, 2026 23:59:59 UTC; later pulls dropped | all pulls after Sep 30 |
| Dropped Collector Crypt pulls under $300 and Phygitals pulls under $100 (below the tiers the dataset covers) | 16 and 4 |
| Duplicates: two feed events for the same slab (same token and cert), the same receiving wallet and the same price, less than 5 seconds apart, are one pull; the later event was dropped | 12 (Collector Crypt) |
| Kept as separate pulls: same slab within 5 seconds but two different receiving wallets (one pull cannot go to two wallets) | 5 pairs |
| Courtyard: the same token reported twice (Courtyard mints a new token for every pull, so a repeated token is the same pull; the repeats came 25 seconds to 2 hours later, same user and price); the later event was dropped | 7 |
| Missing or zero site price | 0 |
| Grade outside 1 to 10 or not a whole or half grade | 0 |
| Missing cert number | 0 |
| Grader spellings normalized (for example Beckett to BGS) | 0 (all were already PSA, BGS or CGC) |
| Card names: runs of spaces collapsed and ends trimmed | 91 |
| Card names: symbols the feed had already garbled (a star or heart symbol received as "â??") removed | 25 in the pull files, 1 in courtyard_snapshot_oct8.csv |
| Phygitals pack numbers mapped to pack names (13, 14, 15, 27, 29) | 37,232 |
| Pack names: runs of spaces collapsed | 24 |
| Identifiers replaced by `pull_id`; puller wallets, user names and non-public columns removed | all |

Times were already UTC. Cert numbers are text with leading zeros kept. Courtyard asset ids are given as the decimal token id used on Polygonscan.

## Reproducing the report's numbers

Notation: "CC", "PH" and "CY" are the three pull files. "$500+" means `site_price_usd` >= 500. A "median" is the middle value (the mean of the two middle values for an even count) unless stated. Halves round up.

**Summary and section 1**

- Courtyard first exit 80.8% sold back, 14.3% redeemed, 5.0% still held (28,009): `courtyard_first_exit.csv`, count of `first_exit` values / 28,009 (22,626, 3,993, 1,390).
- Courtyard sell-back timing, median 50 s, p25 23 s, p75 4.3 min: rows with `first_exit` = `sold_back`, sort `seconds_mint_to_exit`, take the value at position floor(p x (n - 1)) counting from 0 (49.5 s, 22.5 s, 255 s).
- Courtyard 54.0% / 86.8% / 94.0% within 1 min / 1 h / 24 h: share of those 22,626 values <= 60, 3,600 and 86,400.
- Courtyard $24.8M gross: sum of `usdc_paid_gross` over `sold_back` rows.
- Courtyard median payout 84.6%, and 84.6% paid on 81% of sell-backs: `usdc_paid_gross` / `site_price_usd` x 100 over `sold_back` rows; median; share within 0.5 points of 84.6.
- Courtyard 400 of 400 matched: rows with `history_check` = `match` / rows with `history_check` filled.
- Collector Crypt 95.9% sold back of 1,000, Phygitals 93.1% of 6,927, and the other tier rows of the section 1 table: `sellback_outcomes_solana.csv`, filter `stratum` (for `ph_500_plus` use `in_census` = `true`, for the others `in_sample` = `true`); share with `outcome` = `sold_back`.
- Margins ±3.1, ±4.8, ±4.9 points: 1.96 x sqrt(0.25 / n) x sqrt((N - n) / (N - 1)) x 100 with n the sample size and N the tier's pulls Jun 24 to Sep 15 in the pull files (68,891, 17,368, 27,758).
- Median time to sell-back and p75 (9 s and 23 s; 18 and 36; 20 and 51; 17 and 31) and within 1 min / 1 h / 24 h: same filters, `outcome` = `sold_back` and `seconds_to_sellback` filled; quantiles by linear interpolation between ranks; shares of values <= 60, 3,600, 86,400.
- Median payout (93.0, 90.0, 90.0, 90.0): median of `payout_pct_of_price` over the same sell-backs.
- 384 of 1,000 Collector Crypt $500+ pulls never left the vault, median 2 s: `stratum` = `cc_500_plus`, `sold_back_before_delivery` = `true`; median `seconds_to_sellback`.
- Collector Crypt 93% the most common rate on $500+ pulls, Phygitals 90%: most frequent `published_rate_pct` among `sold_back` rows of `cc_500_plus` and of `ph_500_plus` (census).
- All Collector Crypt sell-backs at the published rate; Phygitals $500+ 96.5%, and 99.8% from Jul 1: share of `at_published_rate` = `true` among `sold_back` rows (Phygitals: census rows; for "from Jul 1", join `pull_id` to PH and keep `pulled_at_utc` >= 2026-07-01).
- Courtyard snapshot (07:52:12 to 13:52:20; 4,748 pulls at least 2 h old, created 07:52:12 to 11:52:18; 79.0% sold back, 1.6% redeemed; table by tier): `courtyard_snapshot_oct8.csv`, filter `at_least_2h_old` = `true`, shares of `outcome` overall and by `price_tier`.

**Section 2**

- Pulled again by Sep 30, 95.8% / 92.9% / 77.8%: in each pull file, $500+ and `pulled_at_utc` < 2026-09-16; share of `pulled_again_by_sep30` = `true` (CC 66,012 of 68,891; PH 6,435 of 6,927; CY 20,401 of 26,210).
- 99.4% of sampled pulls whose slab was pulled again had first been sold back: `sellback_outcomes_solana.csv`, `in_sample` = `true` and `slab_pulled_again_later` = `true`; share with `outcome` = `sold_back` or `returned_no_payout` (2,607 of 2,623).
- Pulls per slab 15.0 / 11.4 / 4.2: $500+ rows / distinct `cert_number` (CC 81,127 / 5,397; PH 11,539 / 1,008; CY 29,704 / 7,065).
- Median gap between pulls 20.4 h / 13 h / 30 h: for each cert, sort its $500+ pulls by time and take the gaps between consecutive pulls in hours; sort all gaps and take the value at position floor(n / 2) counting from 0 (20.4, 13.0, 29.6).
- Most-pulled slab (105, 160 and 56 pulls): the cert with the most $500+ rows in each file; the 66 accounts on the Collector Crypt slab are in `tables/most_pulled_slabs.csv`.
- First exits to redemption or shipping, 14.3% / 2.4% / 4.5%: Courtyard as above; `cc_500_plus` sample and `ph_500_plus` census, share of `outcome` = `redeemed_or_shipped`.

**Section 3**

- Priced above CardLadder, 66.2% / 55.8% / 61.1% of 122,219 pulls: $500+ and `cardladder_value_usd` > 0 (81,074 / 11,482 / 29,663); share of `above_cardladder` = `true`.
- Above CardLadder and every sale in the prior 30 days, 22.6% (18,270) / 29.2% (3,353) / 13.8% (4,079) of the 121,953 with a sale recorded before the pull: the same pulls with `prior30_sale_count` filled (80,884 / 11,466 / 29,603); `above_cardladder` and `above_every_prior30_sale` both `true`.
- Median sales in those 30 days, 12 / 3 / 9: median of `prior30_sale_count` over those 121,953 pulls.
- 40% of Phygitals "above both" cases rested on a single last sale: among those rows, share of `prior30_fallback_last_sale_used` = `true`.
- With at least one sale that month, 22.6% / 22.1% / 12.8%: same base with `prior30_sale_count` >= 1.
- Forward test, priced above the next-7-day median sale, 75.5% / 55.1% / 66.9%: $500+, `cardladder_value_usd` > 0, `pulled_at_utc` < 2026-09-24, `next7_sale_count` >= 1; share of `above_next7_median` = `true`. With `next7_sale_count` >= 3: 81.1% / 57.0% / 68.0%.
- Accuracy table and chart 7: rows with `next7_sale_count` >= 3 and `last5_sales_median_usd` filled (33,645 / 2,113 / 6,520); for each reference (`site_price_usd`, `cardladder_value_usd`, `last5_sales_median_usd`) the error is (reference - `next7_sale_median_usd`) / `next7_sale_median_usd`. Average miss = mean of the absolute errors; typical signed miss = median error; chart 7 shares = errors > 0 and < 0.
- Market drift, -10.5% / -6.2% / -5.2%, 658 / 52 / 103 slabs, 23.1% / 42.3% / 40.8% up: `market_drift.csv` by `site`; median `ratio` minus 1; row count; share of `ratio` > 1.
- 96% of 298 house-held Phygitals cards still carried their last pull value: `phygitals_value_checks_oct.csv`, `check` = `page_value_vs_last_pull`, `holder_class` = `house`; share where abs(`phygitals_page_value_usd` - `last_pull_value_usd`) < 0.005 (285 of 298).
- 98% of re-pulls since September showed the identical value: PH sorted by time, pulls from 2026-09-01 whose cert has an earlier pull in the file; share whose `site_price_usd` equals that earlier pull's to within half a cent (98.4% of all tiers, 98.2% of $500+).
- Pull values a median 14% from ALT's live value, both directions: `check` = `live_alt_vs_pull_value`; median of abs(`alt_live_value_usd` - `last_pull_value_usd`) / `last_pull_value_usd` (13.8%; the pull value sat above ALT's live value in 49 cases and below it in 51).
- The Gyarados page values ($56,000 against a price history of $25,214) come from the page screenshot in `receipts/`, not from these files.

**Section 4**

- Receipts (a) and (b): `phygitals_100pct_buybacks.csv`, the Dark Slowbro rows paid 1,551.71 (Sep 15 and Sep 17) and the Caterpie rows for cert 28256413 paid 5,492.20 twice on Sep 16 (a second copy of the same card and grade, cert 28333872, was also bought back at 5,492.20 on Sep 19), with `alt_value_that_day_usd`, `cardladder_value_usd` and `tx_url`.
- 641 buybacks, 43 under 90% of both, 36 over 110% of both: row count and counts of `group`.
- Median 15 s from pull to buyback (588 with timing): `seconds_pull_to_buyback` filled; sorted, the value at position floor((n - 1) / 2) counting from 0.
- Payout equal to the pull value within 0.5% in 548 of 588: rows with `pull_value_usd`; abs(`usdc_paid` / `pull_value_usd` - 1) <= 0.005.
- Receipt (c): the `sellback_outcomes_solana.csv` row whose `pull_tx_url` ends in `4AnCNsaR...` (17 s, 674.25 USDC, 93%); its `pull_id` gives the card and the $725.00 price.
- Receipt (d): the `courtyard_first_exit.csv` row with `mint_block` 92355822 (exit block 92355855, 50 s, 1,195.82 USDC gross, price $1,413.50).

**Section 6 (method)**

- Coverage begins Jun 24 (Courtyard) and Jun 26 (the other two): earliest `pulled_at_utc` per pull file.
- The 1,000-pull Phygitals $500+ sample, 92.3% ±2.9: `ph_500_plus` rows with `in_sample` = `true`.
- 40 of 40 sampled results matched: `independent_recheck` = `match` (and 10 of 10 for `census_recheck`). None unresolved: no `outcome` = `unresolved`.

## Counts in the report

The report, its charts and the one-page brief take every count from these cleaned files, so they are identical to what the recipes above give: for example 68,891 Collector Crypt and 26,210 Courtyard $500+ pulls in the pulled-again base, 122,370 $500+ pulls in chart 4, and 122,219 $500+ pulls with a CardLadder value. The 19 duplicate feed events removed in cleaning (12 Collector Crypt, 7 Courtyard) changed no percentage. None of the removed rows was in the random sample, so the study samples are unchanged; the Collector Crypt $500+ sample was drawn from the same 68,891 pulls plus the 12 duplicates. The Courtyard on-chain file already counted each token once.

The one figure that cannot be recomputed from row-level data is the number of different accounts on the most-pulled slab (66 on Collector Crypt), because no puller wallet or user name is published; it is given as a count in `tables/most_pulled_slabs.csv`.

## Notes and limits

- The per-cert sale sets behind `last5_sales_median_usd`, `next7_*` and `market_drift.csv` are frozen at Oct 3, 2026, 04:28 UTC, the time of the forward-test analysis. CardLadder keeps recording late-reported sales, so values taken later can differ slightly.
- Re-pulls not in the dataset are not counted, so re-pull counts are minimums.
- CardLadder and ALT values are those companies' estimates.
- Sample margins assume simple random sampling; pulls of the same card are correlated, so treat them as approximate.

## Licence and attribution

Our data (everything in this folder except the CardLadder and ALT values) is licensed CC BY 4.0, like the report: credit "@shady_oak1, Pulled, sold back, repackaged (2026)" with a link to https://github.com/shadyoakcoding/gacha-analysis. CardLadder values and sale statistics and ALT values are their owners' estimates, included only so the report's figures can be checked; they remain the property of CardLadder and ALT. Site names and transaction records belong to their respective owners.
