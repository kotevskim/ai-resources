# jOOQ vs SQLDelight — GitHub Health & Sustainability Analysis

> **Analysis date:** March 22, 2026
> This analysis was completed using Claude Code with the [`github-library-health-comparison-prompt.md`](./github-library-health-comparison-prompt.md) prompt.

---

## Basic Repository Info

| Metric | **jOOQ** | **SQLDelight** |
|---|---|---|
| Stars | 6,681 | 6,774 |
| Forks | 1,229 | 560 |
| Language | Java | Kotlin |
| License | Proprietary (dual-license) | Apache-2.0 |
| Created | Apr 2011 (15 years) | Oct 2015 (10 years) |
| Last push | Mar 20, 2026 | Mar 19, 2026 |
| Archived | No | No |

---

## Commit Activity

| Period | **jOOQ** | **SQLDelight** |
|---|---|---|
| Last year | 753 | 298 |
| Last 6 months | 367 | 166 |
| Last 3 months | 157 | 83 |
| Last month | 53 | 40 |
| Trend | Stable/high | Stable/moderate |

---

## Active Contributors (last 6 months, commits)

| Metric | **jOOQ** | **SQLDelight** |
|---|---|---|
| Total contributors (all-time) | ~111 | ~187 |
| Active committers (last 6mo) | **1** (lukaseder: 367) | **~20** (excl. bots: griffio: 31, JakeWharton: 26, dellisd: 19, AlecKazakova: 6, + others) |
| Human committers last month | 1 (lukaseder: 69) | ~6 (dellisd: 16, griffio: 10, AlecKazakova: 6, JakeWharton: 1, eygraber: 1, madisp: 1) |
| Bus factor | **1 (critical risk)** | **3-4 core + community** |

---

## Issue Health

| Metric | **jOOQ** | **SQLDelight** |
|---|---|---|
| Open issues (total) | ~2,129 | ~363 |
| Issues created (last 6mo) | 684 | 66 |
| Issues created (last month) | 99 | 16 |
| Issues closed (created last 6mo) | 99 | 22 |
| Who creates issues? | **83% by lukaseder** (self-tracked) | Community-driven (vanniktech: 18, others) |
| Open issues w/ 0 comments (last 6mo) | 51 | 9 |
| Avg comments per issue (last 3mo) | 1.22 | 1.64 |
| Total issue comments (last 6mo) | 1,034 | 422 |

> **Note on jOOQ issues:** The ~2,129 open issues and 684 new issues in 6 months look alarming, but **83% are created by Lukas Eder himself** — jOOQ uses GitHub Issues as a feature/task tracker. This is not a sign of neglect; it's a project management style.

### Issue Engagement — Top Commenters (last 6 months)

| **jOOQ** | **SQLDelight** |
|---|---|
| lukaseder: 83 | griffio: 23 |
| o-shevchenko: 4 | renovate[bot]: 19 |
| ThoSap: 3 | AlecKazakova: 13 |
| dstepanov: 2 | dellisd: 11 |
| _mostly 1-off users_ | vanniktech: 11, JakeWharton: 3 |

---

## Pull Request Activity

| Metric | **jOOQ** | **SQLDelight** |
|---|---|---|
| Open PRs | 0 | 4 |
| Closed/merged PRs (last 6mo) | 7 | 99 |
| External contributions via PR | Minimal | Active |

---

## Release Cadence

| Metric | **jOOQ** | **SQLDelight** |
|---|---|---|
| Last release | Jan 30, 2026 (v3.20.11) | Mar 16, 2026 (v2.3.2) |
| Releases in last 6mo | 10 (across 3 maintained versions) | 2 |
| Maintained versions | 3 parallel (3.18, 3.19, 3.20) | 1 (2.x) |
| Release frequency | ~monthly (per version) | ~quarterly |

### Recent Releases

**jOOQ:**
| Version | Date |
|---|---|
| 3.20.11 | Jan 30, 2026 |
| 3.19.30 | Jan 30, 2026 |
| 3.18.37 | Jan 30, 2026 |
| 3.20.10 | Dec 5, 2025 |
| 3.19.29 | Dec 5, 2025 |
| 3.18.36 | Dec 5, 2025 |
| 3.20.9 | Nov 12, 2025 |
| 3.19.28 | Nov 12, 2025 |
| 3.18.35 | Nov 12, 2025 |
| 3.20.8 | Oct 1, 2025 |

**SQLDelight:**
| Version | Date |
|---|---|
| 2.3.2 | Mar 16, 2026 |
| 2.2.1 | Nov 14, 2025 |
| 2.1.0 | May 16, 2025 |
| 2.0.2 | Apr 5, 2024 |
| 2.0.1 | Dec 1, 2023 |
| 2.0.0 | Jul 26, 2023 |

---

## Summary Scorecard

| Category | **jOOQ** | **SQLDelight** | Edge |
|---|---|---|---|
| Project maturity | 15 years, proven | 10 years, proven | jOOQ |
| Commit velocity | Higher (753/yr) | Moderate (298/yr) | jOOQ |
| Bus factor | **1 person (Lukas Eder)** | 3-4 active core devs | **SQLDelight** |
| Community contributions | Very few external PRs | Active PRs from community | **SQLDelight** |
| Issue responsiveness | Fast — maintainer answers most | Good — multiple people engage | Tie |
| Release cadence | Frequent, multi-version support | Slower but steady | jOOQ |
| Open-source license | Dual-licensed (OSS + commercial) | Apache-2.0 (fully open) | **SQLDelight** |
| Long-term sustainability risk | High — single-person dependency | Lower — backed by Cash App / multiple maintainers | **SQLDelight** |

---

## Final Verdict

**Both libraries are actively maintained as of March 2026**, but they present very different risk profiles:

**jOOQ** is essentially a **one-man project**. Lukas Eder is an incredibly prolific and dedicated maintainer — 367 commits in 6 months, answering almost every issue, maintaining 3 parallel versions. The library is mature, battle-tested, and has commercial backing (Data Geekery GmbH). However, the **bus factor of 1 is a serious long-term risk**. If Lukas steps away, the project has virtually no other active contributors to carry it forward. There are almost zero external PRs.

**SQLDelight** has a **healthier contributor distribution** — 3-4 active core developers (griffio, JakeWharton, dellisd, AlecKazakova) plus community contributions. It's backed by Cash App (Block, Inc.), giving it organizational sustainability. The Apache-2.0 license is cleaner for commercial use. The trade-off is lower commit velocity and a slower release cadence.

**For long-term use, SQLDelight carries less risk** due to its distributed maintainership and corporate backing. However, if your project is JVM/server-side Java and you need deep SQL dialect support, jOOQ is functionally superior in that domain — just go in understanding the single-maintainer dependency. If you're doing Kotlin Multiplatform or mobile, SQLDelight is the clear choice.
