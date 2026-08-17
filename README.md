# Max-Cut as Boolean Quadratic Programming

A cutting-plane study of the **Maximum Cut** problem, solved exactly through Padberg's
**Boolean Quadratic Program** and tightened by three families of facet-defining
inequalities: **triangle**, **clique**, and **odd-cycle** cuts.


## What it does

Max-Cut is NP-hard. This project reformulates it as a Boolean Quadratic Program,
linearises it over the **Boolean Quadric Polytope** (Padberg's C4–C7 constraints), and
then drives the LP relaxation's *integrality gap* to zero with a dynamic cutting-plane
engine — one that runs real **separation oracles** to find only the inequalities the
current fractional solution violates, deepest-first, round by round.

Highlights:

- **Exact odd-cycle separation** (Barahona–Mahjoub shortest-path construction) — the
  family that closes the gap on odd cycles where triangle and clique cuts are blind.
- **Integrality gap driven to 0.00%** across every tested density and graph family.
- **Up to 34× faster** than plain CBC branch-and-bound on random graphs up to 40 vertices,
  while matching the exact optimum.

## Method at a glance

| Stage | What happens |
| --- | --- |
| BQP → MILP | `max Σ wᵢⱼ(xᵢ + xⱼ − 2yᵢⱼ)` with Padberg C4–C7 pinning `yᵢⱼ = xᵢxⱼ` |
| Base LP | relax integrality → upper bound `z_LP ≥ z_IP*` |
| Triangle cuts | T1–T4 facets; close most of the gap on dense/random graphs |
| Clique cuts | strictly stronger from `K5` onward |
| Odd-cycle cuts | close the residual gap from chordless odd cycles (≥ 5) |

## Key results

- Four-vertex example: base LP 12 → **10** (optimum) after one round of triangle cuts.
- Odd cycles `C5–C13`: triangle+clique find **0 cuts**; one odd-cycle cut closes each gap to **0%**.
- Complete graphs `K5–K8`: triangle cuts stall at 11–17%; clique cuts reach **0%**.
- Random `G(n, 0.4)`, `n = 12…40`: exact optimum matched, **3.4×–34.4× speedup** vs branch-and-bound.


## Run it

```bash
jupyter notebook IE802_MINLO.ipynb
```

Run the cells top to bottom to regenerate every table and figure (worked example,
odd-cycle and complete-graph sweeps, convergence trace, density study, graph-family
comparison, and the scaling benchmark). A fixed seed (42) makes all bounds, cut counts,
and gaps deterministic; only the wall-clock timings vary by machine.

## References

1. M. Padberg, "The Boolean quadric polytope: Some characteristics, facets and relatives," *Mathematical Programming*, 45(1–3), 139–172, 1989.
2. F. Barahona and A. R. Mahjoub, "On the cut polytope," *Mathematical Programming*, 36(2), 157–173, 1986.
