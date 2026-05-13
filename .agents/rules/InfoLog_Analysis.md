# Info.log File Structure Analysis

This document outlines the structure, parsing rules, and rendering logic for `Info.log` files, based on the implementation in `InfoLogModal.tsx`.

## 1. General Structure
`Info.log` is a text file that follows a custom **Section-based** format, similar to INI files but with specific variations.

-   **Sections**: Start with `[SECTION_NAME]`.
-   **Content**: Lines following a section header belong to that section until the next header or EOF.
-   **Data Types**: Content can be key-value pairs (KV), nested KVs (`nested_kv`), or a simple list of strings.

### Example
```ini
[REVISION_INFO]
	TIME	2026/02/06 16:39:49
	MAIN	2021/04/16/0000,Scheduler Program 6.3...

[MODULE_INFO]
	MODE 	[11   :0112        :  :11  :1   :1]
    ...
```

## 2. Parsing Logic
The parser identifies sections and then determines the content type for each section.

### Section Detection
-   Regex: `^\[(.+)\]`
-   Captures the text inside brackets as the `Section Name` (and ID).

### Content Parsing (Heuristics)
For each section, the content lines are analyzed to determine if they represent Key-Value pairs (`kv`) or a raw list (`list`).

#### Key-Value (KV) Detection
A section is treated as `kv` if lines follow these patterns:
1.  **Tab Separated**: `KEY \t VALUE`
    -   Example: `TIME	2026/02/06`
2.  **Equals Separated**: `KEY=VALUE`
    -   Example: `[1]=1` or `id=123`
3.  **Special Handling**: `LOT_INFO` sections often contain comma-separated KVs on a single line (e.g., `LOGFOLDER=..., GROUP=...`). The parser splits these lines by comma and then by equals sign to extract multiple KVs.

#### List Detection
If content lines do not match KV patterns (e.g., they are just log messages or data arrays without keys), the section is treated as a `list` type.

#### Nested Key-Value Detection
Specific sections like `LOT_INFO` contain sub-headers (e.g., `Parameter`, `Mdl_Use_Flag`) which group KV pairs.
-   **Structure**:
    ```text
    SubsectionName
        KEY=VALUE
        KEY2=VALUE2
    ```
-   **Parsing**: Lines starting with indentation but no `=` are treated as sub-headers. Subsequent lines with `=` are parsed as KVs belonging to that sub-header.

## 3. Visualization Rules

### Key-Value Sections
-   **Display**: Two-column table.
-   **Key**: Left column, muted color.
-   **Value**: Right column, monospace font. Values containing commas may be split into multiple lines for readability.

### List Sections
-   **Display**: Monospace text block.
-   **Styling**: Preserves whitespace and formatting.

### Nested Key-Value Sections
-   **Display**: Grouped by sub-headers.
-   **Structure**: 
    -   **Sub-header**: Bold, blue text with separator.
    -   **Content**: Standard KV table for each group.

## 4. Key Sections
Common sections found in `Info.log`:

| Section Name         | Description                               | Format    |
| :------------------- | :---------------------------------------- | :-------- |
| `[REVISION_INFO]`    | System revision, time, and user info      | KV        |
| `[MODULE_INFO]`      | Mode and status of modules                | KV        |
| `[SYSTEM_OPTION]`    | System configuration flags                | KV        |
| `[ALL_MDL_RUN_FLAG]` | Run flags for modules                     | KV        |
| `[PRIORITY]`         | Task priority settings                    | KV        |
| `[PRIORITY]`         | Task priority settings                    | KV        |
| `[LOT_INFO=x]`       | Detailed lot parameters and schedule data | Nested KV |

## 5. Implementation Reference
The parsing logic is implemented in the frontend component:
-   **File**: `frontend/components/dashboard/InfoLogModal.tsx`
-   **Method**: `parsedData` (useMemo hook)
