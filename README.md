# SCM Assistant — BQBYTE Supply Chain RAG Chatbot

> **Trinamix Inc · Junior AI Engineer Hiring Task · Ref: TX-JrAI-003**

## 🔗 Public Chatbot URL

```
https://cloud.flowiseai.com/chatbot/04022833-6089-4a48-be71-86f28919619c
```
> Replace after publishing via Share → Make Public.

---

## 🛠️ LLM and Embeddings Used

| Component | Model | Provider |
|---|---|---|
| **LLM** | `open-mistral-7b` | Mistral AI |
| **Embeddings** | `mistral-embed` | MistralAI Embeddings |
| **Vector Store** | In-Memory Vector Store | Flowise built-in |
| **Chain** | Conversational Retrieval QA Chain | Flowise built-in |
| **Memory** | Buffer Memory | Flowise built-in |

---

## ⚙️ Two Chunk Configurations Tested

### Config A — 800 / 150 (Text Splitter 1, rag_txt file)

```
Chunk Size:    800 characters
Chunk Overlap: 150 characters
Top K:         20
```
**Observations:** Using the RAG .txt file is better than the raw CSV for 3 reasons:

Raw CSV has 2000 rows — the LLM gets 20 random chunks and misses 90% of suppliers
The .txt has pre-computed answers (regional totals, SWL list, rebate list) baked in directly
The LLM just reads the answer instead of trying to count/aggregate from scattered rows

### Config B — 1200 / 200 (Text Splitter 2, PDF policy)

```
Chunk Size:    1200 characters
Chunk Overlap: 200 characters
Top K:         20
```
**Observations:** Better for the policy PDF — larger chunks keep full policy sections (penalty tables, SLA definitions) together in one chunk rather than splitting mid-table. Overlap of 200 ensures threshold values are never cut off at chunk boundaries.

**Why two splitters:** The rag_txt data benefits from smaller chunks (one supplier profile per chunk), while the PDF policy benefits from larger chunks (full sections with all thresholds intact).

---

# ✅ Sample Questions Testing

---

## Q1

### Question

```txt
Which Tier-3 suppliers have an active disruption flag, and what response level applies per policy?
```

### Answer

```txt
Here are all Tier-3 suppliers with active disruption flags and their assigned response levels per Policy §9 and §2:

    SUP-011 – Dravex Components India
        Region: APAC
        Disruption: Typhoon season disruption
        Risk: High
        Response: Level 3 Activate (CPO escalation, alternate supplier at 40% volume within 10 days, +50% safety stock, RCA within 15 days)

    SUP-041 – Archipelago PCB Corp
        Region: APAC
        Disruption: Labour strike risk
        Risk: High
        Response: Level 3 Activate (mandatory per Policy §9 for labour strikes)

    SUP-045 – Sibertek Molding
        Region: APAC
        Disruption: Port closure risk
        Risk: High
        Response: Level 3 Activate

    SUP-078 – Varna Electronics EAD
        Region: EMEA
        Disruption: Port congestion risk
        Risk: Medium
        Response: Level 3 Activate (per §9, port congestion exceeding 72 hours triggers Level 3)

    SUP-080 – Maghreb Castworks
        Region: EMEA
        Disruption: Tariff adjustment pending
        Risk: Medium
        Response: Level 3 Activate (per §9, pending regulatory audit/tariff adjustment triggers Level 3)

    SUP-090 – Plataforma Metales SA
        Region: LATAM
        Disruption: Export license risk
        Risk: High
        Response: Level 3 Activate (mandatory per Policy §9 for export control restrictions)

    SUP-095 – Cerromax Mineria
        Region: LATAM
        Disruption: Audit overdue; compliance review
        Risk: High
        Response: Level 3 Activate (overdue audit triggers Level 3)

    SUP-100 – Orinoco Pack SAPI
        Region: LATAM
        Disruption: Audit overdue; compliance review
        Risk: High
        Response: Level 3 Activate

    SUP-101 – Quetzal Textiles
        Region: LATAM
        Disruption: Seasonal flood risk
        Risk: Medium
        Response: Level 3 Activate (seasonal disruption triggers Level 3)

    SUP-079 – Helios Pack Greece
        Region: EMEA
        Disruption: Labour strike risk
        Risk: High
        Response: Level 3 Activate (mandatory per Policy §9 for labour strikes)

    SUP-018 – Deltaforge Vietnam
        Region: APAC
        Disruption: Port congestion risk
        Risk: Medium
        Response: Level 3 Activate (port congestion exceeding 72 hours triggers Level 3)

Total Tier-3 suppliers with active disruption flags: 11 (all assigned Level 3 Activate).
Note: SUP-079 and SUP-041 (labour strikes) and SUP-090 (export control) are mandatory Level 3 regardless of risk level.
```

---

## Q2

### Question

```txt
Which suppliers qualify for the annual Volume Rebate Program and how many are there?
```

### Answer

