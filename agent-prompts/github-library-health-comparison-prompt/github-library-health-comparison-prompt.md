# GitHub Library Health Comparison Prompt

**What this prompt does:** Given two (or more) GitHub repository URLs, this prompt instructs an AI assistant with GitHub API access to perform a comprehensive health and sustainability comparison. It evaluates maintainer activity, contributor distribution, issue responsiveness, release cadence, and long-term risk — then produces a structured comparison table with a final verdict. Ideal for making informed decisions about which open-source library to adopt for long-term use.

---

## Prompt

```
Compare the following GitHub repositories as candidates for long-term adoption in my project:

- [REPO_URL_1]
- [REPO_URL_2]

Perform a deep health and sustainability analysis using the GitHub API. For each repository, gather and compare:

### 1. Basic Repository Metrics
- Stars, forks, watchers, language, license
- Creation date, last push date, archived status

### 2. Commit Activity
- Total commits over the last year, 6 months, 3 months, and 1 month
- Identify the trend (accelerating, stable, declining)

### 3. Active Contributors (critical)
- Total all-time contributors
- Active committers in the last 6 months and last month (with commit counts per person)
- **Bus factor** — how many people would need to leave before the project stalls?
- Are commits concentrated in one person or distributed across a team?

### 4. Issue Health
- Total open vs closed issues (and the ratio)
- Issues created in the last 6 months and last month
- Who creates issues — maintainers (self-tracked tasks) or the community?
- How many recent open issues have zero comments (potential neglect signal)?
- Average comments per issue (engagement level)
- Total issue comments in the last 6 months
- Top issue commenters — are maintainers actively responding?

### 5. Pull Request Activity
- Open PRs count
- PRs closed/merged in the last 6 months
- Are external community PRs being accepted, or is development closed?

### 6. Release Cadence
- Last 10 releases with dates
- How many releases in the last 6 months?
- How many parallel versions are maintained?
- Release frequency pattern (weekly, monthly, quarterly?)

### 7. Licensing & Backing
- License type and implications for commercial use
- Is there a company or organization backing the project?

### Output Format

1. **Structured comparison tables** organized by category (one table per section above)
2. A **summary scorecard table** with one row per category, showing which repo has the edge
3. A **final verdict** paragraph that:
   - Highlights the key differentiators
   - Identifies the biggest long-term risks for each option
   - Gives a clear recommendation with caveats based on use case

Be specific with numbers and dates. Flag any red flags (bus factor of 1, declining activity, license concerns, abandoned issues). Note if a repo uses GitHub Issues as a task tracker (inflating issue counts) vs genuine community-reported issues.
```
