# LotData Log Analysis Rules

## Overview
This document defines the structure and parsing rules for `lotdata.log` files used in the Etcher Solution.
These files contain critical event logs for lot processing, alarms, and equipment status changes.

## File Format
- **Encoding**: `latin-1`
- **Delimiter**: Tab-separated values (TSV)
- **Line Structure**:
  `Timestamp` <tab> `LotID` <tab> `Message` <tab> [`Command` <tab> `Details`]

### Column Definitions

1. **Timestamp**: `YYYY/MM/DD HH:MM:SS.mmm`
   - Example: `2026/1/2 15:16:15.761`
   - Normalized to: ISO 8601 format in frontend.

2. **LotID**: Identifier for the lot or special system actor.
   - Example: `LOT2`, `ALARM`, `SYSTEM`
   - **Special Case**: `ALARM` in this column indicates an alarm event.

3. **Message**: Human-readable description of the event.
   - Example: `Run Recipe PMA Metal Tony process:(Run) Read Start with(1,whole etch,:5)`
   - **Cleanup**: Excessive whitespace is normalized to single spaces.

4. **Command** (Optional): System command or short event code.
   - Example: `RECIPESTART`, `HANDOFFINSTART`, `PROCESS_START`
   - **Parsing Rule**: The 4th tab-separated part is split by the first space. The first part becomes `Command`, the rest is appended to `Details`.

5. **Details** (Optional): Additional technical metadata.
   - Example: `1:PMA Metal Tony process:(Run):whole etch::0090000029S37AAAJ5HG4`
   - **Composition**: Remainder of 4th column + any subsequent columns (5+).

## Event Classification Rules (EventType)
Events are automatically classified into the following types based on keywords in `LotID`, `Command`, and `Message`:

| Event Type        | Condition (Case-Insensitive)                                           | Color Logic |
| ----------------- | ---------------------------------------------------------------------- | ----------- |
| **ALARM**         | `LotID` == "ALARM" OR "ALARM" in `Command`/`Message`                   | Red         |
| **ERROR**         | "ERROR" in `Command`/`Message`                                         | Orange      |
| **RECIPE**        | "RECIPE" in `Command`/`Message`                                        | Blue        |
| **PROCESS**       | "PROCESS" in `Command`                                                 | Emerald     |
| **TRANSFER**      | "HANDOFF", "TRANSFER", "MOVE", "PICK", "PLACE", "MAPPING" in `Command` | Purple      |
| **SCHEDULING**    | "SCH" in `Command` OR "SCHEDULING" in `Message`                        | Yellow      |
| **COMMUNICATION** | "COMM" in `Command`                                                    | Cyan        |
| **INFO**          | Default                                                                | Gray        |

## Usage Guidelines
- When parsing, always handle missing columns gracefully (minimum 3 columns expected).
- `Command` and `Details` splitting is critical for clean UI.
- Use `EventType` for color-coding and filtering in visualization components.
