# AEP Big Data Architecture and Governance
---

## What this project is

This is a semester-long case study built around American Electric Power, one of the largest electric utilities in the United States, serving 5.6 million customers across 11 states through six independently managed operating companies.

The project walks through the full lifecycle of a real data architecture and governance engagement: understanding the company, identifying where data creates business problems, scoping projects that deliver measurable value, justifying every decision with evidence, and building a governance framework that holds it all together.

Every step in this repository is documented not just as a deliverable but as a decision. What was chosen, what was rejected, and why. That distinction matters because domain understanding drove everything. Without understanding how AEP's grid works, how its operating companies are structured, and what regulatory obligations it operates under, none of the project scoping, budget justification, tool selection, or governance principles would hold up under scrutiny.

---

## The company

**American Electric Power (AEP)** is one of the largest electric utilities in the United States.

| Attribute | Detail |
|---|---|
| Customers served | 5.6 million across 11 states |
| Transmission network | Nation's largest, 40,000+ miles of high-voltage lines |
| Business segments | 4: Vertically Integrated Utilities (VIU), Transmission and Distribution (T&D), Generation and Marketing (G&M), AEPTHCo |
| VIU operating companies | 6: AEP Ohio, AEP Texas, Appalachian Power, Indiana Michigan Power, SWEPCO, PSO |
| Capital investment plan | $54 to $72 billion through 2030 |
| Data center load committed | 28 GW through 2030 |
| Peak demand growth projected | 75% by 2030 |
| Regulatory environment | NERC CIP compliance for critical grid infrastructure, state utility commission oversight across 11 jurisdictions |

The starting point for this project was a company-wide SWOT analysis grounded in AEP's actual filings, investor relations documents, and operational data, not generic assumptions. Every project decision that follows traces back to something real about AEP's business.

---

## What is in this repository

```
├── presentation/          AEP Big Data Architecture and Governance, full slide deck
├── governance/            Data Governance Guiding Principles (DGGP), the enterprise framework
└── README.md              This file
```

The presentation is the primary artifact. It covers the full project lifecycle from company research through final architecture. The governance document is a standalone deliverable: AEP's enterprise data constitution, covering all ten guiding principles with diagrams, a four-tier data classification system, and quantitative KPIs for every principle.

---

## How the project was structured

### 1. Company research and SWOT

The project began with deep research into AEP's business, not just surface-level facts but the specific challenges that make data governance urgent for a company of this kind. The most important finding: AEP's six operating companies had historically managed data independently, creating fragmented definitions, duplicate records, and manual reconciliation workflows that undermined enterprise-wide decision-making.

The SWOT was built at the enterprise level and became the anchor for everything that followed. Every project-level decision required tracing back to a company-level strength, weakness, opportunity, or threat.

### 2. Business segment analysis

Understanding AEP's four segments (VIU, T&D, G&M, and AEPTHCo) was the precondition for scoping any project. Each segment has different data characteristics, different regulatory obligations, and different operational priorities. A billing system for VIU's residential customers and a SCADA monitoring platform for T&D's 40,000-mile transmission network are not the same type of project and cannot be treated as such.

### 3. Project definition and selection

Two projects were defined and evaluated:

**Project 1: Enterprise Data Architecture and Governance Foundation**
Scope: VIU segment, six operating companies, 3.76 million customers. Core problem: no unified customer view, no enterprise data quality standard, no consolidated governance. The foundational project AEP needs before advanced analytics is possible.

**Project 2: T&D Grid Optimization and Predictive Operations Analytics**
Scope: AEP's full 40,000-mile transmission network. Core problem: reactive operations at a moment when 28 GW of data center customers, who sell uptime to hyperscalers, financial institutions, and healthcare systems, cannot tolerate unplanned downtime. This project converts AEP from reactive to predictive.

Project 2 was selected based on a seven-criteria comparison covering budget, resources, timeframe, risk, strategic alignment, ROI, and customer impact. The decisive factors were ROI (430% vs 232% over three years) and strategic urgency: grid reliability is an operational safety and revenue protection problem, not a management efficiency problem.

### 4. Functional and non-functional requirements

A complete requirements document was produced for the selected project. Requirements were structured by system behavior: data ingestion, processing, alerting, reporting, access management, and external interfaces, rather than by dashboard or output. This distinction matters because dashboards are outputs of a system, not categories of requirements.

A dedicated Data Governance requirement category was added to connect the requirements document to the DGGP, showing coherence across the full project portfolio.

### 5. Architecture tool selection

Twelve tools were selected across six layers: data collection, storage, framework, BI, and management. Each was justified by a specific functional or non-functional requirement. Every excluded tool is documented with a rationale.

| Layer | Tool | Role |
|---|---|---|
| Data collection | Apache Kafka | Real-time SCADA streaming, 50,000+ sensor events per second |
| Data collection | Apache NiFi | Batch ingestion, weather API feeds, satellite data |
| Storage | Apache Kudu | Active sensor data, equipment health records, outage events |
| Storage | HDFS | Cold sensor archives, ML training datasets, audit logs |
| Framework | Apache Spark | Stream processing, batch ETL, MLlib failure prediction |
| Framework | Apache Oozie | Workflow orchestration and pipeline sequencing |
| Framework | TensorFlow | Deep learning on satellite imagery for vegetation threat scoring |
| BI | Power BI | Eight operational dashboards for grid operators and maintenance teams |
| BI | IBM Cognos | Regulatory reports across 11 states, NERC CIP audit and SAIDI/SAIFI reporting |
| Management | ZooKeeper | Cluster coordination and HA failover for Kafka, HDFS, and Kudu |
| Management | YARN | Resource allocation across multi-tenant workloads |
| Management | Chukwa | Infrastructure log collection and health monitoring |

