# I&C Engineering Copilot

**An AI-assisted engineering system for Instrumentation & Control projects: it checks deliverables against
each other, answers specification questions with clause citations, and generates test documents from the
engineering data — with the engineer in control.**

> Personal project by **Kidane Berhane**, I&C engineer, developed on personal time with personal resources.
> Everything shown here runs on **DEMO-GPF**, a *fictional* gas processing facility designed for testing.
> No company, client or project data is used. The engine source is kept private; this repository shows the
> design, the results and real outputs. A walkthrough or code review is available on request.

---

## Why

On an I&C project the same information lives in many deliverables — instrument index, I/O list, alarm and
trip list, SIF register, cause & effect matrices, sequence lists — and they drift apart. Engineers spend
days cross-checking them by hand, and test documents (loop checks, FAT procedures) are retyped from the
same data. Errors that slip through surface late, at FAT or site, where they are expensive.

## How it works

Four principles, taken from functional-safety practice:

1. **Engineering data is the source of truth.** Client Excel deliverables are imported into one structured
   data model through configurable mapping profiles. Documents are generated *from* the data.
2. **Deterministic code does the checking and the arithmetic.** 44 consistency rules are written as data
   (YAML), each linked to the governing specification clause. No language model grades its own work.
3. **Claude is the reasoning and drafting layer.** Through MCP tool servers it triages findings, answers
   questions with citations — or says *"not found"* — and drafts narrative text.
4. **Engineers stay in control.** Every output is marked as an unchecked draft with a generation record
   (data snapshot, source revisions, rule-pack and template versions). The copilot never signs.

```mermaid
flowchart LR
    subgraph Inputs["Client deliverables (Excel) + specifications"]
        A1[Instrument index]:::d
        A2[I/O list]:::d
        A3[Alarm & trip list]:::d
        A4[SIF register]:::d
        A5[ESD and F&G C&E]:::d
        A6[BMS sequence]:::d
        A8[OT network: zones, firewall, assets]:::d
        A7[Client spec, design basis]:::d
    end
    Inputs -->|mapping profiles| DB[(Engineering<br/>database)]
    A7 -->|clause-level index| KB[(Knowledge<br/>index)]
    DB --> R[Rule engine<br/>44 rules as data]
    DB --> G[Test generator<br/>deterministic]
    KB --> S[Cited search]
    R & G & S -->|MCP tools| C{{Claude}}
    C --> O1[Findings triage]
    C --> O2[Cited answers / not found]
    C --> O3[Loop check sheets, FAT procedure]
    C --> O4[Compliance matrix]
    O1 & O2 & O3 & O4 --> E[Engineer review<br/>and approval]
    classDef d fill:#eef2f8,stroke:#9aa8bf
```

**Stack:** Python · SQLite (PostgreSQL-ready schema) · YAML rule packs, mapping profiles and document
templates · three MCP servers (`tagdb`, `docsearch`, `docgen`) · Claude via Claude Code / desktop · Skills
for engineering procedures · openpyxl / python-docx rendering · unittest test suite.

## What it does

### 1. Cross-document consistency checker

Duplicate tags and I/O addresses, range and unit mismatches (incl. barg/bara), alarm hierarchy and
setpoints within range, trip setpoints identical across SIF register, C&E and alarm list, SIS/BPCS/FGS
segregation, IS devices on IS channels, Ex certification vs area classification, redundant devices on
separate cards, SIF voting vs SIL, C&E completeness and safe positions, BMS purge and trial-for-ignition
times, F&G detector setpoints and 2ooN confirmation, spare capacity, fiscal meter run completeness.

**OT network (IEC 62443):** every permitting firewall rule maps to a documented conduit, no 'any' rules
into zones of SL-T 2 and above, untrusted zones reach control and safety zones only through the DMZ,
zone security levels meet the client minimum, asset inventory complete, firmware checked against
vendor advisories.

![Consistency findings](docs/images/consistency-findings.png)

### 2. Specification assistant

