# Political Transparency Tools: Poland vs US & Technical Implementation Guide

## Quick Answer to Original Question

**No, you cannot find databases showing how cities/municipalities voted on specific laws (ustawy).**

In Poland, **cities and municipalities don't vote on national laws**. Poland is a parliamentary democracy — only the Sejm (460 elected MPs) votes on legislation. What you CAN track is how individual MPs voted, and there are databases for that.

---

## Part 1: Current Polish Transparency Infrastructure

### What's Available in Poland

| Data Source | What's Available | URL |
|-------------|------------------|-----|
| **Oświadczenia majątkowe** | Asset declarations for MPs, officials (many handwritten, inconsistent) | sejm.gov.pl, cba.gov.pl |
| **Jawne Partie** | Party donations & contracts | jawnepartie.pl |
| **Jawne Finanse** | Justice Fund beneficiaries | jawnefinanse.pl |
| **KRS** | NGO registration + basic financials | rejestrkrs.pl |
| **Sprawozdania OPP** | Public Benefit Organization reports | sprawozdaniaopp.niw.gov.pl |
| **Sejm voting** | Individual MP votes | sejm-stats.pl, SejmTracker |

**Problem**: Data is fragmented, inconsistent formats, no automated cross-linking, no family connection tracking.

### What's Missing in Poland

| Gap | Poland Status | What Needs Building |
|-----|---------------|---------------------|
| **NGO → Politician pipeline** | No tracking | Map NGO funding → politicians |
| **Lobbying registry** | Weak/no data | Register who lobbies whom |
| **Conflict of interest** | Fragmented | Unified cross-reference |
| **Media ownership transparency** | Partial | Who owns what media |
| **Beneficial ownership** | Coming (2026+) | Real owner tracking |

---

## Part 2: US Political Money Tracking (What Poland Lacks)

### Tools That Exist in US

| Tool | What It Does | URL |
|------|--------------|-----|
| **OpenSecrets** | Campaign contributions, lobbying, dark money, AIPAC tracking | opensecrets.org |
| **MapLight** | **Correlates votes with money** — shows who funded each bill | maplight.org |
| **LobbyView** | Network visualization of interest groups | lobbyview.org |
| **FEC** | Federal Election Commission raw data | fec.gov |

### Key US Innovation: Vote-Money Correlation

MapLight's Bill Positions data tracks:
- Which organizations support or oppose each bill
- Their funding sources
- How that correlates with legislator voting patterns

This is the model Poland doesn't have yet.

---

## Part 3: Achievable with Current AI/Tools

### 1. Vote Pattern Anomaly Detector

**Effort: LOW** | **Data: Already available**

```
Input: Sejm voting API → ML clustering → Output: Outlier MPs
```

| Component | Tool |
|-----------|------|
| Vote data | sejm-votes (GitHub: michalpazur/sejm-votes) |
| Clustering | sklearn, basic statistics |
| Visualization | D3.js / Python Plotly |

**What it does:**
- Cluster MPs by voting patterns
- Flag those who deviate from party line
- Show "expected" vs actual voting
- Highlight switchers on key bills

---

### 2. AI-Powered Oświadczenia Majątkowe Parser

**Effort: MEDIUM** | **Data: Scans from sejm.gov.pl**

Many asset declarations are handwritten. AI solves this:

| Component | Tool |
|-----------|------|
| OCR | **Transkribus** (specifically trained for Polish handwriting) |
| Parsing | GPT-4o / Claude (extract structured data from text) |
| Storage | PostgreSQL + pgvector (for similarity search) |

**What it does:**
- Upload scanned PDF → structured JSON
- Extract: property, savings, shares, companies
- Track wealth changes year-over-year
- Flag sudden wealth increases

**Cost**: ~€50-100/month for API credits + Transkribus

---

### 3. KRS Company Network Graph

**Effort: LOW** | **Data: KRS dumps available**

| Component | Tool |
|-----------|------|
| KRS data | Synteza.pro (commercial) or scrape from gov.pl |
| Graph DB | Neo4j / PostgreSQL |
| Visualization | Cytoscape.js, Gephi, D3 |

**What it does:**
- Map: Person → Company (owner/board) → Company → Person
- Find: All companies connected to MP X
- Detect: Shell company networks, circular ownership

**Existing example**: Offensive OSINT (offensiveosint.io) already does "Spółki posłów" visualization

---

### 4. Cross-Reference Dashboard

**Effort: MEDIUM** | **Data: Already public, just not connected**