The architecture follows a hybrid Lambda-Kappa pattern: the conceptual separation of batch and speed processing paths exists, but both are executed by the same unified Spark engine rather than separate systems.

### 6. Budget and ROI

All cost figures were built bottom-up, sourcing real market salary data for each specialist role, pricing hardware at actual market rates, and calculating software license costs per seat.

**Project 2 budget: $3,800,000**

| Category | Amount |
|---|---|
| Hardware | $750,000 |
| Software | $310,000 |
| Consulting | $1,146,600 |
| Operations (10% contingency) | $345,648 |
| People | $1,247,752 |

**Project 2 ROI over three years: 430%**

Benefits are calibrated to the consequence of failure, not to optimistic assumptions. The $800,000 annual outage prevention benefit is not just cost savings. It is the difference between retaining and losing data center contracts worth hundreds of millions annually. The $2.5 million maintenance savings frees capital for the $54 billion infrastructure plan.

### 7. Data Governance Guiding Principles

The DGGP is AEP's enterprise data constitution: the permanent framework within which all data decisions operate, regardless of which project is active. It covers ten principles organized across four governance layers: structure and people, process and quality, access and security, and education.

Key elements:

**Federated governance model:** an enterprise layer sets standards and accountability, while each of the six operating companies retains stewardship of its regional data assets. Full centralization fails because of state-level regulatory differences across 11 jurisdictions. Full decentralization recreates the fragmentation problem the framework is designed to solve.

**Four-tier data classification:** every data asset is classified by the consequence of unauthorized exposure, not by operational convenience.
- Tier 1 (Public): no controls required
- Tier 2 (Internal): RBAC, team-level approval, full access logging
- Tier 3 (Confidential): need-to-know authorization, multi-factor authentication, individual-level access control
- Tier 4 (Restricted): director-level approval, named individuals only, end-to-end encryption, active intrusion detection, NERC CIP mandatory controls. Unauthorized access to SCADA configurations is not a financial risk. It is a national infrastructure risk.

**Quantitative KPIs for every principle:** a governance document without measurable targets leaves room for different interpretations of what good looks like. In an organization operating across 11 states and six companies, that ambiguity compounds into real operational and regulatory exposure.

Selected targets:

| Principle | KPI | Target |
|---|---|---|
| Data quality: billing | Accuracy | 99% |
| Data quality: SCADA | Accuracy | 99.99% |
| Access revocation | On departure | Within 24 hours |
| Security patches | Application window | Within 30 days |
| Annual training | Completion | 100% of all employees |
| Onboarding training | Completion | Within 30 days of start date |

---

## Key decisions and the reasoning behind them

**Why Project 2 over Project 1:** Project 2 costs $240,000 more (a 6% premium) but delivers $10.3 million more in net value over three years. More importantly, it addresses AEP's most urgent operational problem: 28 GW of data center customers who cannot tolerate outages, served by a grid that was operating reactively.

**Why federated governance:** Full centralization fails because AEP's operating companies have state-level regulatory obligations that require local authority. Full decentralization was already the status quo, and the fragmentation it produced was the problem the entire project was designed to solve.

**Why four tiers instead of three:** The original three-tier system had no meaningful distinction between data that is sensitive because of privacy obligations and data that is sensitive because unauthorized access could enable a grid attack affecting millions of people. Those are not the same category of risk and cannot be governed the same way.

**Why TensorFlow alongside Spark MLlib:** Vegetation management requires processing raw satellite imagery to detect encroachment on transmission lines. This is an image recognition problem using Convolutional Neural Networks on multispectral imagery. Spark MLlib is a machine learning library for tabular data. It cannot process raw images. TensorFlow was included for a specific, distinct workload, not as a general ML tool.

**Why 99.99% for SCADA data:** A billing error is caught in the next cycle. A SCADA error at the wrong moment can cascade into a grid failure affecting millions of people. The consequence is immediate and physical, not financial. The target reflects that distinction.

---

## Tools and technologies

**Architecture:** Apache Kafka, Apache NiFi, Apache Kudu, HDFS, Apache Spark, Apache Oozie, TensorFlow, ZooKeeper, YARN, Apache Chukwa

**Business intelligence:** Power BI, IBM Cognos

**Governance and compliance:** NERC CIP (CIP-005, CIP-007, CIP-010, CIP-011, CIP-013), RBAC, federated governance model

**Project management:** Velero ETP (budget, ROI, resources, risks, SWOT, impact)

---

## About this project

Built as the capstone case study for DAMG 6330: Big Data Architecture and Governance at Northeastern University, Toronto Campus, Spring 2026. The course required selecting a real company and maintaining it as a persistent case study across all assignments: company research, project scoping, requirements, architecture, budgeting, and governance.

The goal was never to produce polished slides. It was to make defensible decisions about real problems at a real company, and to be able to explain the reasoning behind every one of them.
