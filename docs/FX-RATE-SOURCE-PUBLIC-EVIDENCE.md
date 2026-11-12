# Central-bank FX source public evidence

**Task:** MN-PLAN-015 · **Plan phase:** Phase 0 — close planning and turn decisions into build contracts<br>
**Evidence checked:** 12 November 2026<br>
**Status:** Public-source inventory only; no rate source selected and no rates copied into product data.

## Scope and acceptance

The approved billing and reporting rules require official central-bank reference rates, but the repository does not name a rate series, rate type, source, quote direction, or stale/missing-rate behavior. This inventory records what the Central Bank of Kenya (CBK), Bank of Uganda (BoU), and Bank of Tanzania (BoT) publicly describe for KES, UGX, and TZS, and what remains unverified.

Acceptance for this planning task:

- Identify official public source pages and the rate type/date fields they publicly describe.
- Distinguish separate series or source descriptions where they may not be interchangeable.
- Record evidence, access limitations, and unanswered integration questions without selecting a source or conversion formula.
- Link the evidence to the existing billing/reporting requirements while keeping `MN-DEC-015` open.

No quote, reference rate, pair convention, calculation, fallback, or legal/tax interpretation is approved here. No rates were fetched into a dataset or applied to invoice/report examples. No API client, scheduled job, or external integration was configured.

## Public source inventory

