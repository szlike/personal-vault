# eBay Seller Dashboard (System Design Round, Usha Achladi)

**Recorded:** 2026-09-29

## Context

eBay System Design round, real loop round with Usha Achladi, Sep 29. Design a seller dashboard for 500,000 sellers.

## First pass

First pass grade C+. The design had no BFF (backend for frontend) and no write path. The read path was calculated at 0.6 requests per second, while the real load was missed entirely: the 5B-evaluation scheduled job. Doing read-path arithmetic while missing the scheduled job meant the design solved the easy part and ignored the hard part.

## Final drill

Final drill grade B. The A+ list given after the drill: specify the BFF (backend for frontend); state every choice as a trade-off; give a failure mode per component; quantify NFRs (non-functional requirements); flag assumed constants; actually do the deep dive; find the hard scale path first.

## Past mistakes & points to improve

- The scheduled-job and write path is the hard scale path in dashboard problems. Find it before doing read-path arithmetic. A dashboard that looks read-heavy can be dominated by a background job that recomputes values for every seller.
- Specify the BFF (backend for frontend) in the first pass. The dashboard needs an aggregation layer between the clients and the services, and its absence was a visible gap.
- State every choice as a trade-off, with a failure mode per component. A choice without a stated cost and a component without a stated failure mode both read as unexamined.
- Quantify NFRs (non-functional requirements) and flag assumed constants. If a number is assumed, say it is assumed, so the interviewer can correct it instead of discovering it later.
- Actually do the deep dive. After the overview, pick the hard component and go deep on it instead of staying at the high level.
