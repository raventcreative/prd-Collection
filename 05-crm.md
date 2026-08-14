# PRD: CRM (Customer Relationship Management)

## 1. Ringkasan
CRM ini adalah sistem untuk mengelola leads, kontak, deals, dan sales pipeline dalam satu tempat. Ditujukan untuk tim sales dan marketing UMKM hingga mid-market yang ingin berhenti bergantung pada spreadsheet. Nilai utamanya: visibilitas pipeline dan follow-up yang tidak terlewat.

## 2. Problem Statement
Tim sales sering kehilangan jejak leads karena data tersebar di spreadsheet, chat, dan email. Tidak ada visibilitas terhadap status deal, siapa follow-up, dan kapan. Akibatnya konversi rendah, forecasting tidak akurat, dan handover antar staf berantakan.

## 3. Goals & Non-Goals
**Goals:**
- Pusatkan data kontak, leads, dan deals dalam satu sistem.
- Kelola sales pipeline dengan stage yang bisa dikustomisasi.
- Otomatiskan reminder follow-up dan aktivitas.
- Sediakan laporan pipeline dan forecasting sederhana.

**Non-Goals:**
- Full marketing automation (email campaign kompleks) di MVP.
- Modul akuntansi/invoicing penuh.
- Customer support ticketing (helpdesk terpisah).

## 4. Target Users / Personas
- **Sales Rep (Andi):** butuh kelola leads dan catat aktivitas cepat dari mobile.
- **Sales Manager (Maya):** butuh visibilitas pipeline dan forecast tim.
- **Marketing (Rina):** butuh serah-terima leads berkualitas ke sales.
- **Owner UMKM (Pak Joko):** butuh gambaran omzet dan performa sales ringkas.

## 5. Key Features
### MVP
- Manajemen kontak & company.
- Lead capture dan kualifikasi.
- Pipeline deals dengan drag-and-drop stage.
- Activity log (call, email, meeting, note) dan reminder.
- Assignment leads ke sales rep.
- Dashboard pipeline & laporan dasar.

### Future / Phase 2+
- Email integration & tracking (open/click).
- Lead scoring otomatis.
- Workflow automation (trigger-action).
- Quotation/invoice generation.
- Mobile app native & offline mode.
- Integrasi WhatsApp Business.

## 6. User Stories
- Sebagai sales rep, saya ingin menambah lead dari form web agar tidak input manual.
- Sebagai sales rep, saya ingin reminder follow-up agar tidak ada deal yang terlupakan.
- Sebagai sales manager, saya ingin lihat pipeline per stage agar bisa forecast omzet.
- Sebagai marketing, saya ingin assign lead ke rep tertentu agar distribusi adil.

## 7. Functional Requirements
- FR-1: Sistem menyimpan kontak dengan field kustom dan tag.
- FR-2: Sistem menangkap lead dari form web/API dan sumber manual.
- FR-3: Sistem mengelola deals dengan stage yang dapat dikonfigurasi.
- FR-4: Sistem mencatat aktivitas dan mengirim reminder terjadwal.
- FR-5: Sistem mendukung assignment dan reassignment leads/deals.
- FR-6: Sistem menghitung nilai pipeline per stage dan expected revenue.
- FR-7: Sistem mengontrol akses berbasis role (rep vs manager).
- FR-8: Sistem menyediakan filter, search, dan ekspor data ke CSV.

## 8. Non-Functional Requirements
- Performance: load list 10.000 kontak < 2 detik dengan pagination.
- Scalability: multi-tenant, mendukung ratusan organisasi.
- Security: RBAC, enkripsi data, audit log perubahan record.
- Availability: SLA 99.9%, backup harian.
- Compliance: patuh UU PDP, consent tracking, opsi hapus data (right to be forgotten).

## 9. Data Model (High-Level)
- **Contact** (id, nama, email, phone, company_id, tags). Relasi: N-1 ke Company, 1-N ke Activity.
- **Company** (id, nama, industri, size). Relasi: 1-N ke Contact & Deal.
- **Lead** (id, source, status, owner_id, score). Relasi: dapat konversi ke Contact/Deal.
- **Deal** (id, title, value, stage, close_date, owner_id). Relasi: N-1 ke Company, 1-N ke Activity.
- **Activity** (id, type, subject, due_date, done). Relasi: N-1 ke Contact/Deal.
- **User** (id, nama, role, team). Relasi: 1-N ke Lead/Deal (owner).

## 10. Integrations
- Email provider (Gmail/Outlook) untuk sinkron dan tracking.
- Web form & landing page untuk lead capture.
- WhatsApp Business API untuk komunikasi.
- Kalender (Google Calendar) untuk meeting.
- Zapier/webhook untuk integrasi tools lain.

## 11. Success Metrics (KPI)
- Peningkatan konversi lead-to-deal >= 20%.
- Rasio follow-up tepat waktu >= 90%.
- Akurasi forecast (deviasi actual vs forecast) < 15%.
- Adopsi harian sales rep (DAU) >= 80% tim.
- Waktu handover leads berkurang signifikan.

## 12. Risks & Assumptions
- Risk: adopsi rendah jika input data terasa merepotkan; mitigasi UX cepat & mobile.
- Risk: data kotor/duplikat menurunkan kualitas; sediakan dedupe.
- Risk: integrasi email/WhatsApp berubah API.
- Asumsi: tim sales bersedia pindah dari spreadsheet.
- Asumsi: proses sales sudah punya stage yang jelas.

## 13. Roadmap / Phases
- **Phase 1 (MVP):** kontak, leads, pipeline deals, aktivitas & reminder, dashboard dasar.
- **Phase 2:** email integration, lead scoring, workflow automation, quotation.
- **Phase 3:** mobile native, WhatsApp integration, analitik lanjutan & forecasting AI.
