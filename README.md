# Credit VaR: Portfolio Credit Risk Model

A portfolio credit risk model quantifying tail risk on a synthetic 500-borrower corporate 
loan portfolio, using the Vasicek single-factor default correlation model and Monte Carlo 
simulation — the same theoretical foundation underpinning Basel II/III Internal 
Ratings-Based (IRB) regulatory capital calculations.

This project complements [Project 1: IFRS9 ECL Credit Risk 
Model](https://github.com/vishakharaj14-bot/ifrs9-ecl-credit-risk-model), which modelled 
**Expected Loss**. This project models **Unexpected Loss** — the tail-risk buffer banks 
must hold as regulatory capital, beyond what ECL provisioning covers.

## Why Credit VaR, not just ECL

- **Expected Loss (ECL)** answers: *what do we expect to lose, on average?* — it's the 
mean of the loss distribution, and is covered by provisions on the income statement.
- **Credit VaR** answers: *what's the worst loss we could plausibly suffer, at a given 
confidence level?* — it's the tail of the loss distribution, and determines the 
regulatory capital buffer a bank must hold on its balance sheet.

## Methodology

1. **Synthetic Portfolio Construction** — 500 corporate borrowers across 5 sectors 
(Banking, Real Estate, Retail, Technology, Energy), each with a sector-calibrated PD, a 
lognormally-distributed exposure amount, and a sector-specific recovery rate/LGD.

2. **Vasicek Single-Factor Model** — each borrower's simulated asset value is decomposed 
into a systematic factor (Z, shared economic state) and an idiosyncratic factor (ε, 
firm-specific), linked via an asset correlation parameter ρ:
Asset Value = √ρ × Z + √(1 − ρ) × ε
ρ values (12%–24%) are calibrated to Basel III / CRR Article 153 prescribed ranges for 
corporate exposures.

3. **Monte Carlo Simulation** — 10,000 simulated economic scenarios, each generating a 
full-portfolio loss outcome, producing a complete loss distribution.

4. **Credit VaR & Expected Shortfall** — Credit VaR at 99% and 99.9% (percentiles of the 
loss distribution), Expected Shortfall (average loss in the worst 1% of scenarios), and 
Unexpected Loss (the regulatory capital requirement).

5. **Concentration & Stress Testing** — quantifies how sector concentration amplifies 
tail risk, and models portfolio loss under a deliberate severe-recession scenario 
(Z = −3), referencing the type of scenario used in EBA and ECB regulatory stress tests.

## Key Results

| Metric | Value |
|---|---|
| Portfolio size | 500 corporate borrowers, ~€1.07B total exposure |
| Expected Loss | €22.80M |
| Credit VaR (99%) | €118.26M |
| Credit VaR (99.9%) — Basel IRB standard | €168.91M |
| Expected Shortfall (99%) | €140.93M |
| Unexpected Loss (99.9%) | €145.65M (~6.3x Expected Loss) |

**Concentration risk:** shifting Real Estate to 80% of portfolio exposure (holding 
individual PDs and correlations constant) increased Expected Loss by only 14.4%, but 
increased VaR, Expected Shortfall, and Unexpected Loss by 35–40% — demonstrating that 
concentration risk is invisible to average-PD-based approaches and only detectable 
through correlation modelling.

**Stress testing:** a severe fixed-scenario shock (Z = −3) produced defaults in 27.6% of 
the portfolio (vs. ~4% baseline average PD) and a total loss of €161.4M — 7.1x the 
baseline Expected Loss. Sector-level default rates under stress ranked almost exactly by 
asset correlation (ρ), with Real Estate (ρ = 0.24) showing the sharpest amplification.

## Irish/EU Regulatory Relevance

- ρ values calibrated to Basel III / CRR Article 153 — the same framework used by AIB 
and Bank of Ireland for IRB capital calculations.
- Sector composition reflects Irish bank portfolio concentrations, including historically 
significant real estate exposure (a key driver of the 2008–2013 Irish banking crisis).
- Stress scenario design references EBA macro-adverse and ECB climate stress test 
methodology.

## Tools

Python (NumPy, pandas, SciPy, Matplotlib, Seaborn), Jupyter Notebook

## Author

Vishakha Raj — Incoming MSc Financial Risk Management, Trinity College Dublin (Sept 2026). 
Former Audit Associate, KPMG Singapore (Financial Services).
