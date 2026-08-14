# PRD: Governance, Risk & Compliance (GRC)

## 1. Ringkasan
GRC adalah platform terpusat untuk mengelola risiko organisasi, kontrol internal, audit, dan kebijakan dalam satu sistem. Ditujukan untuk tim risk, compliance, internal audit, dan CISO di perusahaan yang teregulasi (keuangan, kesehatan, teknologi). Nilai utamanya: satu sumber kebenaran untuk risiko dan kepatuhan, menggantikan spreadsheet dan email yang terfragmentasi.

## 2. Problem Statement
Risiko, kontrol, temuan audit, dan kebijakan dikelola terpisah di spreadsheet, dokumen, dan email sehingga sulit dilacak dan rawan usang. Saat audit atau regulator datang, tim menghabiskan berminggu-minggu mengumpulkan bukti kepatuhan secara manual. Tidak ada visibilitas real-time atas postur risiko organisasi.

## 3. Goals & Non-Goals
**Goals:**
- Membangun risk register terpusat dengan skoring dan owner yang jelas.
- Menghubungkan kontrol ke risiko dan ke requirement regulasi/framework.
- Mengelola siklus audit: perencanaan, temuan, remediasi, follow-up.
- Mengelola siklus hidup kebijakan: draft, approval, publish, attestasi.

**Non-Goals:**
- Bukan SIEM/tooling keamanan teknis (hanya konsumsi sinyalnya).
- Tidak menggantikan legal contract management.
- Bukan platform ERM finansial/aktuaria mendalam.

## 4. Target Users / Personas
- **Risk Manager (Dewi):** butuh risk register, heatmap, dan tren risiko.
- **Compliance Officer (Fauzi):** butuh mapping kontrol ke framework (ISO 27001, SOC 2, POJK).
- **Internal Auditor (Nina):** butuh mengelola audit plan, temuan, dan remediasi.
- **Control Owner (Rizal):** butuh tugas review kontrol dan upload bukti.
- **Executive/Board (CISO):** butuh dashboard postur risiko & kepatuhan.

## 5. Key Features
### MVP
- Risk register dengan skoring (likelihood x impact) dan heatmap.
- Control library terhubung ke risiko.
- Audit management: temuan (findings) dan action plan remediasi.
- Policy management: authoring, approval workflow, versioning.
- Evidence repository dengan tautan ke kontrol/audit.

### Future / Phase 2+
- Compliance framework mapping otomatis & crosswalk multi-standar.
- Control testing otomatis + continuous monitoring.
- Third-party/vendor risk management.
- Attestation campaign & policy acknowledgement massal.
- AI-assisted risk assessment dan gap analysis.

## 6. User Stories
- Sebagai risk manager, saya ingin menambah risiko baru dengan skor agar muncul di heatmap organisasi.
- Sebagai compliance officer, saya ingin memetakan kontrol ke ISO 27001 agar tahu gap kepatuhan.
- Sebagai auditor, saya ingin mencatat temuan dan menugaskan remediasi agar bisa dilacak sampai selesai.
- Sebagai control owner, saya ingin diminta review kontrol berkala dan upload bukti agar audit lebih cepat.

## 7. Functional Requirements
- FR-1: Sistem dapat membuat risiko dengan atribut kategori, owner, likelihood, impact, dan skor residual.
- FR-2: Sistem menampilkan heatmap dan register risiko yang dapat difilter.
- FR-3: Sistem menautkan satu kontrol ke banyak risiko dan banyak requirement framework.
- FR-4: Sistem mengelola audit engagement beserta temuan dan status remediasi.
- FR-5: Sistem mendukung workflow approval kebijakan berjenjang dan versioning.
- FR-6: Sistem menyimpan bukti (file/link) yang tertaut ke kontrol atau temuan.
- FR-7: Sistem mencatat audit trail immutable untuk semua perubahan.

## 8. Non-Functional Requirements
- Performance: Dashboard risiko termuat < 3 detik untuk 5k+ risiko.
- Scalability: Mendukung 50k kontrol dan 1000 pengguna.
- Security: RBAC granular, enkripsi at-rest & in-transit, SSO, MFA.
- Availability: Uptime 99.9% (sistem kritikal audit).
- Compliance: Audit trail immutable, retensi bukti sesuai regulasi, data residency configurable.

## 9. Data Model (High-Level)
- **Risk** (id, title, category, likelihood, impact, residual_score, owner_id, status). Relasi: dimitigasi banyak Control.
- **Control** (id, name, type, frequency, owner_id, status). Relasi: menutup banyak Risk & memenuhi banyak Requirement.
- **Requirement** (id, framework, clause, description). Relasi: dipenuhi banyak Control.
- **Audit** (id, scope, period, status). Relasi: menghasilkan banyak Finding.
- **Finding** (id, audit_id, severity, remediation_plan, due_date, status). Relasi: milik satu Audit.
- **Policy** (id, title, version, status, approver_id). Relasi: punya banyak Attestation.

## 10. Integrations
- Identity provider (SSO/SAML/OIDC) untuk auth dan user directory.
- HRIS untuk data owner/department.
- Ticketing (Jira/ServiceNow) untuk task remediasi.
- Security tools (vuln scanner, SIEM) untuk sinyal kontrol otomatis (Phase 2).
- Document storage (SharePoint/Google Drive) untuk bukti.

## 11. Success Metrics (KPI)
- Waktu persiapan audit turun 40%.
- 100% risiko kritikal punya owner dan kontrol tertaut.
- Remediasi temuan overdue < 10%.
- Policy attestation rate > 95% per kampanye.

## 12. Risks & Assumptions
- Risiko: resistensi adopsi karena tim terbiasa spreadsheet.
- Risiko: mapping framework kompleks dan bisa salah interpretasi.
- Asumsi: organisasi punya framework kepatuhan yang jelas untuk dipetakan.
- Asumsi: ada sponsor eksekutif (CISO/CRO) untuk mendorong penggunaan.

## 13. Roadmap / Phases
- **Phase 1 (MVP):** Risk register, control library, audit/findings, policy management.
- **Phase 2:** Framework mapping, control testing, vendor risk, attestation campaign.
- **Phase 3:** Continuous monitoring, AI risk assessment, integrasi security tooling, board reporting lanjutan.
