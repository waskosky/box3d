# Downstream engine maintenance

- `origin` is `https://github.com/waskosky/box3d.git`; `upstream` is `https://github.com/erincatto/box3d.git` for review and synchronization, not a release gate.
- Maintain validated native fixes directly in this fork. Do not wait for upstream acceptance or silently restore an upstream version that drops downstream corrections.
- Keep `main` a tested baseline and preserve shared history. Review upstream updates, merge or cherry-pick deliberately, and retain focused regressions for local fixes.
- Keep engine changes independent of any adapter or private consumer. Do not expose private project names, code, links or deployment configuration.
- The CCD impact-to-response correction lives in `src/distance.c` and `src/shape.c`; keep it covered by `CCDResponseTest`. The separate shape-cast target and sensor mesh rest offset must remain distinct.
- Changed physics may require new determinism goldens. Keep the golden, worker-count and SIMD equality assertions enabled; verify new values across the supported CI matrix rather than bypassing assertions.
- Consumers should pin a tested full commit from this fork and qualify matching artifacts. Old embedded libraries/templates do not change when source or a nearby archive is updated.
- Upstream submissions are separate from downstream maintenance. Follow upstream's contribution policy and obtain user authorization before posting issues, comments or PRs.
