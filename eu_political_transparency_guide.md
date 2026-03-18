# EU-Level Political Transparency: Infrastructure & Technical Implementation

## Executive Summary

**The EU has significantly better transparency infrastructure than Poland**, but major gaps remain. This document analyzes what exists at EU level and what's achievable with current tools/AI.

---

## Part 1: EU Institutional Voting Data

### 1.1 European Parliament - Excellent Data Available

| Resource | What It Offers | URL |
|----------|---------------|-----|
| **HowTheyVote.eu** | Full MEP voting records, search by name/party/country | howtheyvote.eu |
| **MEPWatch** | Vote transparency, party rebellion tracking | mepwatch.eu |
| **ParlTrack** | Dossiers, votes, committees, JSON exports | parltrack.eu |
| **EU Open Data Portal** | 339 datasets, real-time documents | data.europarl.europa.eu |
| **Official EP Votes** | Roll-call votes by session | europarl.europa.eu/plenary |

**Key Features**:
- Individual MEP votes on every roll-call
- Political group affiliations
- Country breakdown
- Historical data (2004-present)
- "Rebellion" tracking (voting against party line)
- **JSON/API access available** (ParlTrack)

**Stats**:
- 23,157+ votes tracked
- 1,271 MEPs in database
- 7 years of active data

---

### 1.2 European Council / Council of the EU - Limited Transparency

| Aspect | Status |
|--------|--------|
| **Qualified Majority Voting** | Public results (double majority reached/not) |
| **Individual Minister Votes** | NOT disclosed |
| **Voting records by country** | Available since December 2009 |
| **Who voted how** | Only final outcome |

**The Gap**: Unlike EP, you cannot see:
- Which specific minister from each country voted
- Detailed breakdown of member state positions
- Individual country voting rationale

**URL**: https://www.consilium.europa.eu/en/documents-publications/public-register/votes/

---

### 1.3 European Commission - Meeting Minutes

- General approach publicly available
- Individual commissioner positions not tracked
- Impact assessments published

---

## Part 2: EU Lobbying Transparency

### 2.1 EU Transparency Register (MANDATORY since Sept 2025)

| Metric | Value |
|--------|-------|
| Registered organizations | 13,000+ |
| What it shows | Who lobbies, how much, on what issues |
| Mandatory | YES (since Sept 2025) |
| Financial data | Annual budget for lobbying |
| Issues tracked | Policy areas |

**What's Required**:
- Organization name and address
- Field of activity
- Number of employees in EU affairs
- Clients (if intermediary)
- Budget/turnover
- Policy interests

**Gaps**:
- No requirement to disclose specific meetings
- Quality/reliability issues (self-reported)
- 12% of MEP outside activities linked to registered lobbyists

**URL**: https://transparency-register.europa.eu

---

### 2.2 MEP Conflicts of Interest

| Data | Status |
|------|--------|
| Declaration of Financial Interests | Required at start of mandate |
| Outside activities | Reported (1,678 total) |
| Income ranges | Public (but vague: EUR1,001-5,000) |
| Issues found | 20% imprecise/incomplete |

**Problems Identified by Transparency International EU**:
- 42% of outside employment missing key info
- No banned activity list for high conflict-of-interest
- Up to 36 MEPs potentially deriving undisclosed income
- Only 12% cross-referenced with lobby register

**URL**: https://www.europarl.europa.eu/meps/en/{MEP_ID}/declarations

---

## Part 3: EU Political Party Funding

### 3.1 European-Level Parties

| Authority | What They Track |
|-----------|----------------|
| **APPF** (Authority for European Political Parties) | EU party/foundation funding |
| **Donations lists** | All donations EUR12,000+ disclosed |
| **Annual reports** | Full financial statements |

**EU Party Funding (2025)**:
- Total allocated: EUR567M+ annually
- Largest recipients: EPP, S&D, Renew, Greens/EFA

**URL**: https://www.appf.europa.eu

---

### 3.2 The Transparency Gap (Investigative Journalism)

**Key Finding** (Follow The Money, 2024):
- **74% of political donations in EU are hidden from public**
- Almost EUR1 billion in donations 2019-2022
- Germany and France worst offenders
- Central/Eastern European countries more transparent

**What's Hidden**:
- Individual donor identities below threshold
- Cross-border funding mechanisms
- "Charitable" vehicles used for political giving

**Source**: https://www.ftm.eu/transparency-gap

---

## Part 4: What's Achievable at EU Level (AI/Tools)

### 4.1 Vote Anomaly Detector - EASIEST

**Data Availability**: Excellent

| Component | Tool |
|-----------|------|
| Vote data | ParlTrack JSON API |
| Clustering | sklearn |
| Visualization | D3.js |

**What Already Exists**:
- HowTheyVote already does this!
- MEPWatch shows "rebels"
- Tracks party line deviations

**Additional Value**: Could add:
- Cross-country coalition analysis
- Time-series voting pattern changes
- Topic-based clustering

---

### 4.2 MEP-Lobby Connection Tracker

**Data Available**: Partial (gaps exist)

| Component | Tool |
|-----------|------|
| MEP declarations | europarl.europa.eu/meps |
| Lobby register | transparency-register.europa.eu |
| Cross-reference | Manual + NLP |

**What Could Be Built**:
- Link MEP outside activities to registered lobby organizations
- Track votes on files where conflicts exist
- Flag potential revolving door (former MEPs to lobby)

**Current Gap**: Only 12% of MEP activities linked to lobby register

---

### 4.3 EU Money-Vote Correlation

**Challenge**: More complex than US/Poland because:
- Multiple institutions involved
- Lobby register lacks meeting-level data
- Council votes are opaque

