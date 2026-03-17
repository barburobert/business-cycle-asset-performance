# OECD CLI Business Cycle Phases and Cross-Asset Performance

## Project overview

This project analyzes whether major financial asset classes exhibit different return patterns across business cycle phases identified using the OECD Composite Leading Indicator (CLI) for the United States. The core idea is that the business cycle matters for asset allocation because macroeconomic conditions influence expected earnings, discount rates, inflation expectations, monetary policy, credit conditions, and investor risk appetite. If assets systematically perform differently across macroeconomic regimes, then business cycle indicators may provide useful inputs for tactical asset allocation.

The project builds a four-phase classification framework based on the level and slope of the OECD CLI and then evaluates the performance of several asset classes across those phases using daily financial data aligned to a realistic release-timing rule.

---

## Motivation

Business cycles are recurrent fluctuations in aggregate economic activity around trend. In practical terms, they reflect alternating periods of strengthening and weakening growth dynamics. These fluctuations affect corporate revenues, margins, financing conditions, interest rates, commodity demand, inflation expectations, and cross-asset relative performance. For this reason, business cycle analysis has long been relevant not only in macroeconomics but also in portfolio construction and tactical asset allocation.

A large part of the asset allocation literature starts from the premise that risky assets tend to perform better when growth is improving or above trend, while defensive assets tend to be relatively stronger when the economy weakens. This intuition motivates the use of regime-based frameworks in which asset allocation decisions are conditioned on macroeconomic states rather than based on static long-run averages alone.

In that context, leading indicators are especially relevant because financial markets are forward-looking. Investors do not react only to the current state of the economy, but also to inflection points and changes in direction. Composite leading indicators are designed precisely to capture such shifts early.

---

## Related literature and conceptual background

The project is motivated by two broad strands of literature.

First, the business cycle literature emphasizes that fluctuations in economic activity are systematic, economically meaningful, and relevant for financial markets. Business cycle turning points influence the relative attractiveness of equities, bonds, commodities, real estate, and defensive assets. A regime-based approach to asset allocation therefore has a strong theoretical basis.

Second, the leading indicator literature argues that composite indicators may provide useful information about cyclical turning points before they are visible in coincident macroeconomic data. In particular, the OECD Composite Leading Indicator is designed to signal turning points in economic activity relative to trend. The project is also conceptually related to practical market research that uses leading indicators, including OECD CLI-based frameworks, to motivate tactical asset allocation or regime-aware portfolio tilts. This includes applied investment commentary and research such as work by Invesco on business cycle allocation and broader discussions of asset behavior across macro regimes, as well as academic and practitioner work associated with regime frameworks in the spirit of growth and cycle-sensitive allocation approaches.

The main purpose here is not to replicate a single published paper exactly, but to test a transparent and economically interpretable framework: whether a simple phase-classification rule based on the OECD CLI level and its monthly change produces meaningful differences in cross-asset performance.

---

## Research objective

The main objective of the project is to test whether broad asset classes display distinct return behavior across business cycle phases identified using the OECD CLI for the United States.

More specifically, the project aims to:

1. construct a transparent four-phase business cycle framework using the OECD CLI,
2. align the macro signal with realistic investable dates,
3. compute asset returns over the periods during which each phase is active,
4. compare asset performance across phases,
5. assess whether the results are economically plausible and useful for tactical asset allocation.

---

## Hypothesis

The working hypothesis is that asset returns are regime-dependent.

In particular:

- equities and real estate should perform better in recovery and expansion phases,
- commodities should be relatively stronger in expansion,
- bonds should be more resilient in slowdown and contraction,
- gold should display defensive properties, especially in contractionary conditions.

If these patterns emerge consistently, the OECD CLI may be useful as a practical regime signal in tactical asset allocation.

---

## Data

### 1. OECD CLI data

The macroeconomic regime signal is based on the OECD Composite Leading Indicator for the United States.

**Series name:**  
Leading Indicators OECD: Leading Indicators: Composite Leading Indicator: Amplitude Adjusted for United States

**Series code:**  
USALOLITOAASTSAM

**Reference area:**  
United States

**Frequency:**  
Monthly

**Measure:**  
Composite Leading Indicator (CLI)

