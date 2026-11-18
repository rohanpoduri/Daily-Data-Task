# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Repository state

This is a data-analysis workspace, not a software project. It has no source code, build system, tests, or dependencies yet. It is also not a git repository. The only content is a single dataset in `Data/`. Any analysis code (scripts or notebooks) will be new.

## Dataset: `Data/gender_wage.csv`

US occupation-level earnings by gender, 2013–2016: 2,088 rows, which is 522 occupations × 4 years. Each row is one occupation in one year.

Columns:
- `year`, `occupation`
- `major_category` (8 broad groups, e.g. "Management, Business, and Financial") and `minor_category` (a finer subgroup). Category names contain commas, so they are quoted in the CSV. Use a real CSV parser, not a naive split.
- `total_workers`, `workers_male`, `workers_female`, `percent_female` (0–100 scale)
- `total_earnings`, `total_earnings_male`, `total_earnings_female`: earnings in dollars
- `wage_percent_of_male`: female earnings as a percentage of male earnings (0–100+ scale)

Data quirks to handle:
- Missing values are the literal string `NA`. In pandas this is caught by default; in other readers, set it explicitly.
- `total_earnings_female` is missing in 65 rows and `total_earnings_male` in 4.
- `wage_percent_of_male` is `NA` in 846 rows (about 40%), **including many rows where both male and female earnings are present**. If you need the ratio, recompute it as `total_earnings_female / total_earnings_male * 100` rather than relying on the provided column.
- When aggregating across occupations, weight by worker counts where appropriate. A plain mean of per-occupation ratios treats small and large occupations equally.