```txt
The following 19 Tier-1 suppliers qualify for the annual Volume Rebate Program (Policy §4.2):

    SUP-062 – Borealis Composites (EMEA, Denmark, RM)
    SUP-102 – Crestline Chemical Supply (NA, USA, RM)
    SUP-103 – Fenwick Alloy Solutions (NA, USA, SA)
    SUP-023 – Hanguk Circuit Works (APAC, South Korea, EC)
    SUP-028 – Hokkaido Alloy Tech (APAC, Japan, SA)
    SUP-043 – Tasman Circuit Boards (APAC, Australia, EC)
    SUP-051 – Toreval Electronics (EMEA, France, EC)
    SUP-060 – Nordloom Finland Oy (EMEA, Finland, IT)
    SUP-001 – Orrentek Precision Mfg (APAC, China, EC)
    SUP-064 – Ostwind Composites AG (EMEA, Austria, RM)
    SUP-033 – PrecisionForge Taiyuan (APAC, Taiwan, MC)
    SUP-056 – Solveig Eco Packaging (EMEA, Sweden, PM)
    SUP-037 – Straits Packaging Hub (APAC, Singapore, PM)
    SUP-066 – Valdoro Special Alloys (EMEA, Italy, SA)
    SUP-046 – Krauss-Polymex GmbH (EMEA, Germany, RM)
    SUP-105 – Lakeshore Components (NA, USA, MC)
    SUP-054 – Lumivex Semiconductor NL (EMEA, Netherlands, EC)
    SUP-112 – Maplewood Polymer Corp (NA, Canada, RM)
    SUP-058 – Norbec Alloy Works (EMEA, Norway, SA)

Total qualifying suppliers: 19 (all meet Tier-1, OTD ≥ 93%, Defect Rate < 0.5%, Sustainability Score ≥ 85).
```

---

## Q3

### Question

```txt
Which region has the highest total PO value, and does it breach the concentration limit?
```

### Answer

```txt
The EMEA region has the highest total PO value at $193,987,179.91, which breaches the 45% concentration cap (Policy §5.3).

    EMEA spend: 48.5% of total ($399,563,494.10)
    Breach: Yes (requires a Diversification Plan within 60 days per Policy §5.3).
```

---

## Q4

### Question

```txt
Which suppliers are on Supplier Watch List (SWL) status and what does it restrict?
```

### Answer

```txt
The following 11 suppliers are on Supplier Watch List (SWL) status (Policy §3.4):

    SUP-018 – Deltaforge Vietnam (APAC, Vietnam)
        Compliance Score: 62

    SUP-080 – Maghreb Castworks (EMEA, Morocco)
        Compliance Score: 62

    SUP-079 – Helios Pack Greece (EMEA, Greece)
        Compliance Score: 60

    SUP-095 – Cerromax Mineria (LATAM, Peru)
        Compliance Score: 53

    SUP-100 – Orinoco Pack SAPI (LATAM, Venezuela)
        Compliance Score: 61

    SUP-078 – Varna Electronics EAD (EMEA, Bulgaria)
        Compliance Score: 58

    SUP-041 – Archipelago PCB Corp (APAC, Philippines)
        Compliance Score: 67 (Note: Listed in SWL due to active disruption)

    SUP-011 – Dravex Components India (APAC, India)
        Compliance Score: 66 (Note: Listed in SWL due to active disruption)

    SUP-045 – Sibertek Molding (APAC, Russia)
        Compliance Score: 64 (Note: Listed in SWL due to active disruption)

    SUP-101 – Quetzal Textiles (LATAM, Guatemala)
        Compliance Score: 62

Restrictions (Policy §3.4):

    New PO issuance limited to 20% of prior quarter volume for each SWL supplier.
    No new contracts may be awarded to SWL suppliers unless corrective action raises compliance score ≥ 60.
```

---

## Q5

### Question

```txt
Which product category has the highest average defect rate and does it exceed the Tier-2 limit?
```

### Answer

```txt
The Mechanical Components (MC) category has the highest average defect rate at 2.12% across 360 POs.

    Tier-2 maximum permissible defect rate: 2.50% (Policy §3.2).
    Result: Does not exceed the Tier-2 limit (2.12% < 2.50%).
```

### Policy Threshold

Per Policy §3.2:

```txt
Tier-2 maximum permissible defect rate = 2.50%
```

### Result

- Mechanical Components does NOT exceed the Tier-2 limit
- Compliance status: PASS

```txt
2.12% < 2.50%
```

### Result

Packaging Materials does **NOT** exceed the Tier-2 threshold.

However, several individual suppliers exceed their own limits:

- SUP-079 — Helios Pack Greece → 4.31%
- SUP-027 — Busan Pack Corp → 3.13%

These suppliers should be flagged for quality review.
---

## 📂 Repository Structure

```
scm-assistant-bot/
├── scm_assistant.json
├── supplier_data_for_rag.txt    ← Upload this into the CSV/Text File node
├── README.md
├── .gitignore
└── screenshots/
    ├── 01_chatflow_canvas.png
    ├── 02_files_uploaded.png
    ├── 03_chat_q1.png
    ├── 04_chat_q2.png
    ├── 05_chat_q3.png
    ├── 06_chat_q4.png
    ├── 07_chat_q5.png
    └── 08_share_public_url.png
```

---

## 🔮 What I'd Improve

1. **Pre-aggregated summaries** — The RAG file already pre-computes regional totals, category averages, and SWL lists so the LLM doesn't need to count/aggregate from raw rows
2. **Persistent vector store** (Pinecone/Chroma) — In-Memory resets on restart
3. **Agent with code tool** — For exact numerical queries, a Python-enabled agent would be more reliable than retrieval
4. **Metadata filtering** — Tag chunks as `source:policy` or `source:data` for targeted retrieval
5. **RAGAS evaluation** — Automated scoring against ground-truth Q&A pairs

---

*BQBYTE Technologies — Global Procurement Division | Policy ID: BQB-SCG-032*
