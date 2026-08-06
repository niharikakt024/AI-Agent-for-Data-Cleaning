
# 🧹 AI Data-Cleaning Agent

A Google Colab notebook that takes **any messy CSV**, profiles it, asks an LLM (Groq / Llama 3.3 70B) to build a per-column cleaning plan, executes that plan, removes duplicates, and hands you back a cleaned file plus a summary report — no hardcoded column names anywhere.

---

## How it works

1. **Upload** — you upload any CSV.
2. **Profile** — every column is scanned for dtype, missing %, uniqueness, outliers, negative values, currency-like text, boolean-like values, and sample values.
3. **Plan** — the profile is sent to Llama 3.3 70B (via Groq), which decides which action(s) to apply to each column and why.
4. **Execute** — the plan is applied column by column using generic, reusable cleaning functions.
5. **Deduplicate** — fully identical rows are dropped.
6. **Report** — a before/after summary is printed and the cleaned CSV is downloaded.

---

## Setup (one-time)

1. Get a free API key at [console.groq.com](https://console.groq.com) → **API Keys** → **Create API Key**.
2. In Colab, open the 🔑 **Secrets** panel (left sidebar) and add a secret named `GROQ_API_KEY` with that key as the value.
3. Run the notebook top to bottom.

---

## Usage

Just run every cell in order:

| Cell | What it does |
|---|---|
| 1 | Installs dependencies (`groq`, `pandas`, `numpy`, `thefuzz`, `python-Levenshtein`) |
| 2 | Connects to Groq |
| 3 | Prompts you to upload a CSV |
| 4–5 | Profiles every column |
| 6–7 | Asks the LLM for a cleaning plan |
| 8–9 | Applies the cleaning plan |
| 10–11 | Removes duplicate rows |
| 12–13 | Generates a report and downloads `cleaned_data.csv` |

No editing required — the same notebook works on any CSV you upload.

---

## Available cleaning actions

The LLM picks from this list for each column, based on its profile:

| Action | What it does |
|---|---|
| `trim_whitespace` | Strips leading/trailing spaces from text |
| `standardize_case` | Fixes inconsistent capitalization (Title Case) |
| `fuzzy_merge_categories` | Merges near-duplicate spellings (e.g. "Paypal" vs "Pay Pal") |
| `parse_dates` | Converts inconsistent/invalid date strings into a standard datetime format — repairs swapped month/day and out-of-range days (e.g. Feb 30) instead of discarding them |
| `clean_currency` | Strips `$` and commas from currency-like text, converts to numeric |
| `fix_invalid_negative` | Converts negative values to absolute value (for columns like quantity/age that can't logically be negative) |
| `remove_negative_sign` | Strips the negative sign from a price/amount column (absolute value) |
| `clip_negative_to_zero` | Clamps negatives to 0 (for columns like discounts/refunds where negative means "none," not a sign error) |
| `cap_outliers_iqr` | Caps extreme outliers to the IQR bounds instead of deleting them |
| `cap_outliers_zscore` | Caps outliers beyond 3 standard deviations from the mean |
| `standardize_boolean` | Converts Yes/No, Y/N, True/False, 1/0 variants into consistent True/False |
| `fill_missing_median` | Fills missing numeric values with the column median |
| `fill_missing_mode` | Fills missing categorical values with the column mode |
| `fill_missing_constant` | Fills missing values with 0 (numeric) or "Unknown" (text) |
| `remove_special_characters` | Strips punctuation/symbols, keeps letters/numbers/spaces |
| `remove_non_ascii` | Strips emojis and non-ASCII characters |
| `standardize_id_format` | Uppercases IDs and removes internal spaces |
| `standardize_email` | Lowercases/trims emails, nulls malformed addresses |
| `standardize_phone` | Strips formatting, keeps last 10 digits |
| `standardize_url` | Lowercases URLs, strips trailing slashes |
| `extract_numeric` | Pulls the numeric value out of mixed text (e.g. "5 units", "3kg") |
| `round_numeric` | Rounds a numeric column to 2 decimal places |
| `drop_column` | Drops a column entirely (e.g. >90% missing or unusable) |
| `none` | No action needed |

---

## Output

- **`cleaned_data.csv`** — the cleaned dataset, downloaded automatically at the end.
- **Console report** — original vs. final row counts, missing values before/after, every action taken per column, and duplicate rows removed.

---

## Notes

- Works on *any* CSV schema — column names are never hardcoded.
- A column can have multiple actions applied in sequence (e.g. `trim_whitespace` → `standardize_case`).
- `drop_column` is destructive — the LLM is instructed to use it sparingly (only near-empty or unusable columns).


## 👨‍💻 Author

**Niharika K T**

**Aspiring Data Analyst | Power BI | SQL | Excel | Python | Data Visualization**

📧 Email: niharikakt024@gmail.com  
🔗 LinkedIn: www.linkedin.com/in/niharika-k-t-8a1a2728a  
💻 GitHub: https://github.com/niharikakt024

---

⭐ If you find this project useful, consider giving the repository a star!
