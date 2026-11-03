# Log Forensics

A single-file, offline HTML tool that helps support staff troubleshoot iNovah environments without needing deep technical knowledge. Drop a log onto the page and it reconstructs what happened, where it failed, and what to do next.

**Everything runs in the browser.** Nothing is uploaded and there are no external dependencies, so client data never leaves the server.

## Getting started

1. Copy `log-forensics.html` to a fixed folder on the server (so everyone uses the same version).
2. Open it in Edge or Chrome.
3. Drop a log file onto the matching card, or anywhere on the page — the log type is detected automatically.

Each card has a **Sample** button to see what a report looks like, and a **Where to find these logs** panel with the default file locations and a Copy button for each path.

## Supported logs

| Card | Log | Typical location |
|---|---|---|
| **IIS web logs** | W3C request logs (`u_exYYMMDD.log`) | `%SystemDrive%\inetpub\logs\LogFiles\W3SVC<SiteID>` |
| **E-Commerce logs** | eCommerce SQLite logs (`*-MM-DD-YYYY.db`) | `C:\ProgramData\System Innovators\iNovah\System Logs\eCommerce` and the `App_Data` folders under `C:\Program Files (x86)\System Innovators\` |
| **SQL Server log** | `ERRORLOG`, `ERRORLOG.1`… (full SQL Server and SQL Express) | `C:\Program Files\Microsoft SQL Server\MSSQL<Version>.<Instance>\MSSQL\Log` |
| **iNovah logs** | iNovah application logs (`AdminMgmtPortal.txt` etc.) | `C:\ProgramData\System Innovators\iNovah\System Logs\<Instance>` |

The page also lists locations for logs that don't have an analyzer yet: SSRS, Tender Retail / MCM, PayConex POSLink and installation logs.

## What a report shows

- **Verdict** — a one-line summary of the main problem (or confirmation that nothing is wrong).
- **What to do next** — numbered, plain-language steps for that problem, with clear "escalate if…" guidance. Click any row to get steps for that specific item.
- **Flow** — source → receiving system → result, showing where the failure happened.
- **Timeline** — volume of entries over time, colored by severity, to spot the incident window.
- **Breakdowns** — decoded error codes, top failing URLs, affected client IPs, login failures, recurring exceptions, failed or incomplete notifications.
- **Detail view** — full message, stack trace and context for any item.

## Features

- **Client ID / IP filter** to narrow a shared log to one client.
- **Copy for ticket / Download .txt** — select items and export a ticket-ready summary including the suggested next steps.
- **Time adjuster** to line up logs written in different time zones (IIS logs are UTC).
- **Multiple files** of the same type are merged onto one timeline (e.g. several days of IIS logs).
- **Large files** (over ~25 MB) are streamed in full; every problem is kept while totals and the timeline cover the whole log.
- Handles UTF-8 and UTF-16 encoded logs.

## Customizing

Two blocks near the top of the script are meant to be edited:

- `LOG_LOCATIONS` and `OTHER_LOCATIONS` — the paths and tips shown under **Where to find these logs**. Use `<Placeholder>` for parts that vary; they're highlighted for users.
- The **What to do next** functions (`nextIIS`, `nextSQL`, `nextINV`, `nextDB`) — the troubleshooting steps and escalation guidance. Edit the wording or add internal contacts.

## Limitations

- SQL Server Agent logs (`SQLAGENT.OUT`) aren't supported yet — use the ERRORLOG.
- For eCommerce databases in WAL mode, only the main `.db` file is read; the newest entries may be in the `.db-wal` file.
- One SQLite database is analyzed at a time.
- Example below:
<img width="1169" height="703" alt="image" src="https://github.com/user-attachments/assets/0f1869a6-78e5-42e8-9e7a-208c1fbf0f7d" />

<img width="1161" height="1218" alt="image" src="https://github.com/user-attachments/assets/c489d57f-1145-4e5b-95c9-16aec5db334b" />