| Institution / public page | What the source describes | Public data shape and historical access | Limits for MylesNet |
|---|---|---|---|
| **CBK — Foreign Exchange Rates** | The Financial Markets department says it compiles indicative rates daily for public use, reflecting average buying and selling rates of major market participants at the **open of trade**. | The page exposes date, currency, mean, buy, and sell fields, table export, and downloadable historical CSV/yearly datasets. Currency labels include USD and regional labels such as `KES/USHS` and `KES/TSHS`. | A separate CBK [Forex page](https://www.centralbank.go.ke/forex/) describes rates calculated as a weighted average of registered interbank spot trades **during the trading session**. These may be different series or cutoffs; do not treat either as the approved invoice/report rate without finance review. The slash labels do not establish a quote direction on their own. Publication time, correction policy, service terms, and non-business-day behavior remain unverified. |
| **BoU — Official Foreign Exchange Rates** | The public page says BoU publishes rates daily; it characterizes them as indicative of prevailing market conditions and says the coverage includes major international currencies and COMESA member states. It describes an interactive historical trend chart. | The review tool could read this descriptive page text but not the live rate table. The reviewed page text did not document field names, quote direction, export/API contract, or an as-of time beyond a daily cadence. | Confirm that KES, TZS, and any required invoice/report pairs are actually present in the current source, whether values are means or buy/sell rates, when they are observed/published, and how historical values can be retrieved. Do not assume that a public chart is a supported machine interface. |
| **BoT — Exchange Rates** | The current public table lists currency, buying, selling, mean, and transaction date. The page includes KES, UGX, and USD rows. | A separate **Previous Exchange Rates** page offers start- and end-date filters. The reviewed page establishes a date-oriented lookup UI, but did not establish the maximum history, an automated endpoint contract, or an export format. BoT's weekly financial-markets report labels its published series “Official Indicative Mean Exchange Rates” and states the reported values are **TZS per unit of foreign currency**. | Confirm that this quote convention and rate definition also apply to the daily page, the available daily history, publication/revision behavior, interface stability, access terms, and non-business-day handling. A weekly series is not evidence that the daily series has the same date/cutoff semantics. |

The public sources describe indicative market information; they do not say that the rate is a guaranteed transaction rate, a tax-authorized valuation method for MylesNet, or an approved price input. This document does not treat the institutions' publication as legal, accounting, or tax advice.

## Decisions still needed for billing and reporting

The current requirements distinguish invoice-time conversion from report-time conversion:

- **Hotspot billing:** calculate usage fees in each sale currency, then convert to the selected billing legal entity's currency using the approved central-bank reference rate at invoice issuance. Store and show the applied rate, source, pair, and as-of time on the invoice.
- **Consolidated reporting:** use the latest approved central-bank reference rate at report view time and show the rate and as-of time. Separate branch/entity-currency totals remain available.
- **Local price schedule:** publish validated fixed KES, UGX, and TZS equivalents for the USD $0.25 PPPoE reference and the $5-equivalent combined monthly minimum. These values are not derived or set by this source inventory.

The finance owner and country counsel must decide and document, before implementation or pricing approval:

1. Which official daily series is authoritative for each use case, and whether one central-bank source or the invoice entity's country source governs a cross-country conversion.
2. The rate type to use (for example, published mean versus a side-specific buy/sell rate), exact source currency, target currency, pair direction, inversion/triangulation rules, and how a direct cross-rate relates to a rate derived through USD.
3. Effective date versus retrieval/publication time, business-day and holiday policy, timezone, invoice issuance cutoff, and behavior when the source publishes late, revises a prior value, omits a pair, or is unavailable.
4. A maximum acceptable rate age for consolidated reports; whether to display stale values with a warning, omit conversion, or use another approved behavior. No fallback is authorized by this note.
5. Precision, rounding, currency minor-unit treatment, immutable invoice snapshot fields, rate-source attribution, history retention, and reconciliation evidence.
6. The permitted access method, rate limits, historical lookback, licensing/attribution, change notice, and operational support for any automated feed.

Until those items are approved, do not build a production FX adapter, derive a rate from a different source, fill a missing rate with the last observed value, or silently present a stale rate as current.

## Evidence notes and limitations

- CBK's public rate pages describe two calculation contexts: an indicative mean around the open of trade and a separate interbank spot-trade weighted-average description. The pages must be reviewed together with finance before selecting the rate series.
- BoU's current public description claims daily indicative rates and interactive history, but the live rate rows and their schema were not available to this review tool. A historical methodological document is not substituted for current field-level confirmation.
- BoT's public daily table exposes buying/selling/mean/date columns. A BoT weekly report provides an explicit TZS-per-unit-of-foreign-currency label for its weekly official indicative mean series; verify that this convention carries over to the daily table before use.
- The current public pages do not establish service-level availability, update deadline, corrections/versioning guarantees, an API contract, contractual reuse rights, or exact non-business-day behavior. “Not found” means not found in the public material reviewed, not that the institution has no such information.
- No current numeric rate sample is reproduced. Rates change and a sample value would not decide the approved series or policy.

## Official sources

Pages reviewed on 12 November 2026. Dynamic pages and data access must be rechecked before any implementation decision.

### Kenya

- [CBK Foreign Exchange Rates](https://www.centralbank.go.ke/rates/forex-exchange-rates/) — daily indicative rate description, table fields, export and historical CSV/yearly data.
- [CBK Forex](https://www.centralbank.go.ke/forex/) — describes an exchange-rate statistic based on weighted-average registered interbank spot trades during the trading session; distinguish it from the indicative open-of-trade page.
- [CBK Exchange Rate Statistics](https://www.centralbank.go.ke/statistics/exchange-rates/) — monthly average and end-period statistics, a separate aggregation from daily invoice-time candidates.

### Uganda

- [BoU Official Foreign Exchange Rates](https://bou.or.ug/interest_rates_exchange_rates) — current public description of daily indicative rates, stated coverage, and interactive historical trends. The live rows/schema were not extractable in this review.

### Tanzania

- [BoT Exchange Rates](https://www.bot.go.tz/ExchangeRate/excRates?lang=en) — daily public table with currency, buying, selling, mean, and transaction date.
- [BoT Previous Exchange Rates](https://www.bot.go.tz/ExchangeRate/previous_rates?lang=en) — historical lookup page with start- and end-date inputs; maximum range and automated access were not verified.
- [BoT Domestic Financial Markets Weekly Report, 2–6 November 2026](https://www.bot.go.tz/Publications/Regular/Weekly%20Financial%20Markets%20Report/en/2026110911470849.pdf) — labels a weekly table “Official Indicative Mean Exchange Rates” and explicitly titles the values “TZS per unit of foreign currency”. It is weekly context, not proof of the daily table's full contract.

## Related requirements

- [Decision register — MN-DEC-015 and MN-DEC-022](DECISION-REGISTER.md)
- [Readiness decisions — pricing and FX open items](READINESS-DECISIONS.md)
- [Core flows — billing and currency rules](CORE-FLOWS-AND-DATA.md)
- [Technical specification — billing invoice conversion](TECHNICAL-SPECIFICATION.md)
- [Implementation plan — Phase 0](IMPLEMENTATION-PLAN.md#phase-0--close-planning-and-turn-decisions-into-build-contracts)