**Calculation methodology:**  
OECD harmonised

**Unit of measure:**  
Index, amplitude adjusted

The OECD CLI is an amplitude-adjusted, harmonized leading indicator designed to signal turning points in the business cycle relative to trend. The neutral benchmark is 100:
- values above 100 indicate above-trend conditions,
- values below 100 indicate below-trend conditions.

The project uses the latest available revised version of the series rather than real-time vintage data. This is an important limitation because the OECD revises the series retrospectively. Therefore, the results should be interpreted as an ex post regime-performance analysis rather than a strict real-time out-of-sample backtest.

### 2. OECD CLI component series for the United States

The US CLI is a country-specific composite indicator constructed from several leading variables selected by the OECD for their ability to anticipate turning points in US economic activity.

| Component series | Source |
|---|---|
| Consumer survey - confidence indicator sa (normal = 100) | Survey of Consumers – University of Michigan |
| Manufacturing survey - confidence indicator (% balance) | US Institute of Supply Management |
| Work started for dwellings sa (number) | Bureau of the Census |
| Net new orders - durable goods sa (USD) | Bureau of the Census |
| Share prices: NYSE composite (OECD base year = 100) | New York Stock Exchange |
| Weekly hours worked: manufacturing sa (hours) | Bureau of Labor Statistics |
| Spread of interest rates (% p.a.) | Federal Reserve |

These components capture different dimensions of cyclical dynamics: consumer confidence, manufacturing sentiment, housing activity, industrial demand, equity market conditions, labor input intensity, and the slope of interest rates. Taken together, they provide a broad leading signal for business cycle turning points.

---

## Financial asset data

The project uses daily financial data for the following asset classes:

- SPY: US equities
- AGG: US bonds
- GLD: gold
- DBC: broad commodities
- VNQ: listed real estate

Daily adjusted closing prices are used in order to reflect total investor return more accurately than raw closing prices, accounting for splits and distributions where relevant.

The use of daily data is important because the OECD CLI is monthly, while financial markets trade continuously. A purely calendar-month mapping would create timing distortions. The daily series allow the analysis to align asset performance with the date on which the regime signal becomes usable.

---

## Data processing and phase construction

### 1. Building the business cycle phases

The project constructs four business cycle phases using two dimensions of the OECD CLI:

- the level of the indicator relative to 100,
- the monthly slope of the indicator.

The slope is defined as the month-over-month change in the CLI:

\[
\text{slope}_t = CLI_t - CLI_{t-1}
\]

Using these two dimensions, the phases are defined as follows:

- **Expansion**: CLI >= 100 and slope > 0
- **Slowdown**: CLI >= 100 and slope < 0
- **Recovery**: CLI < 100 and slope > 0
- **Contraction**: CLI < 100 and slope < 0

This framework is simple, transparent, and economically interpretable. The level captures whether the economy is above or below trend, while the slope captures whether cyclical momentum is improving or deteriorating.

### 2. Processing the OECD data in Excel

The CLI data were first organized in Excel with the following columns:

- `observation_date`
- `CLI`
- `slope`
- `phase`

The slope was computed as the month-over-month change in the indicator, and the phase variable was assigned based on the classification rules above. This pre-processed dataset was then imported into Python for visualization, descriptive analysis, phase-transition analysis, and asset-performance evaluation.

### 3. Release-timing alignment

A critical methodological issue is that the OECD CLI is not observable to investors at the beginning of the reference month. The information becomes available only after publication.

To approximate this timing realistically, the project assumes that each CLI observation becomes available around the middle of the following month. Specifically:

- for each CLI observation in month \( t \),
- a `target_date` is defined as the 15th day of month \( t+1 \),
- if the 15th is not a trading day, the phase becomes active on the first available trading day after that date,
- this is called the `effective_date`.

Each phase remains active until the next phase `effective_date`.

This procedure creates investable holding intervals that are more realistic than assigning the signal directly to the same calendar month in which the observation is recorded.

### 4. Calculating asset returns by phase

For each phase interval, the project computes the holding-period return of every asset:

\[
R = \frac{P_{end}}{P_{start}} - 1
\]

where:
- \( P_{start} \) is the adjusted close at the start of the active phase interval,
- \( P_{end} \) is the adjusted close immediately before the next phase begins.

