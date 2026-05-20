---
name: board-meeting-prep
description: Prepares the user for an upcoming board meeting by reviewing all board pack documents and the risk register, then generating a prioritized list of shareholder-focused questions about company performance, risks, and governance. Invoke when the user asks to prepare for a board meeting, review the board pack, or generate questions for a meeting.
---
# Board Meeting Prep

When the user asks to prepare for a board meeting, follow these steps to produce a comprehensive, shareholder-focused question list.

## Step 1: Identify the Next Meeting

- Query the **Meeting Schedule** list (ID: `632193d2-917b-4172-9961-56bc3aab6f9a`) for items where Status = "Scheduled" and MeetingDate is in the future.
- Sort by MeetingDate ascending and pick the nearest upcoming meeting.
- Note the Meeting Name (Title), MeetingDate, Location, and Chair.
- If the user specifies a particular meeting by name or date, use that instead.

## Step 2: Retrieve the Board Pack

- Query the **Board Pack** library (ID: `e4217a0e-d1bf-4dd3-bb55-99bdb55c4215`).
- Filter by the Meeting lookup field matching the identified meeting, or if no meeting link exists, retrieve all documents with Status = "Final" or "Draft" that were recently modified.
- Retrieve all fields including: FileLeafRef, DocumentType, Status, Confidential, Meeting, _ExtendedDescription.
- Read the content of each document using the file reading tool. Prioritize Agendas first, then Board Papers, then Reports, then Presentations, then Minutes from prior meetings.

## Step 3: Analyze Documents for Performance Impact

Review each document and identify items that could affect company performance. Look for:

- **Financial indicators**: Revenue, profit, margins, cash flow, debt levels, forecasts, budget variances, capital expenditure
- **Strategic changes**: New initiatives, market expansion, product launches, M&A activity, partnerships, divestitures
- **Operational issues**: Supply chain problems, staffing challenges, technology failures, capacity constraints
- **Customer/market signals**: Customer churn, market share changes, competitive threats, pricing pressure
- **Governance concerns**: Board composition changes, executive compensation, related-party transactions, audit findings, regulatory actions
- **Vague or missing information**: Areas where the board pack lacks detail, uses ambiguous language, or omits expected updates

## Step 4: Review the Risk Register

- Query the **Risk Register** list (ID: `25afe23a-ee20-4bfd-af79-a4419fd6a8c7`).
- Retrieve all fields: Title, Category, Description, Likelihood, Impact, RiskScore, Owner, Mitigation, Status, ReviewDate, Created.
- Focus on risks where Status = "Open" or "Mitigating".
- Flag **recently added risks** — items where the Created date is within the last 30 days.
- Flag **high-severity risks** — items where Likelihood or Impact is "High" or "Very High".
- Flag **overdue reviews** — items where ReviewDate is in the past.
- Note any risks with weak or missing Mitigation plans.
- Categories to pay attention to: Financial, Operational, Compliance, Health and Safety.

## Step 5: Cross-Reference Risks with Board Pack

- Connect Risk Register items to topics found in the board pack documents.
- Identify risks that are mentioned or should be mentioned in the board pack but are not.
- Look for discrepancies between the tone of board papers (optimistic) and the risk register (concerning).

## Step 6: Generate the Question List

Produce a structured, prioritized list of questions organized into these sections:

### Financial Performance & Outlook
Questions about revenue, profitability, cash flow, forecasts, and budget adherence. Challenge any overly optimistic projections or unexplained variances.

### Strategic & Operational Risks
Questions about strategic decisions, operational challenges, and their potential impact on shareholder value. Reference specific board pack content.

### Recently Added Risks
Questions specifically about risks that appeared in the Risk Register in the last 30 days. Ask why they emerged, what the timeline for mitigation is, and what the worst-case scenario looks like.

### Governance & Compliance
Questions about board oversight, audit findings, regulatory compliance, executive decisions, and transparency. Flag any related-party transactions or conflicts of interest.

### Shareholder Value & Capital Allocation
Questions about dividends, share buybacks, investment priorities, return on capital, and long-term value creation strategy.

## Step 7: Flag Shareholder Concerns

At the end, include a clearly marked **"Shareholder Alert"** section that highlights:

- High-impact risks (High/Very High) that lack adequate mitigation plans
- Declining financial metrics or missed targets mentioned in board papers
- Vague or evasive language in board documents that warrants follow-up
- New risks without clear ownership or timelines
- Any gaps — topics you would expect to see in a board pack that are missing
- Overdue risk reviews that suggest insufficient oversight

## Output Format

Present the output as:

1. **Meeting Summary** — Brief overview of the meeting details and board pack contents
2. **Questions** — Numbered questions grouped by the sections above, with a brief rationale for each question referencing the specific document or risk that prompted it
3. **Shareholder Alerts** — Bulleted list of top concerns

## Constraints

- Do not fabricate information — all questions must be grounded in actual board pack content or risk register data
- If the board pack is empty or documents cannot be read, inform the user and work with whatever is available
- Treat Confidential documents with care — note their existence but flag that they are marked confidential
- If no upcoming meeting is found, ask the user which meeting to prepare for