**What Could Be Done**:
- EP votes + lobby register positions (on specific files)
- Map organizations supporting/opposing bills to MEP voting patterns
- Track EU Commission + Parliament trilogues

**Tools**:
- MapLight model could apply to EP
- Lobby positions from public consultations
- EP committee amendments

---

### 4.4 EU Party Funding Network

**Data Sources**:
- APPF donations (above EUR12K public)
- EU party foundations
- National party funding (varies by country)

**What Could Be Built**:
- Network of EU party funding flows
- Link donations to policy positions
- Cross-border donation tracking

**Challenge**: 74% hidden = major gap

---

## Part 5: Technical Implementation

### 5.1 EU Data Sources (Ready to Use)

| Source | Type | Format | URL |
|--------|------|--------|-----|
| ParlTrack | Votes, dossiers | JSON | parltrack.eu |
| HowTheyVote | MEP votes | CSV | github.com/HowTheyVote/data |
| EU Open Data | 339 datasets | Various | data.europarl.europa.eu |
| Transparency Register | Lobbyists | API | transparency-register.europa.eu |
| Council Votes | Legislative outcomes | HTML | consilium.europa.eu |

### 5.2 Architecture (EU Scale)

```
+-------------------------------------------------------------+
|                    EU TRANSPARENCY STACK                   |
+-------------------------------------------------------------+
|  DATA SOURCES                                              |
|  +-- EP Votes (ParlTrack API)                             |
|  +-- MEP Declarations (scraped)                           |
|  +-- Lobby Register (API)                                  |
|  +-- Council Votes (consilium)                             |
|  +-- Party Funding (APPF)                                 |
+----------------------------+--------------------------------+
                             |
                             v
+-------------------------------------------------------------+
|                  PROCESSING LAYER                          |
|  +-- Python ETL pipelines                                 |
|  +-- PostgreSQL + PostGIS                                 |
|  +-- NLP: GPT-4o / Claude (declarations)                 |
|  +-- Graph: Neo4j (relationships)                        |
+----------------------------+--------------------------------+
                             |
                             v
+-------------------------------------------------------------+
|                   ANALYTICS LAYER                         |
|  +-- Vote clustering (sklearn)                            |
|  +-- Anomaly detection (wealth, voting)                  |
|  +-- Lobby-MEP cross-reference                           |
|  +-- Money-influence correlation                         |
+----------------------------+--------------------------------+
                             |
                             v
+-------------------------------------------------------------+
|                 PUBLIC DASHBOARD                           |
|  +-- MEP profile pages                                    |
|  +-- Vote explorer                                        |
|  +-- Lobby connections                                    |
|  +-- Alert system                                        |
+-------------------------------------------------------------+
```

---

## Part 6: Poland vs EU Comparison

| Dimension | Poland | EU (EP) | EU (Council) |
|-----------|--------|---------|-------------|
| **Individual votes** | Yes | Yes (excellent) | No |
| **Asset declarations** | Handwritten, poor | Vague ranges | N/A |
| **Lobby register** | None | Mandatory | Mandatory |
| **Party funding** | Partial | EU-level yes | N/A |
| **API access** | Partial | Good | Limited |
| **Data quality** | Low | High | Medium |

---

## Part 7: Priority Projects

### Priority 1: Connect EP Vote Data to Lobby Register
- **Why**: Most actionable gap
- **Data**: Both publicly available
- **Effort**: Medium
- **Value**: High - shows money to influence to votes

### Priority 2: Improve MEP Declaration Analysis
- **Why**: 20% incomplete, 12% linked to lobby register
- **Data**: Available but unstructured
- **Effort**: Medium (OCR + NLP)
- **Value**: High

### Priority 3: Council Vote Transparency Advocacy
- **Why**: Individual minister positions hidden
- **Data**: Would require institutional change
- **Effort**: N/A (advocacy)
- **Value**: High

### Priority 4: EU-Party Funding Network
- **Why**: 74% hidden
- **Data**: Partial from APPF
- **Effort**: High
- **Value**: Medium

---

## Part 8: Resources

### Essential EU Data Sources
| Source | URL |
|--------|-----|
| ParlTrack (votes/dossiers) | https://parltrack.eu |
| HowTheyVote | https://howtheyvote.eu |
| MEPWatch | https://mepwatch.eu |
| EU Open Data Portal | https://data.europarl.europa.eu |
| Transparency Register | https://transparency-register.europa.eu |
| Council Voting Results | https://consilium.europa.eu |
| APPF (Party Funding) | https://www.appf.europa.eu |

### Investigative Journalism
| Source | Focus |
|--------|-------|
| Follow The Money (FTM) | EU party funding investigation |
| VSquare | Eastern European money flows |
| Transparency International EU | MEP disclosure analysis |

### Technical Stack
| Component | Tool |
|-----------|------|
| Vote data pipeline | ParlTrack scraper |
| NLP for declarations | GPT-4o / Claude |
| Graph database | Neo4j |
| Visualization | D3.js / Cytoscape |
| OCR (if needed) | Transkribus |

---

## Summary: EU vs Poland

| Aspect | Poland | EU (EP) | EU (Council) |
|--------|--------|---------|-------------|
| **Can track individual votes?** | Yes | Yes | No |
| **Lobby transparency?** | None | Mandatory | Mandatory |
| **Asset declarations?** | Poor quality | Vague ranges | N/A |
| **Easy to build tools?** | Medium | Easy | Hard |

**Key Insight**: The **European Parliament is actually easier to analyze than national parliaments** because:
1. Better structured data
2. JSON APIs available
3. Lobby register mandatory
4. Active civil society tools exist

**The main EU gaps**:
- Council transparency (individual minister votes)
- MEP declaration quality (vague, incomplete)
- Party funding transparency (74% hidden)

---

*Last updated: March 2026*
