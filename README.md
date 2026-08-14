# PRD Collection

Kumpulan **Product Requirements Document (PRD)** untuk 23 jenis sistem/aplikasi enterprise & konsumer. Setiap PRD mengikuti template yang sama sehingga mudah dibandingkan dan dipakai ulang.

## Struktur Repo

```
prd-colletion/
├── README.md                 # file ini
├── ai_agent_system_prompt.md # system prompt agent (referensi)
└── prds/                     # satu file markdown per produk
```

## Daftar PRD

| # | Produk | Domain | File |
|---|--------|--------|------|
| 01 | AI Chatbot | Conversational AI | [prds/01-ai-chatbot.md](prds/01-ai-chatbot.md) |
| 02 | Accounting | Finance | [prds/02-accounting.md](prds/02-accounting.md) |
| 03 | Business Intelligence | Analytics | [prds/03-business-intelligence.md](prds/03-business-intelligence.md) |
| 04 | CV Extractor | HR / AI | [prds/04-cv-extractor.md](prds/04-cv-extractor.md) |
| 05 | CRM | Sales | [prds/05-crm.md](prds/05-crm.md) |
| 06 | Document Management System | Content | [prds/06-document-management-system.md](prds/06-document-management-system.md) |
| 07 | Content Management System | Content | [prds/07-content-management-system.md](prds/07-content-management-system.md) |
| 08 | Customer Support Help Desk | Support | [prds/08-customer-support-helpdesk.md](prds/08-customer-support-helpdesk.md) |
| 09 | E-Commerce | Retail | [prds/09-e-commerce.md](prds/09-e-commerce.md) |
| 10 | ERP | Enterprise | [prds/10-erp.md](prds/10-erp.md) |
| 11 | Enterprise Asset Management | Operations | [prds/11-enterprise-asset-management.md](prds/11-enterprise-asset-management.md) |
| 12 | GRC | Compliance | [prds/12-grc.md](prds/12-grc.md) |
| 13 | HRM | HR | [prds/13-hrm.md](prds/13-hrm.md) |
| 14 | Healthcare EMR | Healthcare | [prds/14-healthcare-emr.md](prds/14-healthcare-emr.md) |
| 15 | Learning Management System | Education | [prds/15-learning-management-system.md](prds/15-learning-management-system.md) |
| 16 | Personal Finance Tracker | Fintech | [prds/16-personal-finance-tracker.md](prds/16-personal-finance-tracker.md) |
| 17 | POS | Retail | [prds/17-pos.md](prds/17-pos.md) |
| 18 | Procurement | Operations | [prds/18-procurement.md](prds/18-procurement.md) |
| 19 | Project Management & Collaboration | Productivity | [prds/19-project-management-collab.md](prds/19-project-management-collab.md) |
| 20 | Real Estate Management | Property | [prds/20-real-estate-management.md](prds/20-real-estate-management.md) |
| 21 | Social Media Analytics | Marketing | [prds/21-social-media-analytics.md](prds/21-social-media-analytics.md) |
| 22 | Supply Chain Management | Logistics | [prds/22-supply-chain-management.md](prds/22-supply-chain-management.md) |
| 23 | Vehicle Fleet Management | Logistics | [prds/23-vehicle-fleet-management.md](prds/23-vehicle-fleet-management.md) |

## Template PRD

Setiap PRD memakai struktur berikut:

1. **Ringkasan** — apa produknya, untuk siapa, nilai utama
2. **Problem Statement** — masalah nyata yang dipecahkan
3. **Goals & Non-Goals** — scope in / out
4. **Target Users / Personas**
5. **Key Features** — MVP + Future
6. **User Stories**
7. **Functional Requirements** (FR-1, FR-2, …)
8. **Non-Functional Requirements** — performance, security, compliance
9. **Data Model (High-Level)**
10. **Integrations**
11. **Success Metrics (KPI)**
12. **Risks & Assumptions**
13. **Roadmap / Phases**

## Cara Pakai

- Ambil satu PRD sebagai titik awal, sesuaikan scope dengan kebutuhan proyek.
- Bagian **MVP** di section 5 & roadmap Phase 1 adalah rekomendasi rilis pertama.
- Section **Functional Requirements** siap dipecah jadi backlog/epic.
