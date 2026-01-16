# Research Operations Manual

## Document Purpose

This document defines the complete research methodology for evidence-based restaurant research for this project. It serves as the single source of truth for both human researchers and AI agents.

**Language Policy:**
- Language requirements are defined in `CONSTITUTION.md` and must be followed.
- This document does not restate those requirements.

---

## Project Scope

### In Scope

**Primary Objective**: Build evidence-based restaurant recommendations for group dining (around 16–20 people) in Taipei City.

**Target City**:
- Taipei City

**Categories Covered**:
- Restaurants (all cuisine types)

**Research Deliverables**:
- Scored candidate list with evidence
- Top picks and backup recommendations
- Practical constraints (reservations, hours, closures)
- Exclusion rationale for rejected candidates

### Out of Scope

**Not Researched**:
- Accommodations (already booked)
- Non-food activities or attractions
- Shopping (unless food-related)
- Nightlife (bars, clubs) unless food-focused
- Non-restaurant food venues (cafes, dessert shops)
- Cities outside Taipei City

**Fixed Constraints** (not modifiable by research):
- Group size: around 16–20 people
- Previously visited restaurants (reference only):
  - <店舗名>（<都市名>）

---

## Agent Roles and Responsibilities

### Research Agent
**Primary function**: Information discovery and evidence collection

**Responsibilities**:
- Search for candidate establishments across multiple sources
- Collect ratings, reviews, and practical information
- Document evidence in structured format
- Record source URLs and access dates
- Flag conflicts or uncertainties in data

**Authority**:
- Add candidates to discovery pipeline
- Request additional sources when needed
- Escalate unclear cases to Verification Agent

**Constraints**:
- Must not fabricate or infer data without clear evidence
- Must cite all sources with URLs
- Must collect minimum 4 sources per candidate (see Evidence Rules)

### Verification Agent
**Primary function**: Data validation and conflict resolution

**Responsibilities**:
- Cross-reference information from multiple sources
- Identify and resolve conflicts in evidence
- Verify practical constraints (hours, reservations, closures)
- Flag outdated or unreliable information
- Validate URL accessibility and accuracy

**Authority**:
- Request additional sources from Research Agent
- Mark evidence as "conflicting" or "uncertain"
- Escalate unresolvable conflicts for human review

**Constraints**:
- Cannot override source data without justification
- Must document all conflict resolution decisions
- Must preserve original conflicting evidence

### Scoring Agent
**Primary function**: Apply standardized scoring rubric

**Responsibilities**:
- Apply 50-point scoring system consistently (see Scoring System)
- Justify each score component with evidence
- Document scoring rationale in notes.md
- Update candidate scores in candidates.md
- Flag edge cases requiring human judgment

**Authority**:
- Assign scores 0-50 based on rubric
- Request additional evidence if insufficient for scoring
- Recommend exclusion for scores below thresholds

**Constraints**:
- Must follow rubric strictly (no subjective adjustments)
- Must justify every score with documented evidence
- Cannot score without minimum evidence requirements met

### Synthesis Agent
**Primary function**: Generate final recommendations

**Responsibilities**:
- Aggregate scored candidates into tiers (Top Picks, Backups)
- Create dining strategy across all recommendations
- Identify gaps in category or geographic coverage
- Generate practical guidance (reservation timing, budget allocation)
- Ensure progressive disclosure in documentation

**Authority**:
- Promote candidates to Top Picks (score ≥35)
- Designate Backups (score 30-34)
- Recommend additional research for underserved categories

**Constraints**:
- Cannot override scores from Scoring Agent
- Must respect score-based tier thresholds
- Must maintain geographic and category balance

### Human Review Agent
**Primary function**: Final approval and strategic decisions

**Responsibilities**:
- Review completion status for Taipei
- Approve or reject recommendations
- Override automated decisions when justified
- Provide strategic direction for research priorities
- Validate compliance with project scope

**Authority**:
- Full override authority on any decision
- Set research priorities and focus areas
- Approve final completion status
- Request re-research or additional candidates

**Constraints**:
- Overrides must be documented with rationale
- Cannot modify historical research data (audit trail)

---

## Quick Start Guide

**For new agents entering this project:**

