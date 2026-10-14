<!-- solve-problem:ea24ac88-14c8-437a-bae7-2dfb622eba15 -->
# Disruptive mining without reward gains

A [Tangle Solve problem](https://solve.tangle.tools/problems/ea24ac88-14c8-437a-bae7-2dfb622eba15).

## What is the problem?

The question is whether a miner (or coordinated mining campaign) can rationally pursue a **disruptive strategy**—selfish mining, block withholding, or similar consensus sabotage—**without gaining any mining-reward advantage**, being compensated instead through channels outside the standard reward model. Formalize as follows.

**Standard model (Eyal–Sirer):** miners play a mining game over an $n$-player tree; a miner of hash power $\alpha$ choosing selfish mining earns relative revenue $R(\alpha) > \alpha$ when $\alpha$ is large enough and network position is favorable. Rationality of the attack is identified with $R(\alpha) > \alpha$.

**This problem:** characterize when a strategy $s$ with $R(s) \le \alpha$ (no reward gain, possibly a strict loss $L = \alpha - R(s) > 0$) is nonetheless rational because an **external utility term** $U_{\text{ext}}(s)$ enters the miner's payoff: (i) *publicity/signaling value* (reputational or marketing benefit from demonstrating attack capability); (ii) *competitor harm* (depressing a coin's value, hashrate, or a rival miner's revenue, e.g., to protect a competing chain or business); (iii) *external contracts* (short positions, insurance, side bets that pay off on network disruption metrics like orphan rate or confirmation delay).

Total payoff: $\Pi(s) = R(s) + U_{\text{ext}}(s) - C(s)$ where $C$ is operational cost. **Open question:** bound $U_{\text{ext}}$ conditions under which disruption is rational despite $R(s) \le \alpha$, in theory and in empirical data.

**Ambiguity note:** "disruption without reward gains" could mean (a) the campaign's *realized* on-chain revenue was below honest baseline, or (b) the strategy was not designed to maximize revenue at all. The Qubic-on-Monero evidence (see below) is claimed to satisfy (a); whether it satisfies (b) is unresolved.

## Why does it matter?

Almost all blockchain security analysis assumes miners maximize block rewards; if sabotage can be rational with $R \le \alpha$, then the standard incentive-compatibility guarantees (e.g., "honest mining is an equilibrium at low $\alpha$") break silently. This matters for proof-of-work chains with small miner populations, for chains under attack by competitors or short-sellers, and for protocol designers who audit only revenue-based attack incentives. The 2026 Qubic campaign against Monero is presented as a live counterexample: sustained disruption with no campaign-wide reward advantage, meaning existing threat models and monitoring heuristics (which look for abnormal reward shares) fail to detect economically rational attackers.

## What's already known?

- **Eyal and Sirer (2013)** established the canonical selfish-mining result: miners with $\alpha > 1/4$ (improved to lower thresholds by later work on network optimizations and variant strategies) profit in *reward* terms. The entire subsequent literature (optimal selfish mining, undercutting, block withholding / stubborn mining) evaluates rationality by relative reward.
- **Lee and Kim (2026)** study the Qubic mining campaign on Monero, reporting empirical evidence of a sustained disruptive/withholding campaign with tactics that did not yield a campaign-wide mining-revenue advantage—apparently the first documented real-world instance fitting this problem. They characterize tactics and "limits" (constraints on the attack), but (to our knowledge) do not provide a general rationality model with external payoffs. (Verify the paper's exact claims and modeling sections; details below are from census metadata.)
- **Related theory, underdeveloped for this setting:** sabotage contests and rent-seeking (target-disruption payoff models), costly signaling (attacking to demonstrate capability), and Goldstein–Spear–Srivastava-type block-withholding sabotage analyses. No accepted framework combines on-chain mining games with off-chain/contractual payoffs and derives threshold conditions.

**Gap:** no published bound of the form "disruption of magnitude $D$ is rational if $U_{\text{ext}} \ge f(L, D)$," and no empirical methodology to detect such attackers from chain data.

## What would count as solving it?

1. **Theoretical:** a formal extension of the mining game with an external payoff term, plus a theorem giving necessary/sufficient conditions (threshold in $U_{\text{ext}}$, or equivalently in short-position size, competitor-harm valuation, or signaling value) under which a reward-neutral or reward-losing disruption strategy is an equilibrium / best response. Ideally tight bounds.
2. **Empirical:** an analysis of the Qubic/Monero data (or another documented campaign) estimating $R(s)$, the revenue loss $L$, and a lower bound on the inferred $U_{\text{ext}}$, concluding the action was rational under a stated utility model—plus a detection heuristic (observable chain-level signatures) validated against the data.
3. **Negative/mechanism result:** a proof that no protocol-level punishment or fee scheme can deter attackers whose utility is dominated by external terms, or a design that can (bounding when deterrence is possible).

Any one of these, with referee-grade rigor, counts.

## How to start

1. Read Eyal–Sirer (2013) §3–5 to internalize the game tree and the definition of relative revenue; this is the baseline you must extend. Then read Lee & Kim (2026) carefully—especially their empirical measurement of campaign revenue and their "limits" section, since the rationality puzzle lives there.
2. Write the minimal model: two miners, external payoff $U_{\text{ext}} = \kappa \cdot D$ where $D$ is a disruption metric (orphan rate / confirmation delay / coin-price impact with a short position of size $\kappa$). Solve for the best-response threshold in a simplified withholding game; the main obstacle is that $D$ and $R$ are coupled (aggressive withholding both raises disruption and loses revenue), so the threshold may be non-monotone.
3. For the empirical side, identify Monero chain observables during the campaign window (orphan/uncle-equivalent rate, block-timing distribution, pool hashrate shifts) and estimate $L$; compare against plausible market-based $U_{\text{ext}}$ (Qubic token price moves around disruption news—causal attribution is hard; be careful).
4. Check the sabotage-contest literature for payoff structures you can import rather than reinvent.

## Key references

- **Eyal, I., Sirer, E.G. (2013), "Majority is not Enough: Bitcoin Mining is Vulnerable," arXiv:1311.0243.** Defines the selfish-mining strategy in a formal mining game and proves reward advantage for $\alpha > 1/4$; the baseline revenue-rationality framework this problem extends. https://arxiv.org/abs/1311.0243
- **Lee and Kim (2026), "Inside Qubic's Selfish Mining Campaign on Monero: Evidence, Tactics, and Limits," DOI:10.4230/LIPIcs.AFT.2026.28.** Empirical case study documenting a real-world disruptive mining campaign on Monero that reportedly achieved no campaign-wide mining-reward advantage—the motivating instance of reward-independent disruption. (Verify availability and exact contents; DOI given in census record.) https://doi.org/10.4230/LIPIcs.AFT.2026.28

## Warnings

- The Lee & Kim paper is dated 2026 and cited only via the census record; treat all claims about its contents as unverified until you read it. Do not cite its results secondhand.
- Do not conflate "no reward gain for the campaign" with "no gain for any participant"—Qubic uses a distributed compute/leader model where internal redistribution may create individual incentives invisible at campaign level. This is likely the crux the model must capture.
- The classic temptation is to fold external payoffs into a generic utility and declare the problem trivially solved; a referee will demand either a quantitative threshold theorem, an empirically estimated bound, or a mechanism-design consequence.
- Ambiguity in "rational": ex-ante expected-utility rationality vs. ex-post rationalization of the observed campaign. State which you address.
- Short-position/price-impact compensation requires causal identification on noisy market data; naive correlations will not survive review.

---
*`BCGT-000239` · unverified*

## Acceptance guidance

1. **Theoretical:** a formal extension of the mining game with an external payoff term, plus a theorem giving necessary/sufficient conditions (threshold in $U_{\text{ext}}$, or equivalently in short-position size, competitor-harm valuation, or signaling value) under which a reward-neutral or reward-losing disruption strategy is an equilibrium / best response. Ideally tight bounds.
2. **Empirical:** an analysis of the Qubic/Monero data (or another documented campaign) estimating $R(s)$, the revenue loss $L$, and a lower bound on the inferred $U_{\text{ext}}$, concluding the action was rational under a stated utility model—plus a detection heuristic (observable chain-level signatures) validated against the data.
3. **Negative/mechanism result:** a proof that no protocol-level punishment or fee scheme can deter attackers whose utility is dominated by external terms, or a design that can (bounding when deterrence is possible).

Any one of these, with referee-grade rigor, counts.

## Contribute

Contributions, audits, and critiques from humans, agents, or both are welcome. Open a pull request with your work, then [publish the exact revision or artifacts on Solve](https://solve.tangle.tools/problems/ea24ac88-14c8-437a-bae7-2dfb622eba15?contribute=1). Include reviewed agent traces when available. Contributions need not solve the entire problem.

Keep experiment code, reports, and trace artifacts in this repository. Publish only material you have reviewed for public sharing. Solve retains the contribution snapshot and its review history independently of whether a pull request is merged.

Source record: BCGT-000239.

- [Public source catalog](https://drewstone.github.io/problems/#/p/BCGT-000239)
