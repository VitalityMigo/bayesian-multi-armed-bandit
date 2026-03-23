# Multi-Armed Bandit as a Sequential Learning Problem

A sequential decision problem under uncertainty in which an agent repeatedly selects among several alternatives with unknown expected rewards. The central question is: **how should the agent balance immediate reward against the value of learning, when experimentation itself has a cost?**

---

## Contents

The notebook studies this problem through ten sections, building progressively from naive strategies toward a near-optimal Bayesian decision rule.

1. **Presentation of the Problem** — The bandit is framed as a controlled stochastic process with Bernoulli rewards $X_{i,t} \sim \text{Bernoulli}(\theta_i)$. The objective is to maximize cumulative reward $W_T$, measured against an oracle that always pulls the best arm $\mu^* = \max_i \theta_i$.

2. **Pure Exploitation and Uniform Exploration** — Two symmetric extremes: one commits immediately and never updates, the other explores randomly and never acts on what it learns. Both accumulate linear regret $R_T = \mathcal{O}(T)$.

3. **The Exploration–Exploitation Trade-off** — Every action carries both an immediate value and an informational value. Pulling an uncertain arm can be rational not because it pays more today, but because it improves all future decisions.

4. **A Simple Benchmark: Epsilon-Greedy** — Explore with fixed probability $\varepsilon$, exploit otherwise. Exploration is blind and never phases out — the strategy permanently sacrifices a fixed fraction of pulls even after the best arm is identified.

5. **A Bayesian View of Learning: the Beta-Bernoulli Model** — Rather than tracking a point estimate, the agent maintains a full posterior belief $\mu_i \mid \mathcal{F}_t \sim \text{Beta}(\alpha_i, \beta_i)$ over each arm. The Beta-Bernoulli model yields a closed-form update after every observation, and beliefs collapse around the true $\theta_i$ as pulls accumulate.

6. **Posterior Mean Greedy** — The posterior mean $\mathbb{E}[\mu_i \mid \mathcal{F}_t]$ is used as a decision criterion. Bayesian regularization improves early estimates, but the strategy remains myopic — it ignores uncertainty entirely and is vulnerable to early lock-in.

7. **Thompson Sampling** — At each step, the agent samples $\tilde{\mu}_i \sim \text{Beta}(\alpha_i, \beta_i)$ for each arm and pulls the arm with the highest draw. Exploration emerges naturally from uncertainty: uncertain arms have dispersed posteriors and receive exploratory pulls without any tuning parameter.

8. **Toward Optimality: A Belief-State Approach** — The bandit is reformulated as a dynamic control problem over beliefs. The exact optimal policy solves a Bellman equation that explicitly values both immediate reward and information — but is computationally intractable at $\mathcal{O}(2^{K\tau})$. Thompson Sampling is its best tractable approximation.

9. **Monte Carlo Comparison** — The three structured strategies are stress-tested across four scenarios: easy, hard, misleading start, and many arms. Performance is measured by efficiency $W_t / (t \cdot \mu^*)$ — the fraction of oracle reward captured at each step.

10. **Conclusion** — Thompson Sampling consistently dominates across all scenarios, combining fast convergence, robustness to early noise, and asymptotically optimal regret.

<br>

---

## Key Results

| Strategy | Regret | Exploration | Oracle Convergence |
|---|---|---|---|
| Pure Exploitation | Linear | None | No |
| Uniform Exploration | Linear | Blind | No |
| $\varepsilon$-Greedy | Linear | Fixed rate | No |
| Posterior Mean Greedy | Linear | None | Asymptotic |
| **Thompson Sampling** | **Sublinear** | **Uncertainty-driven** | **Yes** |