| Component | Tool |
|-----------|------|
| MP voting | Sejm API |
| Asset declarations | Your OCR pipeline |
| Party donations | Jawne Partie API |
| Company ownership | KRS |
| Government contracts | BZP / GUS tender data |

**What it does:**
- Search: "Show me all companies owned by MPs who voted YES on bill X"
- Connect: This NGO → received EU funds → connected to MP Y → voted Z
- Alert: MP's company just won a government contract

---

### 5. Media/Speech Analysis

**Effort: LOW-MEDIUM** | **Data: Sejm transcripts**

| Component | Tool |
|-----------|------|
| Transcripts | sejm.gov.pl (wypowiedzi) |
| NLP | GPT-4o / Claude (summarization, sentiment) |
| Topic modeling | BERTopic |

**What it does:**
- Summarize MP speeches
- Track who speaks on what topics
- Detect shift in rhetoric over time

---

## Part 4: Recommended Architecture

```
┌─────────────────────────────────────────────────────────────┐
│                    MP / PUBLIC FIGURE                       │
├─────────────────────────────────────────────────────────────┤
│  Voting Record   │  Oświadczenia   │  Family Network        │
│  (Sejm)         │  Majątkowe      │  (manual + KRS)         │
└────────┬─────────┴────────┬─────────┴────────┬────────────┘
         │                 │                  │
         ▼                 ▼                  ▼
┌─────────────────────────────────────────────────────────────┐
│              CENTRAL CORRELATION DATABASE                  │
│  - Link MP → Companies → NGO → Funding → Votes             │
│  - Family relationship mapping                              │
│  - Anomaly detection (sudden wealth, outlier votes)       │
└────────┬───────────────────────────────────────────────────┘
         │
         ▼
┌─────────────────────────────────────────────────────────────┐
│                    PUBLIC DASHBOARD                         │
│  - Search by MP, by NGO, by company                        │
│  - "Influence flows" visualization                         │
│  - Alerts for anomalies                                    │
└─────────────────────────────────────────────────────────────┘
```

---

## Part 5: Quick Win Stack (MVP)

### Phase 1 (Week 1-2): Vote Anomaly Detection
1. Scrape Sejm votes → SQLite DB
2. Calculate party-line baseline
3. Flag MPs who deviate >20% from party
4. Simple web dashboard (Streamlit / Flask)

**Result**: "Which MPs broke from their party?"

### Phase 2 (Week 3-4): Company Connections
1. Add KRS company data for each MP
2. Map company → government contracts
3. Add Jawne Partie donations

**Result**: "Who has companies winning contracts?"

### Phase 3 (Month 2): Wealth Tracking
1. OCR pipeline for oświadczenia majątkowe
2. Year-over-year wealth tracking
3. Anomaly detection (sudden wealth ↑)

**Result**: "Who got richer unexpectedly?"

---

## Part 6: Existing Tools to Connect

| What you want | Existing Polish tool |
|---------------|---------------------|
| MP voting records | sejm-stats.pl |
| Party donations | Jawne Partie |
| Justice Fund | Jawne Finanse |
| Company registry | Synteza.pro, Transparent Data |
| NGO financials | Sprawozdania OPP |
| Court cases | orzeczenia.ms.gov.pl |

---

## Part 7: What NOT Easy (Institutional Change Required)

| Gap | Why |
|-----|-----|
| Real-time lobbying tracking | No mandatory registry |
| Beneficial ownership (until 2026+) | Not yet required |
| Automatic family linking | No public family DB |
| NGO → Politician money flow | Fragmented, no single source |

---

## Summary

Poland has **some** data, but it's fragmented and not interconnected. The US systems (especially MapLight) show what's *possible* — but would require significant data engineering to replicate in Poland due to legal/institutional gaps.

**Start with the vote anomaly detector** — it's the easiest first project:
1. Data is clean (Sejm API)
2. Algorithm is simple (distance from party centroid)
3. Value is immediate
4. No legal issues (public data)

---

## Resources

### Polish Data Sources
- Sejm voting: https://sejm-stats.pl
- Asset declarations: https://sejm.gov.pl/sejm10.nsf/page.xsp/formularzeosw
- Party donations: https://jawnepartie.pl
- KRS company data: https://synteza.biz
- NGO reports: https://sprawozdaniaopp.niw.gov.pl

### US Reference (for inspiration)
- OpenSecrets: https://opensecrets.org
- MapLight: https://maplight.org
- LobbyView: https://lobbyview.org

### Technical Tools
- Transkribus (OCR for handwriting): https://transkribus.org
- Neo4j (graph DB): https://neo4j.com
- D3.js (visualization): https://d3js.org

---

*Last updated: March 2026*