Clause-level index of the client specification and design basis (revision-aware, superseded revisions
excluded, one index per project). Hybrid keyword + identifier search finds tags, clause numbers and
voting expressions verbatim; answers cite the clause or say *"not found in the project documents"*.
A requirements register and compliance matrix are built from the spec and linked to the checker results.

![Spec assistant](docs/images/spec-assistant-answer.png)

### 3. Test document generator

Loop check sheets and a FAT procedure generated from the database: 5-point calibration checks in mA,
alarm setpoints converted to mA, Pt100 resistances per IEC 60751, NAMUR line-fault checks, C&E functional
tests with the full **MooN voting matrix** (M−1 initiators must not trip, M must trip), F&G voting groups,
BMS sequence tests with negative cases, and IEC 62443 network tests (a positive and a default-deny
negative test per firewall rule, zone isolation scans from untrusted zones, firmware verification). Open data issues are printed on the affected loop or test.

| C&E tests with voting matrix | BMS sequence test |
|---|---|
| ![FAT C&E voting](docs/images/fat-ce-voting.png) | ![FAT BMS sequence](docs/images/fat-bms-sequence.png) |

![Loop check sheet](docs/images/loop-check-sheet.png)

![FAT network tests](docs/images/fat-network-tests.png)

## Results on the demo plant

| Measure | Result |
|---|---|
| Plant scope | Units 100–900: inlet & HIPPS, separation, compression (API 670 MPS, anti-surge), TEG dehydration with burner management, fiscal metering, condensate & produced water, flare & blowdown, fire & gas, ESD, and the OT network (zones, conduits, firewalls, asset inventory) |
| Data volume | 166 instruments · 393 I/O channels · 112 alarms/trips · 14 SIFs · 2 C&E matrices · BMS sequence · 8 network zones with firewall rule base |
| Seeded-error detection | **47 / 47** findings found, **0 false positives**; error-free version of the plant: **0 findings** |
| Rules | 44, each linked to a clause of the (fictional) client specification |
| Retrieval accuracy | 64 golden questions: correct clause in top 5 **98 %**, ranked first **84 %** |
| Generated tests | 127 loop check sheets / 1,038 steps · 25 C&E functional tests · 1 BMS sequence test · network tests per firewall |
| Test suite | 41 automated tests, including "error-free plant gives zero findings" |

Along the way the rules also caught **real mistakes in my own baseline design** — e.g. two SIL 3 HIPPS
valves on the same SIS output card. The design was fixed and the specification clause widened, rather
than the rule weakened.

## Sample outputs (fictional data)

- [FAT procedure (PDF, draft)](samples/FAT_Procedure_DRAFT.pdf)
- [Loop check sheets — excerpt (PDF, draft)](samples/Loop_Check_Sheets_excerpt_DRAFT.pdf)
- [Consistency check report](samples/Consistency_Check_Report.md)

## Design for portability

A new project is configuration, not code: tag convention, unit spellings, card catalogue, Excel column
layouts, rule parameters with their spec clauses, and document templates all live in the project folder.
One database and one knowledge index per project keep client information isolated. The next milestone is
loading a second, different fictional plant (a hydrogen plant with IIC areas and another client's
conventions) with **zero code changes**.

## Safety and governance

- No AI signatures; drafts stay visibly *unchecked* until a named engineer approves them.
- Every number that matters comes from deterministic code, never from model text.
- Rule checks run independently of the language model.
- Full generation record on every output; documents are fixed at the source and regenerated.
- Read-only access to engineering data; nothing is ever written to a control system.
- Document text is treated as data, never as instructions.

## Roadmap

- Semantic ranker alongside keyword search; public vendor datasheets in the index
- Answer-level evaluation of Claude on the golden questions
- Portability test on a second fictional plant (HELIOS-5)

---

© 2026 Kidane Berhane. All rights reserved. Shared for portfolio purposes; no licence is granted to reuse
the content. All plant data, specifications and documents are fictional.
