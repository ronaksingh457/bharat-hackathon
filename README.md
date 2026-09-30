# THROUGHLINE

A calm documentation aid for turning raw screenshots into a neutral, exhibit-cited timeline. Throughline is not legal advice, does not identify a perpetrator, and does not make guilt or account-ownership claims.

## Stack choice and source assumption

This sandbox is preconfigured for a Next.js full-stack application, so this prototype uses **Next.js 16 + React 19 + a server-side route handler** instead of a separate Vite/FastAPI pair. The product behavior remains the requested MVP. No database is used by the workflow.

The referenced `BWB_4_0__kk.pdf` was not present in the project filesystem. The detailed product prompt was therefore treated as the available source of truth.

## Run in mock mode (under 5 minutes)

Requirements: Node.js 20+ and npm.

```bash
npm install
cp .env.example .env.local
npm run dev
```

Open `http://localhost:3000`. Leave the OpenAI and Anthropic settings empty. Choose **Load synthetic demo data** for the fastest walkthrough, or upload PNG/JPG files to see the clearly labeled mock extraction path. The assistant remains usable as a clearly labeled local guide.

## Optional vision extraction

Set both of these server-side values in `.env.local`:

```bash
OPENAI_API_KEY=your_server_side_key
OPENAI_VISION_MODEL=a_vision_capable_model_available_to_your_project
```

The app calls the documented OpenAI Responses API from `/api/extract`; the key is never sent to the browser. No model is hard-coded because availability can vary by account. The route requests strict JSON, validates it with Zod, retries once when the result cannot be validated, and then returns empty fields for manual entry.

## Claude documentation assistant

Set both values in `.env.local`, then restart the app:

```bash
ANTHROPIC_API_KEY=your_server_side_anthropic_key
ANTHROPIC_MODEL=a_claude_model_available_to_your_account
```

The server route `/api/chat` calls Anthropic’s documented Messages API. The key and model name stay server-side. The browser sends the user’s chat text plus a minimal status summary: current stage, record counts, platform labels, timestamp-source labels, and rule-based gap names. It does **not** send screenshots, account handles, usernames, or message text to the chat endpoint.

Claude cannot edit evidence, confirm a record, add a link, or change the report. Its system prompt prohibits identity matching, guilt or intent judgments, psychological labels, legal advice, fabricated facts, and claims of screenshot authenticity. When Anthropic settings are absent, the same panel uses a deterministic local guide and labels each response accordingly.

## Production check

```bash
npx next typegen
npm exec tsc -- --noEmit --pretty false
npm run build
npm run start
```

The platform preview uses its managed production start and health check. The Throughline workflow itself does not require PostgreSQL; the starter health route retains its platform-provided database check.

## Project structure

```text
.
├── .env.example                      # Server-side AI settings
├── .npmrc                            # Exact dependency versions
├── README.md                         # Setup, scope, and demo script
├── requirements.txt                  # Notes the intentional no-Python adaptation
├── package.json                      # Pinned Next/React dependencies and scripts
└── src
    ├── app
    │   ├── api
    │   │   ├── chat/route.ts         # Claude assistant and local guide fallback
    │   │   ├── extract/route.ts      # Vision/mock extraction and validation
    │   │   └── health/route.ts       # Platform health check
    │   ├── globals.css               # App, assistant, responsive, and print styles
    │   ├── layout.tsx                # Metadata and root shell
    │   └── page.tsx                  # Throughline entry point
    ├── components
    │   ├── throughline-app.tsx       # Six-stage interactive workflow
    │   └── throughline-chat.tsx      # Claude/local documentation assistant
    └── lib
        ├── capture-guides.ts         # Generic platform capture advice
        ├── demo-data.ts              # Six synthetic screenshot exhibits
        ├── evidence-rules.ts         # Gap checks and literal overlap rules
        └── types.ts                  # Evidence and timestamp-provenance model
```

## Workflow

1. **Upload** PNG/JPG screenshots or load six synthetic demo exhibits.
2. **Confirm** every extracted field beside its image. Editing a confirmed record makes it unconfirmed again.
3. **Check** rule-based evidence gaps such as missing timestamps, handles, platform labels, cropping, and relative times.
4. **Link** incidents while keeping literal overlaps separate from user-asserted beliefs.
5. **Timeline** exact confirmed dates, approximate/undated records, and counts by day, week, and platform.
6. **Report** a neutral packet with exhibit citations, then use the browser print dialog to save it as PDF.

## Sample report layout

The generated report contains:

1. Cover, scope, method, timestamp-authenticity note, and required disclaimer
2. Exhibit index with screenshot thumbnails and exhibit numbers
3. Confirmed timeline with separate message-time and screenshot-capture provenance
4. **Observed overlaps (literal matches only)**
5. User-asserted links
6. Evidence-gap summary
7. Records to request from the platform when message time is unknown
8. **Not established by this evidence**
9. Repeated footer disclaimer

Every factual report sentence is followed by an exhibit citation. The print stylesheet removes all app controls and formats the report for A4 PDF output.

## What is done

- End-to-end guided MVP from upload through PDF export
- Side-by-side screenshot review and editable nullable fields
- Explicit user confirmation gate before later steps
- Separate message and screenshot-capture time fields with source labels
- Relative-time handling that avoids fabricated exact dates
- Fixed, non-AI evidence-gap checks
- Fixed, literal-only overlap detection using normalized text, shared n-grams, exact dates, and platform labels
- Exact-date timeline plus day/week/platform counts
- Strictly separated observed overlaps and user-asserted links
- Neutral report template with exhibit citations and required boundaries
- Six generated, clearly synthetic screenshots with fictional test handles and harmless project messages
- Browser-local workspace storage and **Delete all my data**
- Generic screenshot-capture guidance for WhatsApp, Instagram, Telegram, X/Twitter, and other platforms
- Claude-powered documentation assistant with stage-aware, non-sensitive context
- Assistant guardrails for legal, identity, guilt, intent, authenticity, and psychological-judgment requests
- Clear Claude/local-response labels, prompt suggestions, error handling, and chat deletion
- Responsive UI and print-to-PDF export

## What is mocked

- With no AI settings, uploaded screenshots receive conspicuously labeled synthetic sample fields. The app states that those fields were not read from the images and requires user review.
- The six demo screenshots are generated SVG images representing fictional conversations. They are not real evidence and contain no real usernames or real-world harassment content.
- Without `ANTHROPIC_API_KEY` and `ANTHROPIC_MODEL`, the assistant uses a deterministic local guide. The UI labels those replies “Answered by local guide.”
- Report prose uses a deterministic template when no AI service is configured.

## Known limitations

- This is a hackathon prototype, not an evidence-authentication system.
- Browser print-to-PDF output can vary slightly by browser and print settings.
- Browser-local storage is convenient, not encrypted case storage. Deleting browser data can also remove the workspace.
- File `lastModified` is displayed only as **File details (not verified)** and does not prove screenshot capture time.
- Relative times are not converted to an exact date. A calendar range is shown only when a reference exists or as a relative description.
- Shared phrase detection is literal and may surface ordinary words; it does not infer identity, intent, or coordination.
- Assistant replies are guidance, not evidence, and never enter the generated report.
- The assistant does not view screenshots, handles, usernames, or message text; it receives only the disclosed status summary.
- The prototype does not request records from platforms; it only lists records whose message time is unknown.
- No accounts, cloud sync, database persistence, or legal analysis are included.

## 60-second demo script for judges

**0–8 seconds:** “Throughline turns a pile of screenshots into a neutral report that is honest about what the images can and cannot show. It never identifies a person or decides guilt.”

**8–16 seconds:** Click **Load synthetic demo data**. “These six exhibits are entirely synthetic. A real user can upload PNG or JPG screenshots from any platform.”

**16–28 seconds:** Show the side-by-side confirmation view and timestamp sources. “AI only drafts visible fields. Missing values stay blank, message time is separate from capture time, and nothing moves forward until the user confirms every record.” Click **Confirm all reviewed**.

**28–38 seconds:** Open **Check**, then ask the guide “What should I check next?” “Fixed rules create the flags. Claude can explain them, but it cannot edit evidence or add anything to the report.”

**38–48 seconds:** Open **Link**. “Literal phrase, date, and platform matches stay in Observed overlaps. The user’s own belief is stored separately as a User-asserted link.”

**48–55 seconds:** Open **Timeline**. “Only exact confirmed dates enter the dated timeline. Approximate and unknown records remain visibly separate.”

**55–60 seconds:** Open **Report** and click **Export / save as PDF**. “Every factual sentence cites an exhibit, the report ends with what is not established, and the disclaimer appears on every packet.”

## Privacy and safety boundary

Throughline is a documentation aid, not legal advice or a legal determination. AI reads, drafts, and explains; the user confirms facts. The Claude assistant is read-only and cannot change evidence or the report. The app does not make identity, guilt, psychological, or character judgments.