1. **Understand context**: Read [Project Scope](#project-scope) above
2. **Learn structure**: Review [Repository Structure](#repository-structure) below
3. **Follow workflow**: Execute [Research Workflow](#research-workflow) step-by-step
4. **Apply standards**: Use [Scoring System](#scoring-system) and [Evidence Rules](#evidence-rules)
5. **Check progress**: Verify completion against [Completion Criteria](#completion-criteria)

---

## Repository Structure

### File Organization

The project uses a single city directory under `gourmet/` with exactly six files following the progressive disclosure principle:

```
gourmet/
  taipei/
    overview.md    - City context, strategy, research progress
    inbox.md       - Unresearched candidates awaiting investigation
    candidates.md  - All candidates in structured table format
    notes.md       - Detailed evidence and scoring rationale  
    excluded.md    - Rejected candidates with exclusion reasons
    top-places.md  - Final recommendations and dining strategy
```

### File Purposes and Reading Sequence

#### 1. overview.md
**Purpose**: Quick orientation and research strategy  
**Contains**: Travel context, group constraints, food highlights, research strategy, progress checklist

**Update trigger**: Project start, strategy changes, completion milestones

#### 2. inbox.md
**Purpose**: Temporary holding area for unresearched candidates  
**Contains**: Simple list (name, area, type, source), no scores

**Workflow**:
- Add here first when discovering candidates
- Move to candidates.md when starting research
- Remove from inbox after migration

**Migration rule**: Once moved to candidates.md with `status: researching`, remove from inbox.md.

#### 3. candidates.md
**Purpose**: Complete candidate inventory with status and scores  
**Contains**: Structured table with columns: name, category, area, type, google_maps_url, status, score, sources, notes

**Preservation rule**: NEVER delete entries. Use `status: rejected` and document in excluded.md instead.

**Status values**:
- `researching`: Active research in progress
- `shortlisted`: Scored and under consideration
- `top`: Promoted to Top Picks
- `rejected`: Excluded with reason in excluded.md

**Note**: Unresearched candidates stay in inbox.md until research begins.

#### 4. notes.md
**Purpose**: Detailed evidence trail and scoring justification  
**Contains**: Evidence sections per candidate (sources, ratings, reviews), scoring breakdown, practical information

#### 5. excluded.md
**Purpose**: Audit trail for rejected candidates  
**Contains**: Categorized exclusion reasons, source references, decision dates

#### 6. top-places.md
**Purpose**: Actionable final recommendations  
**Contains**: Top Picks (≥35), Backups (30-34), Dining Strategy, To-Do (trip execution)

### Progressive Disclosure Principle

**Information flow**: Each file serves a distinct abstraction level.

**Traveler journey**:
```
README.md → top-places.md → Make reservations
```

**Quick research scan**:
```
overview.md → inbox.md (未研究清單) → candidates.md (table scan) → Identify priorities
```

**Deep evidence review**:
```
candidates.md → notes.md → Validate scores and sources
```

**Exclusion investigation**:
```
excluded.md → [reason] → (Optional) notes.md for details
```

**Key principles**:
- Start with answers, then provide justification
- Link between files (don't duplicate content)
- Keep summary files concise (tables, bullet points)
- Bury detailed evidence in notes.md
- Respect reader's journey and time

**Anti-patterns** (avoid these):
- ❌ Mixing summary table with detailed evidence in candidates.md
- ❌ Duplicating scores across multiple files
- ❌ Hiding final picks deep in notes.md
- ❌ Deleting rejected candidates instead of documenting
- ❌ Creating files outside the six-file structure
- ❌ Keeping unresearched candidates in candidates.md (use inbox.md instead)
- ❌ Duplicating entries between inbox.md and candidates.md

### Template Files

**Location**: `templates/` directory at repository root

**Purpose**: Standardized starting points for city-specific research documentation.

**Available Templates** (6 files):

1. **overview.md** - City strategy, progress tracker
2. **inbox.md** - Unresearched candidates holding area
3. **candidates.md** - Structured table with all required columns
4. **notes.md** - Evidence collection and scoring breakdown
5. **top-places.md** - Final recommendations and dining strategy
6. **excluded.md** - Rejected candidates with reasons

**Usage**:

1. Create directory: `gourmet/[cityname]/`
2. Copy all 6 templates from `templates/`
3. Start with `overview.md` for context
4. Add discoveries to `inbox.md`
5. Move to `candidates.md` when starting detailed research
6. Replace placeholder text (marked with `[brackets]`)

**Reference**: See `templates/README.md` for detailed instructions.

---

## Research Workflow

### Workflow Overview

**Six-stage process** from discovery to completion:

```
Stage 0: Initialize    → Create overview.md, set strategy
Stage 1: Discovery     → Collect candidates in candidates.md  
Stage 2: Evidence      → Gather detailed evidence in notes.md
Stage 3: Scoring       → Apply 50-point rubric
Stage 4: Triage        → Accept or reject candidates
Stage 5: Synthesis     → Generate top-places.md
Stage 6: Completion    → Verify and document completion
```

### Stage 0: Initialize Research

**Objective**: Establish research foundation and strategy

**Actions**:
1. Set up directory structure:
   - Create directory: `gourmet/taipei/`
   - Copy templates from `templates/` directory (see [Template Files](#template-files))
   - This provides standardized starting structure for all six required files

2. Create `overview.md` (using template as starting point):
   - Group size constraints and dining context
   - Taipei-specific food highlights (e.g., a local noodle specialty)
   - Research priorities (cuisine types, group-friendly formats)
   - Progress checklist (initially empty)
   - Known constraints (business hours, holidays, group capacity)

3. Create `inbox.md` for quick candidate collection:
   - Simple list format ready for new discoveries
   - No detailed structure needed yet
   - Serves as scratchpad during initial searches

4. Gather initial candidate pool using web_search:
   - "best [local specialty dish] restaurants [year]" (e.g., "best [local noodle specialty] restaurants 2026")
   - "best [category] in Taipei" (e.g., "best hot pot Taipei")
   - "[specific dish] restaurants Taipei" (e.g., "[local specialty] restaurants Taipei")
   - Focus on recent guides (2024-2026) and local sources
   - **Add all discoveries to inbox.md immediately** to avoid forgetting

5. Batch similar searches (efficiency):
   - One search: main restaurants by cuisine type
   - One search: casual/local specialties
   - One search: large-group dining or private-room options

**Success criteria**:
- City directory created with all template files
- overview.md created with complete sections
- inbox.md populated with 15-25 initial candidates
- Research priorities documented

**Time estimate**: 30 minutes

---

### Stage 1: Discovery and Candidate Collection

**Objective**: Build comprehensive candidate list, moving from inbox.md to structured candidates.md

**Actions**:
1. Review `inbox.md` for all discovered candidates

2. Select 3-5 highest-priority candidates to move to `candidates.md`:
   - Consider: local specialties, high ratings, Michelin mentions, 百名店
   - Group capacity confirmed (16-20 people)
   - Good geographic distribution

3. For each selected candidate, add to `candidates.md` table with required fields:
   - `name`: Restaurant name (prefer original language)
   - `category`: restaurant
   - `area`: Neighborhood or district
   - `type`: Cuisine or specialty (e.g., hot pot, beef noodles)
   - `google_maps_url`: Google Maps link (search link acceptable initially)
   - `status`: **researching** (starts here after moving from inbox)
   - `score`: - (empty initially)
   - `sources`: Brief list (e.g., "食べログ, Google Maps")
   - `notes`: One-line summary (expanded in notes.md later)

4. **Remove moved candidates from inbox.md** to prevent duplication

5. Continue adding new discoveries to inbox.md as they're found

**Preservation rules**:
- NEVER delete candidates from candidates.md table
- Acceptable modifications only:
  - Merge duplicates
  - Correct errors
  - Note permanent closures
- Unwanted candidates: Set `status: rejected`, document in excluded.md

**Success criteria**:
- 3-5 candidates moved from inbox.md to candidates.md
- All moved candidates have `status: researching`
- inbox.md updated (moved candidates removed)
- Remaining candidates stay in inbox.md for future prioritization

**Time estimate**: 20-30 minutes

---

### Stage 2: Evidence Collection

**Objective**: Gather detailed, multi-source evidence for prioritized candidates

**Actions per candidate**:

1. **Search pattern** (efficient):
   - Single comprehensive query: "[Restaurant Name] Taipei 食べログ 予約 口コミ"
   - Often returns Google Maps, 食べログ, and Google口コミ together
   - Extract systematically: ratings, review counts, prices, hours, pros/cons

2. **Create evidence section in notes.md**:
   ```markdown
   ### [Place Name]
   
   **Official**: [website URL or "no official website"]
   
   **Google Maps**: X.X/5 (Y reviews)
   
   **食べログ (Tabelog)**: X.X/5 (Y reviews)
   - [URL]
   - 夜予算/昼予算: [price range]
   - Area ranking: [e.g., "Taipei [cuisine] ranking #3"]
   
   **Other ratings**: [Retty, Hot Pepper Gourmet, etc.]
   
   **Guide sources**:
   - [URLs with brief descriptions]
   
   **Google口コミ patterns**:
   - [Summarize recurring themes from Japanese reviews]
   
   **Recurring pros**:
   - [List from multiple sources]
   
   **Recurring cons**:
   - [List from multiple sources]
   
   **Practical**:
   - reservation requirement: [required|recommended|optional|none|unknown]
   - best visiting time: [specific times or "off-peak"]
   - closed days: [day of week or "irregular"]
   - queue: [expected wait time or "none"]
   - group capacity: [if researching for large groups, specify max capacity and private room availability]
   ```

3. **Update candidates.md**:
   - Status already set to `researching` (from Stage 1)
   - Add brief summary to `notes` column (keep concise)
   - Keep `score` empty until Stage 3

**Evidence rules** (see [Evidence Rules](#evidence-rules) for details):
- Minimum 4 sources required: Google Maps + 食べログ + Google口コミ + Guide
- Optional additional sources: Retty, Hot Pepper Gourmet, travel blogs
- Document conflicts clearly
- Mark uncertain information as `unknown`
- Include actual URLs (not placeholders)

**Tabelog ranking importance**:
- Always check score (0-5.0 scale) and area ranking
- 4.0+ = Exceptional (0.07% of restaurants)
- 3.5-3.9 = Excellent
- 3.0-3.4 = Good
- Search: "[City] [cuisine type] 食べログ ランキング"
- Note: Score-based ranking ≠ 百名店 (annual award)

**Success criteria**:
- Evidence section created in notes.md
- Minimum 4 sources collected with URLs
- Practical constraints documented
- Conflicts or uncertainties noted
- candidates.md updated with `status: researching`

**Time estimate**: 15-20 minutes per candidate

---

### Stage 3: Scoring

**Objective**: Apply standardized 50-point rubric consistently

**Actions**:

1. **Score each candidate** using rubric (see [Scoring System](#scoring-system)):
   - Taste/Quality (0-10): Food quality, authenticity, execution
   - Value (0-10): Price vs quality, portion size
   - Convenience (0-10): Location, reservation ease, hours
   - Consistency (0-10): Reliability across reviews/time
   - Risk (0-10): Low risk=10; disappointment likelihood, queue uncertainty

2. **Document scoring in notes.md**:
   ```markdown
   **Score (50-point rubric)**:
   - Taste/Quality: X/10 [justification with evidence]
   - Value: X/10 [justification with evidence]
   - Convenience: X/10 [justification with evidence]
   - Consistency: X/10 [justification with evidence]
   - Risk: X/10 [justification with evidence]
   - **Total**: XX/50
   ```

3. **Update candidates.md**:
   - Enter total score in `score` column
   - Keep `status: researching` (changed in Stage 4)

**Scoring requirements**:
- Every score must be justified from documented evidence
- No subjective adjustments outside rubric
- Document edge cases or difficult judgment calls
- Maintain consistency across all candidates

**Success criteria**:
- All five components scored with justification
- Total score calculated
- Scoring rationale documented in notes.md
- Score added to candidates.md table

**Time estimate**: 5-10 minutes per candidate (after evidence collected)

---

### Stage 4: Triage and Exclusion

**Objective**: Decide candidate fate based on scores and evidence

**Decision rules**:

| Score Range | Status | Action |
|-------------|--------|--------|
| ≥35 | `shortlisted` or `top` | Promote to Top Pick |
| 30-34 | `shortlisted` | Consider as Backup |
| <30 | `rejected` | Exclude with documented reason |

**Hard exclusion triggers** (regardless of score):
- Multi-source evidence of tourist trap
- Repeated hygiene or safety concerns
- Severe service issues across multiple sources

**Exclusion process**:
1. Update candidates.md: Set `status: rejected`
2. Create/update excluded.md:
   - Add to appropriate category section
   - Document exclusion reason with evidence
   - Include source references
   - Note date of decision

3. Preserve candidates.md entry (do NOT delete)

**Exclusion categories** (for excluded.md):
- Low Score (<30)
- Tourist Trap (evidence-based)
- Safety/Hygiene Concerns
- Service Issues
- Practical Constraints (location, hours)
- Not Researched Further (deprioritized)

**Success criteria**:
- All researched candidates triaged
- No `status: researching` remains after triage
- Rejected candidates documented in excluded.md
- Exclusion reasons clearly stated with evidence

**Time estimate**: 5 minutes per candidate

---

### Stage 5: Synthesis and Final Recommendations

**Objective**: Generate actionable recommendation list with strategy

**Actions**:

1. **Create/update top-places.md** with sections:

   **Section 1: Top Picks** (score ≥35)
   - List in descending score order
   - Include for each: name, type, area, score, Google Maps link, Tabelog link, one-line justification, constraints

   **Section 2: Backups** (score 30-34)
   - List in descending score order
   - Same information format as Top Picks

   **Section 3: Dining Strategy**
   - Time planning (lunch/dinner hours, local customs)
   - Reservation strategy (which need booking, how far ahead)
   - Budget allocation (price ranges by category)
   - Transportation (how to reach areas from hotel)

   **Section 4: To-Do** (trip execution checklist)
   - Reservation tasks
   - Information confirmation
   - Day-of preparation
   - Note: This is NOT research completion tracking

2. **Balance recommendations**:
   - Target: 5-10 Top Picks for Taipei
   - Target: 3-5 Backups for Taipei
   - Ensure variety across cuisines and dining formats (casual vs special occasion, private rooms)
   - Ensure geographic spread across Taipei

3. **Link requirements**:
   - Google Maps: Valid URL (direct link or search link)
   - Tabelog: Valid URL or "no tabelog listing"
   - Test all links before finalization

**Success criteria**:
- top-places.md created with all four sections
- Top Picks and Backups sorted by score
- All required information included per place
- All links valid and tested
- Dining Strategy practical and complete
- Balance across categories and geography

**Time estimate**: 30-45 minutes

---

### Stage 6: Completion and Documentation

**Objective**: Verify completion and update tracking systems

**Actions**:

1. **Verify completion criteria** (see [Completion Criteria](#completion-criteria)):
   - ✅ All candidates triaged (no `status: inbox` in candidates.md)
   - ✅ No pending decisions in excluded.md
   - ✅ top-places.md finalized with all sections
   - ✅ overview.md checklist fully marked `[x]`

2. **Run verification commands** (from PROGRESS.md):
   ```bash
   grep "status: inbox" gourmet/taipei/candidates.md  # Should return nothing
   grep -i "TODO\|pending" gourmet/taipei/excluded.md  # Should return nothing
   grep -E "^## (Top Picks|Backups|Dining Strategy|To-Do)" gourmet/taipei/top-places.md  # Should find all 4
   grep "\- \[ \]" gourmet/taipei/overview.md  # Should return nothing
   ```

3. **Update progress tracking**:
   - Update PROGRESS.md status (⏳ → 📝 → 🔄 → ✅)
   - Sync README.md progress table
   - Update recommendation counts
   - Add completion notes

4. **Document improvements** (if applicable):
   - Add efficiency tips to this document
   - Note common patterns discovered
   - Update workflow if process improved

**Completion status definitions**:
- ⏳ Not Started: No files created yet
- 📝 In Progress: Files exist, research ongoing
- 🔄 Needs Finalization: Research done, needs review/finalization
- ✅ Completed: All criteria met

**Critical distinction - Two checklists**:

| Checklist | Purpose | Location | Completion Rule |
|-----------|---------|----------|----------------|
| **Research Completion** | Track research progress | overview.md | MUST be 100% `[x]` for ✅ |
| **Trip Execution** | Track trip planning | top-places.md To-Do | MAY have `[ ]` when research complete |

**Success criteria**:
- All verification commands pass
- PROGRESS.md and README.md updated
- Status set to ✅ only if all criteria met

**Time estimate**: 10-15 minutes

---

## Evidence Rules

### Minimum Source Requirements

**Every researched candidate must have evidence from at least 4 independent sources:**

1. **Google Maps** (required)
   - Overall rating (X.X/5)
   - Review count
   - Recurring themes from reviews (pros/cons)

2. **食べログ (Tabelog)** (required)
   - Score (0-5.0 scale)
   - Review count
   - Price range (夜予算/昼予算)
   - Area ranking if available

3. **Google口コミ** (required)
   - Japanese-language reviews
   - Local reviewer perspectives
   - Recurring patterns across multiple reviews

4. **Food/Travel Guide** (required - at least one)
   - Michelin Guide
   - Local Japanese food blogs
   - TimeOut Tokyo, VOKKA, Hitosara
   - 百名店 (annual award)
   - Regional tourism sites

**Optional additional sources** (strengthen evidence):
- Retty (レッティ)
- Hot Pepper Gourmet (ホットペッパーグルメ)
- Chinese-language travel blogs (PTT, Dcard) - cite clearly
- Social media aggregates (TikTok, Instagram) - verify authenticity

### Source Quality Standards

**URL requirements**:
- Include actual URLs (not "see website")
- Verify links are accessible
- Note if behind paywall or login
- Document access date for time-sensitive info

**Citation format**:
```markdown
**Source name**: [Brief finding]
- URL: [actual link]
- Accessed: YYYY-MM-DD
```

**Source reliability assessment**:
- Prefer sources with large review counts (100+ reviews)
- Cross-reference claims across 3+ sources
- Flag sources that are outliers
- Note if source is outdated (>2 years old)

### Conflict Handling

**When sources disagree**:

1. **Document the conflict explicitly**:
   ```markdown
   **Conflict noted**: 
   - Source A claims: [X]
   - Source B claims: [Y]
   - Source C claims: [X]
   ```

2. **Resolution strategy**:
   - Majority consensus: Use most common finding
   - Recency: Prefer more recent information
   - Detail: Prefer source with more specific detail
   - Scale: Weight by review count/authority

3. **When unresolvable**:
   - Mark as `unknown` or `conflicting`
   - Document both sides
   - Flag for human review
   - Note in scoring rationale

**Common conflict types**:
- Hours of operation (check official website first)
- Reservation requirements (call restaurant if critical)
- Price ranges (use most recent, note inflation)
- Service quality (look for trend over time)

### Uncertainty Documentation

**Mark information as uncertain when**:
- Only one source provides the information
- Sources conflict without clear resolution
- Information is outdated (>1 year for restaurants)
- Seasonal variation possible

**Uncertainty labels**:
- `unknown`: No reliable source found
- `conflicting`: Sources disagree, no clear resolution
- `unverified`: Single source only, not confirmed
- `seasonal`: May vary by season (note which season)
- `outdated`: Based on old information (note date)

**Example**:
```markdown
**Practical**:
- reservation requirement: required (per Tabelog, unverified on official site)
- best visiting time: conflicting (some say lunch, some say dinner)
- closed days: unknown (no clear information)
- queue: 30-60 min (based on 2024 reviews, may be outdated)
```

### Seasonality Considerations

**Seasonal factors to document**:

1. **Ingredients**: Note peak seasons for key ingredients (seafood, produce)
2. **Tourist volume**: Peak vs off-peak seasons affecting availability
3. **Operating hours/closures**: Seasonal menu items, holiday closures

**Documentation requirement**:
```markdown
**Seasonal notes**:
- Best visit season: [season] because [reason]
- Trip timing: [date range] = [適切/注意が必要/オフシーズン]
- Seasonal considerations: [specific factors]
```

### Data Fabrication Prevention

**Strictly prohibited**:
- ❌ Inventing ratings or review counts
- ❌ Inferring facts not stated in sources
- ❌ Mixing personal opinion with source data
- ❌ Claiming consensus without evidence

**Acceptable synthesis**:
- ✅ Summarizing patterns across multiple reviews
- ✅ Noting common themes (with citation)
- ✅ Calculating averages from multiple sources (show calc)
- ✅ Drawing logical conclusions (clearly marked as inference)

**Boundary examples**:
- ❌ "This restaurant is popular" (subjective without evidence)
- ✅ "High review volume (2000+ on Tabelog) suggests popularity"
- ❌ "Service is excellent" (opinion)
- ✅ "12 of 15 recent reviews mention friendly service"

---

## Scoring System

### Rubric Definition

**Total: 50 points** across five equally-weighted categories.

#### 1. Taste / Quality (0-10 points)

**Intent**: Measure food quality, authenticity, and execution.

**Scoring guidelines**:
- **9-10**: Exceptional. Michelin-starred, 百名店, or Tabelog 4.0+. Consistently praised for outstanding flavors
- **7-8**: Excellent. Tabelog 3.5-3.9, strong reviews, specific dishes highly praised
- **5-6**: Good. Tabelog 3.0-3.4, generally positive, solid execution
- **3-4**: Acceptable. Mixed reviews, some quality concerns
- **0-2**: Poor. Multiple complaints, quality issues

**Evidence required**:
- Tabelog score
- Google Maps rating
- Specific dish mentions from reviews
- Guide recognition (Michelin, 百名店) if any

**Example scoring**:
```
Taste/Quality: 10/10
Evidence: Tabelog 4.00 (top 0.07%), 百名店 3 consecutive years,
reviews consistently praise 備長炭焼き technique
```

#### 2. Value (0-10 points)

**Intent**: Measure price-to-quality ratio and portion adequacy.

**Scoring guidelines**:
- **9-10**: Outstanding value. High quality at low/moderate price. Generous portions
- **7-8**: Good value. Fair pricing for quality delivered
- **5-6**: Acceptable. Slightly expensive but justified by quality
- **3-4**: Poor value. Overpriced for quality
- **0-2**: Very poor value. Significantly overpriced

**Evidence required**:
- Price range from Tabelog (夜予算/昼予算)
- Google Maps price level (¥/¥¥/¥¥¥/¥¥¥¥)
- Review mentions of portions or value
- Comparison to similar establishments

**Example scoring**:
```
Value: 7/10
Evidence: ¥15,000-20,000 per person (fine dining), but exceptional
quality justifies price per multiple reviews
```

#### 3. Convenience (0-10 points)

**Intent**: Measure ease of access, reservation process, and operating hours.

**Scoring guidelines**:
- **9-10**: Highly convenient. Central location, walk-in friendly, long hours, near hotel
- **7-8**: Convenient. Accessible location, easy reservation, reasonable hours
- **5-6**: Moderate. Requires some planning, reservation needed, standard hours
- **3-4**: Inconvenient. Difficult access, complex reservation, limited hours
- **0-2**: Very inconvenient. Remote location, strict requirements, very limited hours

**Factors to consider**:
- Distance from hotel or major sites
- Reservation requirement (none/optional/recommended/required)
- Reservation difficulty (easy/moderate/difficult)
- Operating hours (flexible vs restrictive)
- Transportation access

**Example scoring**:
```
Convenience: 6/10
Evidence: 完全予約制 (reservation required), 18:00 synchronized start
(inflexible), 遅刻厳禁 (no-late policy), but walkable from station
```

#### 4. Consistency (0-10 points)

**Intent**: Measure reliability and stability over time.

**Scoring guidelines**:
- **9-10**: Highly consistent. Long-established, stable high ratings, minimal complaints
- **7-8**: Consistent. Reliable quality, few negative reviews
- **5-6**: Moderately consistent. Some variation noted, generally reliable
- **3-4**: Inconsistent. Mixed experiences, hit-or-miss quality
- **0-2**: Unreliable. Frequent complaints, significant variation

**Evidence required**:
- Rating stability over time (if data available)
- Review count (more reviews = more reliable signal)
- Establishment age/history
- Pattern of complaints (recurring vs isolated)
- 百名店 streak (indicates sustained quality)

**Example scoring**:
```
Consistency: 10/10
Evidence: 百名店 3 consecutive years, Tabelog score stable at 4.0,
4000+ reviews maintain high rating, few service complaints
```

#### 5. Risk (0-10 points)

**Intent**: Measure likelihood of disappointment or problems. **Higher score = lower risk.**

**Scoring guidelines**:
- **9-10**: Very low risk. Reliable, predictable, few ways to go wrong
- **7-8**: Low risk. Generally safe choice, minor uncertainties
- **5-6**: Moderate risk. Some factors that could lead to disappointment
- **3-4**: Higher risk. Significant potential issues
- **0-2**: High risk. Multiple red flags or major concerns

**Risk factors** (reduce score):
- Long unpredictable queues
- Strict cancellation policies
- Service quality complaints
- Tourist trap signals
- Seasonal closures during trip
- Unclear reservation process
- Location difficult to find

**Risk mitigators** (increase score):
- Confirmed reservation possible
- Consistent positive reviews
- Clear operating information
- Backup options nearby

**Example scoring**:
```
Risk: 9/10
Evidence: Reservation required (eliminates queue uncertainty),
strict cancellation policy but ensures quality control,
consistent reviews indicate reliable experience
```

### Tier Definitions

**Based on total score (0-50):**

| Score Range | Tier | Meaning | Action |
|-------------|------|---------|--------|
| 40-50 | Top Pick | Exceptional, highly recommended | Promote to Top Picks, prioritize |
| 35-39 | Top Pick | Very good, solid choice | Promote to Top Picks |
| 30-34 | Backup | Good, acceptable alternative | Promote to Backups |
| 25-29 | Marginal | Borderline, needs justification | Consider rejection |
| 0-24 | Reject | Below standard | Reject with documentation |

**Tier targets for Taipei**:
- **Top Picks**: 5-10 candidates (quality over quantity)
- **Backups**: 3-5 candidates (alternatives if Top Picks unavailable)
- **Total recommendations**: 8-15 (avoid overwhelming)

### Scoring Consistency Rules

**To maintain consistency across candidates**:

1. **Score relative to category**:
   - Compare restaurants within similar price bands or styles
   - Don't penalize casual places for not being fine dining
   - Adjust expectations by price band

2. **Document edge cases**:
   - Note when scoring difficult or borderline
   - Flag for human review if uncertain
   - Explain unusual scores in rationale

3. **Avoid score inflation**:
   - Not every place can be 40+
   - Reserve 9-10 components for truly exceptional
   - A score of 35-38 is still very good

4. **Justify every component**:
   - Every 0-10 score must cite specific evidence
   - No scores without documented rationale
   - Refer to evidence section in notes.md

---

## Decision Rules

### Promotion Thresholds

**Automatic promotion**:
- Score ≥35: Promote to Top Pick
- Score 30-34: Promote to Backup

**Requires justification**:
- Score <30 but promoting anyway (explain why)
- Score ≥35 but not promoting (explain why)

### Exclusion Thresholds

**Automatic exclusion**:
- Score <25: Below quality standard
- Hard exclusion triggers (see below)

**Requires consideration**:
- Score 25-29: Marginal, needs discussion
- Score 30-34 with red flags: May exclude despite score

**Hard exclusion triggers** (override score):
1. **Tourist trap**: Multi-source evidence of inflated prices/quality
2. **Safety/hygiene**: Multiple food safety/cleanliness complaints
3. **Service**: Consistent severe service issues
4. **Practical**: Inaccessible location or always closed during visit

### Human Override Conditions

**Human reviewer may override when**:

1. **Strategic**: Geographic/cuisine coverage needs, historical/cultural significance
2. **Context not in rubric**: Unique experience, once-in-lifetime opportunity, trusted recommendation
3. **Edge cases**: Conflicting evidence, new establishment with promise, legendary place debate

**Override documentation**:
```markdown
**Human override**: [Decision]
**Reason**: [Justification]
**Date**: YYYY-MM-DD
**Reviewer**: [Name/ID]
```

### Documentation Requirements

**Every decision must be documented**:

1. **Promoted (Top Pick/Backup)**: Score breakdown, evidence, constraints, justification if borderline
2. **Rejected**: Score (if calculated), exclusion reason, evidence, entry in excluded.md
3. **Deprioritized** (not researched): Reason, entry in excluded.md under "Not Researched Further"
4. **Human overrides**: Original score/decision, override, justification, date, reviewer

**Traceability**: Every decision traceable to documented evidence or explicit judgment.

---

## Completion Criteria

### Definition of Done

**Taipei marked "✅ Completed" when ALL met:**

1. **All candidates triaged**: No `status: inbox` in candidates.md; all scored or excluded
2. **No pending decisions**: excluded.md has no "TODO"/"pending"; all reasons documented
3. **top-places.md finalized**: Top Picks (≥35), Backups (30-34), Dining Strategy, To-Do complete
4. **overview.md checklist complete**: Research completion checklist fully `[x]`

### Checklist Distinction

| Checklist | Purpose | Location | Completion Rule |
|-----------|---------|----------|----------------|
| **Research Completion** | Track research progress | overview.md | MUST be 100% `[x]` for ✅ |
| **Trip Execution** | Track trip planning | top-places.md To-Do | MAY have `[ ]` when research complete |

**Critical**: Only Research Completion checklist determines "✅ Completed" status.

### Verification Commands

**Run these commands to verify completion**:

```bash
# Should return nothing (no inbox entries)
grep "status: inbox" gourmet/taipei/candidates.md

# Should return nothing (no pending decisions)
grep -i "TODO\|pending" gourmet/taipei/excluded.md

# Should find all 4 sections
grep -E "^## (Top Picks|Backups|Dining Strategy|To-Do)" gourmet/taipei/top-places.md

# Should return nothing (no unchecked research items)
grep "\- \[ \]" gourmet/taipei/overview.md
```

**Expected results for Taipei completed**:
- No inbox entries found ✓
- No pending decisions found ✓
- All 4 sections found in top-places.md ✓
- No unchecked items in overview.md ✓

### Status Progression

**Status indicators**:
- ⏳ **Not Started**: No files created yet
- 📝 **In Progress**: Files exist, research ongoing, some candidates still inbox
- 🔄 **Needs Finalization**: Research done, all triaged, needs review/top-places.md completion
- ✅ **Completed**: All verification commands pass

**Progression rules**:
- Only advance status when criteria met
- Can regress if new candidates added or issues found
- Document status changes in PROGRESS.md

---

## Auditability and Maintenance

### Audit Trail Requirements

**Every research action must leave a trace**:

1. **Candidate addition**: Entry in candidates.md (timestamped in commit), `status: inbox`
2. **Research conducted**: Evidence in notes.md with URLs/dates, `status: researching`
3. **Scoring**: Breakdown in notes.md, total in candidates.md, justification for each component
4. **Triage decision**: Status updated (`shortlisted`/`top`/`rejected`); if rejected, entry in excluded.md
5. **Changes**: Document reason, update files, note in commit

**Preservation rules**:
- ❌ NEVER delete candidates, evidence, or exclusion reasons
- ✅ Mark as rejected and document; preserve historical scores if recalculated

### Maintenance Procedures

**Regular maintenance**:

1. **Weekly** (active research): Update PROGRESS.md, sync README.md, review inbox
2. **Per city completion**: Run verification, update status, document completion date
3. **Pre-trip**: Verify hours, confirm seasonal availability, update closures

**Version control**: Commit meaningful units of work with descriptive messages

### Quality Assurance

**Self-review checklist before marking complete**:

- [ ] All candidates scored or excluded with documented reasons
- [ ] All sources have working URLs
- [ ] No fabricated data; conflicts resolved; uncertainty marked
- [ ] top-places.md complete with all sections
- [ ] Dining Strategy practical and complete
- [ ] All verification commands pass
- [ ] PROGRESS.md and README.md updated

**Peer review focus areas**: Score consistency, evidence quality, exclusion clarity, category coverage, practical accuracy

### Continuous Improvement

**Learning capture**: Document improvements, patterns, pitfalls in this file

**Feedback incorporation**: Track overrides, adjust scoring for bias, refine ambiguous rubric, update source requirements

---

## Process Improvements (Lessons Learned)

### Efficient Research Tactics

1. **Start with overview.md** for context
2. **Batch web searches**: Gather 20+ candidates in 3-4 searches
3. **Prioritize ruthlessly**: Research top 3-5 first, expand if needed
4. **Use comprehensive queries**: "[Place] [City] 食べログ 予約 口コミ" yields most info
5. **Document incrementally**: Prevent information loss

**Time estimates**:
- Overview + candidate collection: 30 minutes
- Detailed research per place: 15-20 minutes
- Scoring per place: 5-10 minutes
- Synthesis (top-places.md): 30-45 minutes
- **Total for Taipei**: 4-6 hours for 10 candidates

### Pattern Recognition

**Tourist trap signals**:
- Located only near major attractions
- Generic positive reviews; no local mentions
- Price significantly higher than similar places

**Authentic signals**:
- High Tabelog ratings from verified locals
- Mixed tourist/local clientele; family-owned
- Specific dishes praised repeatedly
- Reservation difficulty (genuine popularity)

**Red flags**:
- Service complaints (>20% of reviews)
- Multiple hygiene issues
- Irregular hours; conflicting information

**Green flags**:
- Michelin/百名店 recognition
- Tabelog 3.8+ with 500+ reviews
- Specific technique/ingredient praised
- Consistent experience over time

### Common Pitfalls

**Avoid these mistakes**:

1. **Research sprawl**: Focus on prioritized candidates, not everything at once
2. **Skipping overview.md**: Context essential for prioritization
3. **Single-source reliance**: Always cross-reference 4+ sources
4. **Ignoring constraints**: Reservation policies, closed days, queues matter
5. **Score inflation**: Reserve 9-10 scores for truly exceptional; 35-38 is excellent
6. **Poor coverage**: Ensure geographic spread and format variety (casual/upscale)
7. **Deleting records**: Use excluded.md to preserve audit trail

### Research Quality Checklist

**Before finalizing any candidate, verify**:

- ✓ Exact ratings and review counts (Google Maps, Tabelog)
- ✓ Patterns from Japanese reviews (Google口コミ)
- ✓ Signature dishes identified
- ✓ Reservation requirements and closed days confirmed
- ✓ Wait time documented (if no reservation)
- ✓ Price range verified (Tabelog 夜予算/昼予算)
- ✓ Common complaints noted
- ✓ Clientele assessed (tourist vs local)
- ✓ Seasonal considerations documented (if applicable)

---

## Documentation Standards

### Language Rules

Language requirements are defined in `CONSTITUTION.md`. This section does not restate them.

### File Naming Conventions

**Repository-level files**:
- `AGENTS.md`: This file
- `PROGRESS.md`: Research progress tracker
- `README.md`: Project overview (see `CONSTITUTION.md` for language requirements)

**City directory name**:
- `taipei`
- Lowercase, no spaces, no special characters

**City file names** (standardized, no variation):
- `overview.md`
- `candidates.md`
- `notes.md`
- `excluded.md`
- `top-places.md`

### Date and Data Formats

**Dates**: ISO 8601 format (YYYY-MM-DD)
- Example: `YYYY-MM-DD`

**Unknown information**: Use `unknown` (not "-", "TBD", "N/A", etc.)

**Scores**: Always show as fraction
- Example: 42/50 (not "42" alone)

**URLs**: Full URLs, not shorteners
- Example: `https://tabelog.com/mie/...` (not `bit.ly/...`)

### Markdown Standards

**Headers**: Use ATX style (`#`, `##`, etc.)

**Lists**: 
- Unordered: `-` (not `*` or `+`)
- Ordered: `1.`, `2.`, etc.

**Emphasis**:
- Bold: `**text**`
- Italic: `*text*`
- Code: `` `text` ``

**Links**: Use reference or inline, not automatic
- Inline: `[text](url)`
- Reference: `[text][ref]` with `[ref]: url` below

**Tables**: Use standard Markdown tables with alignment

---

## Appendix: Quick Reference

### Research Workflow Checklist

- [ ] **Stage 0: Initialize**
  - [ ] Create `gourmet/taipei/` and copy templates from templates/
  - [ ] Create overview.md
  - [ ] Conduct initial web searches
  - [ ] Gather 15-25 candidates
  
- [ ] **Stage 1: Discovery**
  - [ ] Use candidates.md template
  - [ ] Add all candidates with status: inbox
  - [ ] Prioritize top 3-5

- [ ] **Stage 2: Evidence**
  - [ ] Research prioritized candidates
  - [ ] Create notes.md evidence sections
  - [ ] Collect minimum 4 sources per candidate
  - [ ] Update candidates.md status

- [ ] **Stage 3: Scoring**
  - [ ] Apply 50-point rubric
  - [ ] Document scoring in notes.md
  - [ ] Update candidates.md scores

- [ ] **Stage 4: Triage**
  - [ ] Promote candidates ≥30 score
  - [ ] Reject candidates <30 or with red flags
  - [ ] Document exclusions in excluded.md

- [ ] **Stage 5: Synthesis**
  - [ ] Create top-places.md
  - [ ] Organize Top Picks and Backups
  - [ ] Write Dining Strategy
  - [ ] Add To-Do (trip execution)

- [ ] **Stage 6: Completion**
  - [ ] Run verification commands
  - [ ] Update PROGRESS.md
  - [ ] Sync README.md
  - [ ] Mark complete (✅) only if all criteria met

### Essential URLs

- **PROGRESS.md**: Detailed completion criteria and verification commands
- **README.md**: Traveler-facing project overview (see `CONSTITUTION.md` for language requirements)
- **templates/**: Template files for creating new city research documentation
- **City directory**: `gourmet/taipei/`

### Key Principles

1. **Evidence-based**: Every decision backed by documented sources
2. **Traceable**: Full audit trail preserved
3. **Comparable**: Standardized scoring across all candidates
4. **Actionable**: Clear recommendations with practical constraints
5. **Maintainable**: Consistent structure and documentation
6. **Progressive disclosure**: Information layered by abstraction level

---

*Document version: 2.0*  
*Last updated: `YYYY-MM-DD`*  
*Purpose: Single source of truth for gourmet research operations*
