# Data Cleaning Rules

## Overview

This document describes all the rules applied by the DataClean Excel Data Cleaner tool.

---

## STEP 1 — DATA CLEANING & NORMALIZATION

### 1.1 Whitespace Trimming
- **Rule**: Remove all leading and trailing whitespace from every cell value.
- **Scope**: All columns, including notes/comments columns.
- **Example**: `"  John Doe  "` → `"John Doe"`

### 1.2 Casing Normalization
- **Rule**: For text columns with inconsistent casing, normalize to lowercase.
- **Scope**: Text columns only. Excludes: email columns, notes/comments columns, protected columns.
- **Example**: `"YES"`, `"Yes"`, `"yes"` → `"yes"` (for non-boolean text)
- **Excluded columns**: Any column whose name contains: note, notes, comment, comments, remark, remarks, description, details, additional
- **Excluded columns**: Any column whose name contains: email, e-mail

### 1.3 Missing Value Handling
- **Rule**: Empty cells are flagged as missing values. The suggested value is NULL (no placeholder added by default).
- **Scope**: All non-protected columns.
- **Note**: The user can choose to apply or skip missing value flagging per cell.

### 1.4 Date Format Standardization
- **Rule**: Detected date values are reformatted to ISO 8601 format: `YYYY-MM-DD`.
- **Input formats detected**: DD/MM/YYYY, DD-MM-YYYY, DD.MM.YYYY, YYYY-MM-DD, YYYY/MM/DD, plain text date strings.
- **Output format**: `YYYY-MM-DD` (ISO standard, stored as string in Excel).
- **European display**: The upload/preview uses European format DD/MM/YYYY for display purposes.
- **Scope**: Columns where ≥70% of non-empty values look like dates.
- **Note**: If a column type is not detected as date but contains date-like strings, the values are still reformatted to YYYY-MM-DD.

### 1.5 Number Format Normalization
- **Rule**: Numbers are normalized to use a period as decimal separator internally. No thousands separator is added.
- **European format**: Values with comma as decimal separator (e.g., `"3,14"`) are detected and converted.
- **Display**: Decimals use comma as separator (European format).
- **Rule**: Do NOT add dot (.) as thousands separator.
- **Example**: `"1.234,56"` → `"1234,56"` (removes thousands dot, keeps decimal comma)

### 1.6 Duplicate Detection
- **Row-level**: Rows where all cell values (trimmed) are identical are flagged as duplicates.
- **Column-level**: Column pairs where all values are identical are flagged.

### 1.7 Inconsistent Value Detection
- **Rule**: Text values that differ only in casing (e.g., `"Active"`, `"active"`, `"ACTIVE"`) are flagged as inconsistent.
- **Suggested fix**: Normalize to lowercase.
- **Scope**: Excludes email columns and notes/comments columns.

### 1.8 Boolean / Yes-No Normalization
- **Rule**: Values that semantically mean Yes, No, or N/A are normalized to a standard three-value set.
- **Standard values**: `Yes`, `No`, `N/A`
- **Mapped variants**:
  - Yes: yes, y, true, 1, oui, si, da
  - No: no, n, false, 0, non
  - N/A: n/a, na, not applicable, -, none, null, unknown
- **Scope**: Columns where ≥70% of values are boolean-like.

---

### 1.9 Email Normalization to Lowercase
- **Rule**: All values in email columns are converted to lowercase.
- **Scope**: Columns whose name contains: email, e-mail, e_mail.
- **Example**: `"John.DOE@Company.COM"` → `"john.doe@company.com"`
- **Note**: Email casing is never modified in the inconsistent-value rule — only this dedicated rule applies.

### 1.10 People Name — Special Character Cleanup
- **Rule**: If a special character appears in a name field with NO whitespace immediately after it, the special character is removed and replaced with a space.
- **Scope**: Columns whose name contains: name, firstname, lastname, fullname, surname, given name, middle name, person, contact name.
- **Preserved characters**: Hyphens (`-`) and apostrophes (`'`) are kept as they are valid in names (e.g. "Mary-Jane", "O'Brien").
- **Removed characters**: `.`, `,`, `;`, `/`, `\`, `|`, `_`, `*`, `#`, `@`, `!`, `$`, `%`, etc.
- **Example**: `"John.Doe"` → `"John Doe"`, `"Smith,Mary"` → `"Smith Mary"`

### 1.11 People Name — Capitalize First Letters
- **Rule**: If a name value is entirely lowercase, capitalize the first letter of each word.
- **Scope**: Same name columns as rule 1.10.
- **Only applied when**: The entire name value is lowercase (preserves existing mixed-case values like "McDonald").
- **Example**: `"john doe"` → `"John Doe"`, `"mary ann smith"` → `"Mary Ann Smith"`

### 1.12 City / Country — Capitalize First Letter
- **Rule**: Capitalize the first letter of each word in city or country values.
- **Scope**: Columns whose name contains: city, country, state, province, region, location, town, municipality, territory, nation, county, district.
- **Example**: `"new york"` → `"New York"`, `"united states"` → `"United States"`

### 1.13 City / Country — Abbreviation Expansion
- **Rule**: Common geographic abbreviations are expanded to their full names.
- **Scope**: Same geo columns as rule 1.12.
- **Coverage**: All ISO 3166-1 alpha-2 country codes + common aliases (US/USA, UK/GB, UAE, etc.) for 80+ countries.
- **Example**: `"US"` → `"United States"`, `"UK"` → `"United Kingdom"`, `"DE"` → `"Germany"`, `"FR"` → `"France"`

---

## STEP 2 — PROTECTED COLUMNS

The following columns are NEVER modified, regardless of their content:
- `Submission ID`
- `ID`

These columns are displayed in the UI with a [protected] badge and highlighted in amber.

---

## STEP 3 — COLUMN CLASSIFICATION

Columns are classified into the following types:
- **text**: Predominantly string values
- **number**: Predominantly numeric values
- **date**: Predominantly date-formatted values (≥70% match)
- **boolean**: Predominantly Yes/No/N/A type values
- **mixed**: Combination of types
- **empty**: No values

Notes/comments columns (detected by name keywords) receive trimming only — no casing normalization or boolean detection.

---

## STEP 4 — SELECTION & PREVIEW

- The user selects which issues to apply via checkboxes.
- All issues are pre-selected by default.
- Changes can be filtered by type or column.
- A side-by-side comparison is shown before final export.

---

## STEP 5 — APPLY TRANSFORMATIONS

- Selected transformations are applied to the dataset.
- Original data is preserved in memory for comparison.
- An undo/reset button is available before export.

---

## STEP 6 — EXPORT DATA DICTIONARY

Exported as Excel (.xlsx) with the following structure:

| Column Name | Column Index | Original Value | Mapped Value | Rule Applied |
|-------------|--------------|----------------|--------------|--------------|
| Status      | 3            | YES            | Yes          | Normalized boolean/Yes-No value to standard form (Yes/No/N/A) |
| Created At  | 5            | 01/03/2024     | 2024-03-01   | Standardized date to YYYY-MM-DD (ISO 8601) |

---

## UX RULES

- Step-by-step wizard layout with progress indicator
- All steps clearly labeled (Upload → Analyze → Review → Compare → Apply → Export)
- Clear error messages for unsupported file types
- Preview before applying changes (side-by-side comparison)
- Download buttons clearly visible
- Responsive design for all screen sizes
- Sunny/warm color palette (amber, orange tones) — no dark mode
- 100% browser-based — no data sent to a server