This creates a phase-level return dataset that can then be grouped by regime and compared across assets.

---

## Descriptive analysis

The project includes several descriptive steps:

- summary statistics for the CLI level and slope,
- histograms for CLI and slope,
- distribution of observations across business cycle phases,
- observations around the key thresholds \( CLI = 100 \) and \( slope = 0 \),
- transition matrices across phases,
- visualization of the full CLI series with shaded regime periods.

The transition analysis shows strong phase persistence and economically coherent movement across regimes. Expansion tends to move into slowdown, slowdown tends to move into contraction more often than back to expansion, recovery tends to transition into expansion, and contraction transitions into recovery. This supports the internal consistency of the classification framework.

---

## Main empirical results

The cross-asset results are broadly consistent with standard business cycle intuition.

The phase-level average returns indicate that:

- **SPY** performs best in **Recovery**, followed by **Expansion**,
- **VNQ** also performs best in **Recovery**,
- **DBC** performs best in **Expansion**,
- **GLD** performs best in **Contraction**,
- **AGG** remains positive across all phases and is relatively strongest in **Contraction**.

These patterns are economically plausible:

- equities and real estate tend to benefit from improving growth expectations,
- commodities tend to perform best when growth is above trend and demand is firm,
- bonds are more resilient in weaker macro regimes,
- gold behaves as a defensive asset during contractionary conditions.

Overall, the results suggest that the OECD CLI phase framework captures meaningful macro-financial regime differences.

---

## Interpretation for tactical asset allocation

The findings provide a reasonable motivation for using the OECD CLI as an input in tactical asset allocation.

A regime-aware investor could interpret the evidence as supporting:
- a more pro-cyclical tilt toward equities, real estate, and commodities during recovery and expansion,
- a more defensive tilt toward bonds and gold during contraction,
- a more selective or cautious positioning during slowdown.

This does not mean the indicator should be used mechanically or in isolation. However, it suggests that business cycle phase information may improve the economic context for asset allocation decisions relative to a purely static portfolio approach.

---

## Limitations

The project has several important limitations.

1. **No vintage data**  
   The analysis uses the latest revised OECD CLI series, not the real-time values that were historically available to investors.

2. **Release-date approximation**  
   The use of the 15th of the following month is a proxy for the release schedule, not an exact historical release calendar for every observation.

3. **ETF sample limitations**  
   The asset analysis is constrained by ETF availability, which shortens the usable sample relative to the full CLI history.

4. **Ex post regime analysis**  
   The results are descriptive and regime-based, but not yet a full out-of-sample tactical allocation backtest with transaction costs, rebalancing assumptions, or formal portfolio optimization.

5. **Simple phase framework**  
   The level-slope classification captures business cycle direction and position, but it does not explicitly control for inflation shocks, policy asymmetries, or financial stress regimes.

---

## Conclusion

This project shows that a simple phase-classification framework based on the OECD Composite Leading Indicator for the United States can generate economically interpretable differences in cross-asset performance.

The results are broadly consistent with the literature and market intuition:
- cyclical assets such as equities, real estate, and commodities tend to perform better in improving or above-trend environments,
- defensive assets such as bonds and gold tend to be relatively stronger in weaker macro regimes.

Although the analysis is not based on real-time vintage data and should therefore be interpreted with caution, it provides useful evidence that business cycle phases identified through the OECD CLI may serve as a practical input for regime-aware asset allocation.

---

## Repository contents

- `README.md` – project description and methodology
- notebook / script files used for the analysis
- processed OECD CLI phase dataset
- financial asset dataset
- figures generated in the analysis

---

## Reproducibility

To reproduce the analysis:

1. import the processed OECD CLI phase dataset,
2. import the daily asset price dataset,
3. construct the CLI phase intervals using the 15th-of-next-month release proxy,
4. compute holding-period returns for each asset over each active phase interval,
5. group the results by phase and compare mean returns across regimes.

---

## Author note

This project is intended as an applied macro-financial regime analysis rather than a fully optimized trading strategy. Its main contribution is to test whether a transparent and intuitive OECD CLI phase framework produces economically meaningful differences in cross-asset behavior.
