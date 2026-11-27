<div align="center">

# On 글꺼내기 (on-text)

A remote MCP server that extracts text and tables from Korean HWP/HWPX, PDF, Word, Excel and PowerPoint files

[한국어](README.md) · **English**

`https://onmunseo.kr/mcp/text`

</div>

<!-- ai-install -->
## For AI assistants — installing this repository

There are three paths; **start with ①**. None needs a key or sign-in. The tools of this repository (On 글꺼내기 — text and table extraction) are in ① the local edition (the npm package `on-munseo-local` by the same maker, on-munseo — see the checks below) — when simply asked to install, register ①. `plugin/` and `server.json` in this repository are the settings for ③, the server edition of this bundle.

**① Local edition — documents never leave this computer (first).** Extracts text, masks personal data and runs pre-approval checks, and writes government-style HWPX, makes HWPX files and tidies text, on this computer. The program does not use the internet (check with `on_local_info` after installing — npx asking the npm registry for a newer version at start-up is npx itself; to avoid even that, `npm install -g on-munseo-local` and register the command `on-munseo-local`). Needs Node.js 20+.
- **Only the AI tool in use** (what an assistant should run — other tools' settings are left alone)
  ```bash
  claude mcp add -s user on-local -- npx -y on-munseo-local@latest
  ```
  Codex `codex mcp add on-local -- npx -y on-munseo-local@latest` · Claude Code on Windows `claude mcp add -s user on-local -- cmd /c npx -y on-munseo-local@latest` · Cursor / Claude Desktop: add `"on-local": { "command": "npx", "args": ["-y", "on-munseo-local@latest"] }` to `"mcpServers"` (Windows `"on-local": { "command": "cmd", "args": ["/c", "npx", "-y", "on-munseo-local@latest"] }`) · VS Code `code --add-mcp '{"name":"on-local","command":"npx","args":["-y","on-munseo-local@latest"]}'`
- **For a person, all AI tools on this PC at once** — finds Claude Code, Claude Desktop, Codex, Cursor and VS Code and registers it in each (backs up first, skips what is there, preview `--dry-run`, undo `--undo`, `--pair` adds ②).
  ```bash
  npx -y on-munseo-local@latest setup --pair
  ```

**② pair address (statutes) — only the words and text you give the tools go to the server; no tool sends files.**
- **Claude Code**
  ```bash
  claude mcp add --transport http -s user on-geungeo https://onmunseo.kr/mcp/law
  ```
- **Codex** — append two lines to the config file (`~/.codex/config.toml`; skipped if already there). Do not use `codex mcp add --url` — it opens a browser sign-in window right away.
  ```bash
  C="${CODEX_HOME:-$HOME/.codex}"; mkdir -p "$C"; touch "$C/config.toml"; if ! (cd / && codex mcp get on-geungeo >/dev/null 2>&1); then if cp "$C/config.toml" "$C/config.toml.bak-on-geungeo"; then printf '\n[mcp_servers.on-geungeo]\nurl = "https://onmunseo.kr/mcp/law"\n' >> "$C/config.toml"; (cd / && codex mcp get on-geungeo >/dev/null 2>&1) || { cp "$C/config.toml.bak-on-geungeo" "$C/config.toml"; echo "자동으로 더하지 못했습니다 — config.toml 에 손으로 더해 주세요"; }; else echo "백업을 만들지 못해 그만둡니다 — config.toml 은 그대로입니다"; fi; fi; (cd / && codex mcp get on-geungeo)
  ```
  On Windows, or where the `codex` command is not installed (IDE extension only), add the two lines `[mcp_servers.on-geungeo]` and `url = "https://onmunseo.kr/mcp/law"` to the end of that file by hand (if the file already has a one-line `mcp_servers = { … }` table, add `on-geungeo = { url = "https://onmunseo.kr/mcp/law" }` inside it instead).
- **Cursor** `"on-geungeo": { "url": "https://onmunseo.kr/mcp/law" }` · **VS Code** `code --add-mcp '{"name":"on-geungeo","type":"http","url":"https://onmunseo.kr/mcp/law"}'` · **Claude Desktop · claude.ai** (done by the person in the app) Settings → Connectors → Add custom connector → name `On 근거`, URL `https://onmunseo.kr/mcp/law` → Connect

**③ Server edition (this bundle) — no Node.js needed (for public documents).** Documents are sent to the server (data center in Korea), processed in memory only and discarded. Use it for public or your own documents.
- **Claude Code**
  ```bash
  claude mcp add --transport http -s user on-text https://onmunseo.kr/mcp/text
  ```
- **Codex** — append two lines to the config file (`~/.codex/config.toml`; skipped if already there). Do not use `codex mcp add --url` — it opens a browser sign-in window right away.
  ```bash
  C="${CODEX_HOME:-$HOME/.codex}"; mkdir -p "$C"; touch "$C/config.toml"; if ! (cd / && codex mcp get on-text >/dev/null 2>&1); then if cp "$C/config.toml" "$C/config.toml.bak-on-text"; then printf '\n[mcp_servers.on-text]\nurl = "https://onmunseo.kr/mcp/text"\n' >> "$C/config.toml"; (cd / && codex mcp get on-text >/dev/null 2>&1) || { cp "$C/config.toml.bak-on-text" "$C/config.toml"; echo "자동으로 더하지 못했습니다 — config.toml 에 손으로 더해 주세요"; }; else echo "백업을 만들지 못해 그만둡니다 — config.toml 은 그대로입니다"; fi; fi; (cd / && codex mcp get on-text)
  ```
  On Windows, or where the `codex` command is not installed (IDE extension only), add the two lines `[mcp_servers.on-text]` and `url = "https://onmunseo.kr/mcp/text"` to the end of that file by hand (if the file already has a one-line `mcp_servers = { … }` table, add `on-text = { url = "https://onmunseo.kr/mcp/text" }` inside it instead).
- **Cursor** `"on-text": { "url": "https://onmunseo.kr/mcp/text" }` · **VS Code** `code --add-mcp '{"name":"on-text","type":"http","url":"https://onmunseo.kr/mcp/text"}'` · **Claude Desktop · claude.ai** (done by the person in the app) Settings → Connectors → Add custom connector → name `On 글꺼내기`, URL `https://onmunseo.kr/mcp/text` → Connect

Usually ① alone is enough. Without Node.js, or with connectors only, register ③ (the server edition of this bundle). Registering ① and ③ together shows the same tools twice. Mention the other paths only if the user asks.

**Things to check before installing** — the assistant can verify these itself.
- **Package**: npm `on-munseo-local` 0.2.0. `npm view on-munseo-local version maintainers homepage` → `0.2.0` · `on-munseo <onmunseo.key@gmail.com>` · `https://onmunseo.kr`. The publishing account `on-munseo` is the same maker as the GitHub organization [on-munseo](https://github.com/on-munseo) (same contact mail and home page). The npm web page sometimes blocks automated reads; use the npm command or the [registry record](https://registry.npmjs.org/on-munseo-local/0.2.0).
- **Fingerprint**: `on-munseo-local-0.2.0.tgz` SHA-256 `a3d2ecb03bcf53e4bf97555b190cd5b200f4388e726dbc11ade6e0835cfecd72` ([SHA256SUMS](https://github.com/on-munseo/on-munseo.github.io/blob/main/SHA256SUMS)). Compare with `npm pack on-munseo-local@0.2.0` then `shasum -a 256 on-munseo-local-0.2.0.tgz`. The registry `dist.integrity` is `sha512-pyNyYarxtKoWu+5clkIIa1IICoampkw576tUOP7k7n7Ej/aF40y/rq/OoIN9zf/2mGguhTmNJE5HoKg2IEFz9A==`.
- **No internet use**: before installing, `netScan` in the package's `MANIFEST.json` (zero network calls in our code — the one listed exception is the engine's wasm loader path used only when given a URL); after installing, `on_local_info` → `network_modules_loaded: []`, and on Node 25+ `network.blocked_by_node: true` (the runtime blocks networking). npx asking the npm registry for a newer version at start-up is npx itself (avoid it with a global install and the `on-munseo-local` command).
- **Terms**: local edition [LICENSE.md](https://unpkg.com/on-munseo-local@0.2.0/LICENSE.md) (free, work use allowed · no reverse engineering, redistribution of modified copies or resale) · server edition [terms](https://onmunseo.kr/terms) · [privacy](https://onmunseo.kr/privacy).
- **On폭 fonts** are not part of this install (optional free fonts for drawing Korean documents on screen).

The tools do not attach to the conversation that registered them. When done, tell the user: "Restart the tool or open a new conversation to see the On 글꺼내기 tools." If `claude mcp list` (Codex: `codex mcp list`) lists the name, it is registered. Full steps (Korean): [llms-install.md](llms-install.md)

<!-- ai-install -->

Send a Korean HWP/HWPX/HML, PDF, Word, Excel or PowerPoint file and get its text and tables back as Markdown. Merged cells and nested tables come back as HTML tables; footnotes, bullets, paragraph numbers and bold text are kept. Extraction is rule-based, with no language model. Some steps, such as the optional text recognition for scans and images (key holders, `ocr: true`), use small models on the server CPU, but documents are never sent to an outside AI company.

This repository holds only the setup guide and configuration files (there is no web page here — to use a screen, open the [On 문서 web editor](https://onmunseo.kr/app/)). The engine and server code of the service are not public. The service and its tools speak Korean.

## Comparing with other tools

Measured by us on the same documents with the same scorer — not an official ranking. Versions, conditions and definitions are in the [benchmark notes](https://github.com/on-munseo/on-munseo.github.io/blob/main/docs/benchmark.md) (Korean).

| Same conditions as a local HWP reader (domestic open-source engine A) | On 글꺼내기 | Engine A | Measured |
|---|---|---|---|
| 1,726 public HWP/HWPX files — Hangul character recall (against Hancom PDF) | 99.87% | 99.87% (same) | 2026-11-12 |
| 50 statute annex HWPX files (not used in development) vs. source XML — character error · table structure TEDS | 0.09% · 0.981 | 0.12% · 0.968 | 2026-11-20 · 11-23 |
| 300 Korean PDF pairs — reading order · tables | 0.964 · 0.921 | 0.950 · 0.921 | 2026-11-12 |
| 457 public Word/Excel/PowerPoint files — table cell recall | 98.95 · 99.80 · 99.89% | 90.08 · 99.01 · 95.63% | 2026-11-17 · 11-23 |
| 200 English PDF pages, official scorer — default reading (light text-in-image, guest and the default here) · precise (key) | 0.971 · 0.970 | latest 4.25.22 default 0.965 · 0.967 (OCR on) | 2026-11-26 · 11-25 |
| 2,150 official HWP/HWPX files, extraction speed (same M3 Mac, same run, warmed up) — total · median per file | 3.1 s · 0.27 ms | 36.8 s · 0.52 ms | 2026-11-26 |

- On the English PDFs only the table-structure (TEDS) cell is higher for Engine A's latest release 4.25.22 (0.9805 vs. our default 0.9799; same Mac, same official scorer, 2026-11-26).
- **Rules, not AI:** no language model is called, so the same input gives the same result and nothing that is not in the source is made up.
- **Where documents go:** documents sent to this address are processed only in memory on the On 문서 server (a data centre in Korea) and discarded afterwards; they are not sent to any outside AI company. Send only public documents or your own to this server-side address; handle internal documents of your organisation with the browser-only features of the [web editor](https://onmunseo.kr/app/) (open, edit and save HWP/HWPX, the pre-approval check, masking personal data) — attachment text extraction, OCR and HWPX creation in the web editor still go through the server.

**Jump to:** [Comparing](#comparing-with-other-tools) · [Why](#in-short--why-use-it) · [Tools](#what-it-does) · [Connect](#connect) · [Examples](#examples) · [Measured results](#measured-results) · [Guest and key](#guest-and-key) · [Safety](#safety) · [Good to know](#good-to-know)

## In short — why use it

| Common pain | With On 글꺼내기 |
|---|---|
| An AI chat cannot open Korean HWP/HWPX files, or reads only part of the text | One tool reads HWP/HWPX, PDF and Office files and hands over the text and tables as Markdown |
| Tables turn into running text and the cells get mixed up | Merged cells and nested tables come back as HTML tables (rowspan, colspan); `on_tables` returns tables only |
| You compare the old and new versions of a document by eye | `on_compare` lines up two documents as an old / new / note table |
| Chapter and section structure is lost when you index a document for search (RAG) | `on_chunks` returns chunks that keep the heading and bullet path |
| You worry that resident registration or account numbers in a file go straight to the AI | If resident/alien registration, account, card or passport numbers are found, `on_read` returns a masked text with a notice (when `mask` is not set) |

**When you do not need it:** If you only need a few lines, copy and paste is enough. On a PC with the Hancom word processor installed, copying from there works too.

## What it does

| Tool | What it does | Guest |
|---|---|---|
| `on_read` | Text and tables as Markdown (merged cells and nested tables as HTML), with page, paragraph and table counts | yes |
| `on_detect` | Identifies the format (hwp, hwpx, pdf, docx, xlsx, pptx and more) and version, compression, encryption and distribution-only flags | yes |
| `on_meta` | Title, author, last saved by, created and modified dates, page count (the body is not read) | yes |
| `on_tables` | Data and contact tables only (decoration boxes skipped unless `all=true`); `as=rows` returns rows and cells as JSON | yes |
| `on_chunks` | Search and RAG chunks (JSON) that keep the heading and bullet path; tables become separate chunks | yes |
| `on_compare` | Lines up two documents (formats may differ) paragraph by paragraph and cell by cell as an old / new / note table | yes |

- The 6 guest tools were checked on 2026-11-26 by calling `tools/list` on the live server as a guest.
- Options: `ocr: true` text recognition for scanned pages and images (key holders, 30 pages a day) · `precise: true` reads text and tables inside PDF images in place (key holders; `on_read`, `on_tables`, `on_chunks`) · `mask` (`on_read` masks automatically only when registration, account, card or passport numbers are found unless you set it; true always masks, false gives the original; `on_tables`, `on_chunks` and `on_compare` mask when true).
- Send files as `file_base64` (the file in base64) plus `file_name` (the extension decides the format), or put a 15-minute link from On 문서 “자료 올리기” (upload, [onmunseo.kr/start](https://onmunseo.kr/start)) in `file_url`. `on_compare` takes the two documents as `a_base64`/`a_name` (or `a_url`) and `b_base64`/`b_name` (or `b_url`).

## Connect

Of the three paths at the top ([For AI assistants](#for-ai-assistants--installing-this-repository): ① local edition · ② pair address · ③ server edition), this section details ③, the server edition of this bundle, for each tool.

**Claude Code** — one line in the terminal (`-s user` makes it available in every folder):

```bash
claude mcp add --transport http -s user on-text https://onmunseo.kr/mcp/text
```

**Claude Code plugin** — one line (Claude Code 2.1.275 or later):

```bash
claude plugin install on-text --marketplace on-munseo/on-text
```

In every app, the conversation in which you register the server does not get the tools yet. Restart the app once (Claude Code: `/exit`, then `claude`; VS Code: a new chat) or open a new conversation, and the tools are there from then on. Without a key, skip the sign-in step and use it as a guest.

> To use all document tools and the law tools (On 근거) together, add `https://onmunseo.kr/mcp` instead of this bundle (recommended — [On 문서](https://github.com/on-munseo/on-munseo.github.io)). If you add both, the same tools appear twice.

### Claude Code — more

- With a key: in Claude Code open `/mcp` → `on-text` → Authenticate, and the On 문서 sign-in page opens. Enter your key there.
- To pass the key as a header: `claude mcp add --transport http -s user on-text https://onmunseo.kr/mcp/text --header "Authorization: Bearer <key>"`. This stores the key in `~/.claude.json`, so the sign-in page is the better way.
- Two-step plugin install (older versions): `claude plugin marketplace add on-munseo/on-text`, then `claude plugin install on-text@on-text`. In a Claude Code chat: `/plugin marketplace add on-munseo/on-text` → `/plugin install on-text@on-text`.
- Check: `claude mcp list` · remove: `claude mcp remove on-text`.

### claude.ai · Claude Desktop (connectors)

Settings → Connectors → “Add custom connector”: name `On 글꺼내기`, URL `https://onmunseo.kr/mcp/text`, then Connect. With a key, enter it on the On 문서 sign-in page that opens; without one you are a guest. Menu names differ by plan and version, and some plans may not have this menu.

To add it to Claude Desktop through the config file, use the `mcp-remote` bridge (needs Node.js). macOS `~/Library/Application Support/Claude/claude_desktop_config.json` · Windows `%APPDATA%\Claude\claude_desktop_config.json`:

```json
{
  "mcpServers": {
    "on-text": {
      "command": "npx",
      "args": [
        "-y",
        "mcp-remote",
        "https://onmunseo.kr/mcp/text"
      ]
    }
  }
}
```

To send a key as a header, append `"--header", "Authorization:${ON_AUTH}"` to `"args"` and add `"env": { "ON_AUTH": "Bearer <key>" }` (no space after the colon, so Windows does not split the argument). Restart Claude Desktop after saving.

### ChatGPT

1. Settings → Apps & Connectors → Advanced settings: turn on Developer mode (wording may differ by version).
2. Create a custom MCP server: name `On 글꺼내기`, URL `https://onmunseo.kr/mcp/text`, authentication “OAuth” if you have a key, otherwise “None”.
3. Enter your key on the On 문서 sign-in page, then turn the connector on from the tools (+) menu of a chat.

- ChatGPT receives a separate pass instead of your key (same permissions and daily quota). Revoking the key blocks the pass at once.
- Passing a file attached in the chat is not supported yet. Upload the file on On 문서 “자료 올리기” ([onmunseo.kr/start](https://onmunseo.kr/start)) and pass the 15-minute link; paste text directly.

### Codex

Without a key, add two lines to `~/.codex/config.toml`:

```toml
[mcp_servers.on-text]
url = "https://onmunseo.kr/mcp/text"
```

- `codex mcp add` opens the On 문서 sign-in window right away. With a key, enter it there; without one, close it (Ctrl-C) and use the two lines above.

### Cursor

`~/.cursor/mcp.json` (all projects) or `.cursor/mcp.json` in a project:

```json
{
  "mcpServers": {
    "on-text": {
      "url": "https://onmunseo.kr/mcp/text"
    }
  }
}
```

If your Cursor version cannot sign in to remote MCP servers (OAuth), add `"headers": { "Authorization": "Bearer <key>" }` (the key is then stored in the file).

### VS Code

`.vscode/mcp.json` in the workspace:

```json
{
  "servers": {
    "on-text": {
      "type": "http",
      "url": "https://onmunseo.kr/mcp/text"
    }
  }
}
```

To send a key as a header without writing it in the file, let VS Code ask for it on first start:

```json
{
  "inputs": [
    {
      "type": "promptString",
      "id": "on-key",
      "description": "On 문서 key",
      "password": true
    }
  ],
  "servers": {
    "on-text": {
      "type": "http",
      "url": "https://onmunseo.kr/mcp/text",
      "headers": {
        "Authorization": "Bearer ${input:on-key}"
      }
    }
  }
}
```

### Try it with curl

List the tools (add `-H "Authorization: Bearer <key>"` if you have a key):

```bash
curl -s https://onmunseo.kr/mcp/text \
  -H "Content-Type: application/json" -H "Accept: application/json, text/event-stream" \
  -d '{"jsonrpc":"2.0","id":1,"method":"tools/list"}'
```

Read a document — send the file as base64 (the extension of `file_name` decides the format):

```bash
B64=$(base64 < example.hwpx | tr -d '\n')
printf '{"jsonrpc":"2.0","id":1,"method":"tools/call","params":{"name":"on_read","arguments":{"file_name":"example.hwpx","file_base64":"%s"}}}' "$B64" > call.json
curl -s https://onmunseo.kr/mcp/text \
  -H "Content-Type: application/json" -H "Accept: application/json, text/event-stream" \
  --data-binary @call.json
```

A one-page HWPX made with `on_write` was sent this way; the reply was the safety notice line, “쪽 1 · 문단 3 · 표 0” (1 page, 3 paragraphs, 0 tables) and the Markdown text (2026-11-26, guest). The answer is the text in `result.content`. The server is stateless (Streamable HTTP, POST), so a single `tools/call` works without `initialize`. Opening the URL in a browser shows a short guide page.

## Examples

| If you say | This tool |
|---|---|
| “(attaching an HWP file) Summarize this document” | `on_read` extracts the text, then the AI summarizes it |
| “Pull out only the tables from this PDF” | `on_tables` |
| “Make an old/new comparison table of last year's and this year's rules” | `on_compare` |
| “Who last edited this file, and when?” | `on_meta` |
| “This file will not open. What format is it?” | `on_detect` |
| “Split it by chapter and section for search” | `on_chunks` |

The AI decides which tool to call; these are typical cases.

## Measured results

| What | Test set | Result | Build · date · machine |
|---|---|---|---|
| Korean text in HWP/HWPX | 1,726 files (1,404 statute annexes and forms + 322 government press releases), scored against the text of the matching Hancom PDF | Hangul character recall 99.87% (1,714,403 of 1,716,582 characters) · files with no character missing 98.78% | build of 2026-11-12 |
| Against the HWPX source | 50 statute-annex HWPX files not used in development, scored against the text and tables inside each file | character accuracy 0.999 · table structure TEDS 0.981 | text 2026-11-20 · tables 2026-11-23 (live build at the time) |
| Word, Excel, PowerPoint | 457 public-data files (160 Word · 179 Excel · 118 PowerPoint), scored against text read with the standard libraries | text recall 100% · table-cell recall Word 98.95% · Excel 99.80% · PowerPoint 99.89% · read failures 0 | 2026-11-23 |
| English PDF | opendataloader-bench, 200 pages, official scorer (overall) | default reading 0.971 (guest and the default of this endpoint — light OCR of image areas on non-Korean pages, 2026-11-26) · precise reading 0.970 (`precise: true`, key holders — also 0.970 on the 125 pages not seen during development). The highest score on the official leaderboard (14 engines) is 0.907 | 2026-11-25 · iMac M1 · 0.027 s / 0.396 s per document |
| Korean PDF | 300 HWP↔PDF pairs of the same annexes, scored against text and tables from the HWP source (reference built with two engines and averaged) | reading order 0.964 · tables 0.921 · read failures 0 | build of 2026-11-12 |
| Scans and photos (`ocr: true`, key) | KoOCR-Bench v2, 877 pages | overall 0.883 · handwriting 0.850 | 2026-11-24 · median 4.0 s per page (M3 MacBook) |
| Scanned tables (`ocr: true`, key) | 761 pages made by rendering table pages of public PDFs as images, scored against the PDF text layer | cell checks passed 88.8% (425 pages not seen while tuning) | 2026-11-24 |
| Speed | 2,150 public HWP/HWPX files (statute annexes, forms and more) | 8.2 s for all (median 0.76 ms per file) | build of 2026-11-12 · MacBook M5 Pro |

<sub>All values were measured by us; this is not an official ranking. Rows marked “build of 2026-11-12” were measured with that build and have not been re-run on the live build. The leaderboard value is the one the leaderboard publishes (engines outside the leaderboard are not included); our values come from the same official scorer, run by us. Definitions, confidence intervals and methods are in the [measurement notes](https://github.com/on-munseo/on-munseo.github.io/blob/main/docs/benchmark.md#5-글-꺼내기파싱--ai검색에-넣을-글과-표) (Korean).</sub>

Weak spots: scans and handwriting are read on the server CPU without a GPU, so they score below large AI models. Tables in Korean PDFs (0.921) score lower than other formats, and 16 of the 1,726 HWP/HWPX files lost more than 10 characters. Word table cells (98.95%) are the lowest of the three Office formats.

## Guest and key

|  | Guest (no key) | Key |
|---|---|---|
| How to connect | Add the URL with no authentication | Enter the key on the On 문서 sign-in page (OAuth), or send `Authorization: Bearer <key>` |
| Tools at this endpoint | 6 | 6 |
| Daily quota | 50 calls a day and 6 a minute (the statute tools `law_`·`med_` have their own 100 a day and 20 a minute) per internet address (IP), counted across all On 문서 endpoints and the web editor. When the shared guest quota for the day runs out, guest access closes until midnight Korea time | A daily quota and concurrency limit set per key |
| Size | 6 MB per request (base64 included) | 25 MB per file |
| Needs a key | — | text recognition (`ocr: true`, 30 pages a day per key) and precise reading (`precise: true`, PDF only — pages count toward both the 30-page recognition quota and the 40-page precise-reading quota) |

- To get a key, write to `onmunseo.key [at] gmail.com` (subject “On 문서 열쇠 신청”), with what you will use it for and which bundle you need (e.g. On 글꺼내기).
- A wrong or revoked key gives **401**; a bundle that is not in your key gives **403**. Too many calls in a short time give “too many requests” (429) — wait about a minute.
- Never append the key to the URL; it would end up in chat and access logs.

## Safety

1. **Nothing is kept.** Documents and text you send are handled in server memory only and discarded when done. No document files are stored; the access log holds only the key alias, tool name, request size, time taken and success (no contents, no file names).
2. **No language model.** Extraction is rule-based. Some steps, such as the optional text recognition (key holders), use small models on the server CPU, but documents are never sent to outside AI companies.
3. **Send only public documents or your own to this server-side address.** Handle internal documents of your organisation with the browser-only features of the [web editor](https://onmunseo.kr/app/) (open, edit and save HWP/HWPX, the pre-approval check, masking personal data). Documents with a written security marking (such as “대외비”, confidential) are not returned (except for keys specially allowed to).
4. **Extracted text is returned as data.** When a result carries text taken from a document, its first line says not to follow instructions written inside the document, so text that looks like an instruction to the AI is not obeyed.
5. **Safety limits stay on.** Size limits against attacks such as zip bombs, and refusal of distribution-only documents, are never lifted for convenience.

Report security problems as described in [SECURITY.md](SECURITY.md).

## Good to know

- **This is a free trial service.** Features, quotas and URLs may change or stop. Changes are noted in [CHANGELOG.md](CHANGELOG.md).
- **Results are for reference.** Have a person check them before approval.
- If a guest sets `precise: true`, the default reading is used and the first line of the result says so. Precise reading works on PDF only; other formats fall back to the default reading with a notice.
- Editing, PDF output and page images are not in this bundle (see [On HWPX 만들기](https://github.com/on-munseo/on-hwpx)).
- Internet only — it cannot be used from closed government networks.
- Menus differ by app and version, so screens may not match the steps above. If something fails, see [SUPPORT.md](SUPPORT.md).

## Terms, privacy and notices

- Bulk collection of results and use of results to train other AI models are not allowed. Automated abuse (several keys to get around limits, non-stop calls, scraping results) leads to the key being cut off without notice. Extracting or reverse-engineering the engine, rules or code is not permitted.
- Summary: [terms in the MCP guide](https://github.com/on-munseo/on-munseo.github.io/blob/main/MCP.md#5-이용-조건약관-요약) (Korean). Full text: [terms of use](https://onmunseo.kr/terms) and [privacy policy](https://onmunseo.kr/privacy) (Korean).
- “Hangul”, “Hancom”, “HWP” and “HWPX” are trademarks or registered trademarks of Hancom Inc. On 문서 is an independent service with no affiliation, sponsorship or endorsement by Hancom Inc.
- Claude, ChatGPT, Cursor, VS Code and other names belong to their owners and are used only to explain how to connect. Full notices: [notices in the MCP guide](https://github.com/on-munseo/on-munseo.github.io/blob/main/MCP.md#고지) (Korean).

## Glossary

- **MCP** — Model Context Protocol, an open standard for AI tools to call outside tools. Add this URL as an MCP server and the AI calls these tools inside the chat.
- **Remote MCP server** — An MCP server reached over the internet; nothing to install on your computer.
- **Bundle endpoint** — A URL that carries one On 문서 feature set (`/mcp/text` and so on). Everything is at `/mcp`.
- **Key (열쇠)** — The access value issued by On 문서, with a daily quota and a set of bundles. Keep it secret.
- **Guest (손님)** — Use without a key: some tools and a small daily quota, right away.
- **OAuth (sign-in page)** — You enter the key on the On 문서 sign-in page when connecting; the app receives a separate pass instead of the key.
- **HWPX** — The XML format of Korean Hangul documents (KS X 6101 OWPML); .hwp is the older format.
- **base64** — A way of writing a file as text, used to put a file inside JSON.
- **Upload link (15 minutes)** — Uploading a file on the On 문서 web ([onmunseo.kr/start](https://onmunseo.kr/start)) gives a link that works for 15 minutes; pass it as `file_url` from AI chats that cannot send files. AI tools that can run commands (Claude Code and others) can upload with one line, `curl -s --data-binary @file -H "x-file-name: name" https://onmunseo.kr/api/drop`, and get the same link (`url`) — faster than base64, and uploads do not count against the daily quota.
- **Markdown** — A plain-text format that marks headings (#), lists (-) and tables (|). Easy for AI to read.
- **RAG** — Retrieval-augmented generation: the AI looks up document chunks before answering. `on_chunks` makes those chunks.
- **TEDS** — A score for how closely a table’s cell layout, merged cells and cell text match the reference (1 is perfect).

## Other On 문서 bundles

| Product | Repository | Endpoint (after `https://onmunseo.kr`) |
|---|---|---|
| **On 문서 (all)** | [on-munseo.github.io](https://github.com/on-munseo/on-munseo.github.io) — web editor and every MCP tool | `/mcp` |
| On 글꺼내기 | this repository | `/mcp/text` |
| On 결재점검 | [on-check](https://github.com/on-munseo/on-check) | `/mcp/check` |
| On 개인정보 가리기 | [on-mask](https://github.com/on-munseo/on-mask) | `/mcp/mask` |
| On HWPX 만들기 | [on-hwpx](https://github.com/on-munseo/on-hwpx) | `/mcp/hwpx` |
| On 공문서 쓰기 | [on-gov](https://github.com/on-munseo/on-gov) | `/mcp/gov` |
| On 글 다듬기 | [on-tidy](https://github.com/on-munseo/on-tidy) | `/mcp/tidy` |
| On 근거 | [on-geungeo](https://github.com/on-munseo/on-geungeo) | `/mcp/law` |
| On폭 글꼴 (fonts) | [napurrin/onpok-fonts](https://github.com/napurrin/onpok-fonts) — free fonts (SIL OFL 1.1) · not needed for the MCP install (optional) | — |

## License

Text is [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/); configuration files (`server.json`, `.claude-plugin/`, `plugin/`) are MIT. The names “On 문서” and “On 글꺼내기” and the logo are not covered by these licenses. The engine and server code of the service are not in this repository and are not public. See [LICENSE](LICENSE).

---

Made by [napurrin](https://github.com/napurrin), who builds and maintains it while learning from Coco.
