# clone-lin-llamma-xy

Experimental LIN clone of Curve Finance crvUSD LLAMMA `get_xy_up` / `get_y_up` / `get_x_down` (i64-positive scale: `SCALE=1e9`, one band, 128-bit `muldiv(fx,gy,den)` so `Inv=(f+x)*(g+y)` is not materialized, fail-closed if the quotient does not fit i64).

- **Upstream:** [curvefi/curve-stablecoin](https://github.com/curvefi/curve-stablecoin) `curve_stablecoin/AMM.vy` commit `81b3bea490e9b45221c82e78387920ccfeb05b33`. Curve.Fi publishes that tree as informational-only (no reproduction license); this repository does **not** vendor the Vyper source. Hashes only: sha256 `17700ab4d36b289a0c688259d40b3898421af5360c6da29440af0da363a7d736`, git blob `d14d0329c145d7c5d3e1c61301c93b072184ba02`.
- **LIN kernel:** [`src/lin_llamma_xy.lin`](src/lin_llamma_xy.lin) (sha256 `462f38a142ae84ab482430bdbbfd3c62ab271526f9158fa6b9b5646103f64d4f`, identical to lin-open)
- **C11 oracle copy:** [`test/oracles/llamma_xy_c11.c`](test/oracles/llamma_xy_c11.c)
- **Proofs (lin-open only):** `make llamma-xy-proof` in [kbelludoo/lin-open](https://github.com/kbelludoo/lin-open) `examples/llamma_xy/` and `python3 test/prove_llamma_xy_external.py` (21 PASS / 0 FAIL / 1 SKIP JIT this run)
- **Class:** EXPERIMENTAL. Not Ethereum crvUSD uint256. Not `WAD=1e18` / multi-tick user positions / `p_oracle` storage / `bands_x[n]`. Not a production LLAMMA or Controller replacement.

Distinct from [clone-lin-curve-vp](https://github.com/kbelludoo/clone-lin-curve-vp) (`get_D`/`get_virtual_price`), [clone-lin-curve-gety](https://github.com/kbelludoo/clone-lin-curve-gety), [clone-lin-curve-getdy](https://github.com/kbelludoo/clone-lin-curve-getdy), and [clone-lin-curve-getyd](https://github.com/kbelludoo/clone-lin-curve-getyd).

Named goldens (i64-positive, SCALE=1e9, A=100, p_o_up=1e9): collateral-only `p_o` above band `get_y_up` → **1000000000**; `get_x_down` → **994987437**; naive mark `y*p_o/SCALE` → **1100000000** (adiabatic haircut **105012563**); `DEAD_SHARES=1000` share of that y_up → **499750124**; mixed in-band `get_y_up(1e6,1e6)` → **1995012**; naive i64 `12e9 * 1e9` wraps to **-6446744073709551616**, 128-bit muldiv fail-closes to **0**.
