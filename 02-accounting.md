# PRD: Accounting / Akuntansi

## 1. Ringkasan
Aplikasi Akuntansi adalah software pembukuan berbasis double-entry untuk UMKM dan bisnis menengah di Indonesia, mencakup general ledger, hutang/piutang (AP/AR), perpajakan (PPN/PPh), dan pelaporan keuangan. Target penggunanya pemilik bisnis, staf keuangan, dan akuntan/konsultan pajak. Nilai utamanya: pembukuan rapi otomatis, laporan keuangan siap pakai, dan kepatuhan pajak Indonesia tanpa spreadsheet manual.

## 2. Problem Statement
Banyak UMKM masih mencatat keuangan di spreadsheet yang rawan error, tidak menerapkan double-entry, dan sulit menghasilkan laporan keuangan yang benar. Perhitungan pajak (PPN, PPh 21/23) dan rekonsiliasi bank memakan waktu dan sering keliru. Akibatnya keputusan bisnis diambil tanpa data finansial yang akurat dan risiko denda pajak meningkat.

## 3. Goals & Non-Goals
**Goals:**
- Pembukuan double-entry otomatis dari transaksi sehari-hari.
- Modul AP/AR dengan aging & pengingat jatuh tempo.
- Perhitungan & laporan pajak sesuai regulasi Indonesia (PPN, PPh, e-Faktur).
- Laporan keuangan standar (Neraca, Laba Rugi, Arus Kas) real-time.

**Non-Goals:**
- Bukan payroll lengkap (hanya jurnal gaji ringkas pada MVP).
- Bukan sistem inventory/manufaktur mendalam (integrasi saja).
- Tidak menggantikan peran konsultan pajak untuk kasus kompleks.

## 4. Target Users / Personas
- **Pemilik bisnis (Dewi):** butuh gambaran cash flow & laba rugi cepat.
- **Staf keuangan (Tono):** butuh input transaksi, invoice, dan rekonsiliasi harian.
- **Akuntan/konsultan (Ibu Sri):** butuh akses jurnal, penyesuaian, dan ekspor laporan pajak.
- **Auditor (eksternal):** butuh trail audit yang tidak bisa diubah diam-diam.

## 5. Key Features
### MVP
- Chart of Accounts (COA) template Indonesia + jurnal manual/otomatis.
- Invoice penjualan (AR) & tagihan pembelian (AP) dengan status pembayaran.
- Rekonsiliasi bank via impor mutasi (CSV/OFX).
- Laporan Neraca, Laba Rugi, Arus Kas, Buku Besar, Trial Balance.
- Perhitungan PPN keluaran/masukan & ekspor untuk e-Faktur.

### Future / Phase 2+
- Multi-currency & konsolidasi multi-entitas.
- Bank feed otomatis (open banking) & auto-matching berbasis rules.
- Budgeting & forecasting.
- OCR upload struk/faktur menjadi jurnal otomatis.
- Modul PPh 21 & slip gaji.

## 6. User Stories
- Sebagai staf keuangan, saya ingin membuat invoice agar piutang tercatat otomatis di buku besar.
- Sebagai staf keuangan, saya ingin impor mutasi bank agar rekonsiliasi lebih cepat.
- Sebagai pemilik, saya ingin melihat Laba Rugi bulan berjalan agar tahu profitabilitas.
- Sebagai akuntan, saya ingin membuat jurnal penyesuaian agar laporan akhir periode akurat.
- Sebagai staf, saya ingin melihat aging piutang agar bisa menagih yang jatuh tempo.
- Sebagai akuntan, saya ingin ekspor data PPN agar sesuai format e-Faktur.
- Sebagai auditor, saya ingin melihat audit trail agar bisa menelusuri perubahan transaksi.

## 7. Functional Requirements
- FR-1: Sistem harus mendukung double-entry dengan validasi debit = kredit tiap jurnal.
- FR-2: Sistem harus menghasilkan jurnal otomatis dari invoice AR & bill AP.
- FR-3: Sistem harus menghitung PPN keluaran & masukan dan menyediakan rekap per periode.
- FR-4: Sistem harus mengimpor mutasi bank dan menyediakan pencocokan (matching) manual/otomatis.
- FR-5: Sistem harus menghasilkan Neraca, Laba Rugi, dan Arus Kas per periode yang dipilih.
- FR-6: Sistem harus menampilkan aging report untuk AP dan AR.
- FR-7: Sistem harus mencatat audit trail immutable untuk setiap perubahan transaksi.
- FR-8: Sistem harus mendukung penutupan periode (period lock) agar data historis tidak berubah.

## 8. Non-Functional Requirements
- Performance: laporan keuangan dengan 100k+ transaksi tergenerate < 5 detik.
- Security: RBAC, enkripsi data finansial, 2FA untuk akun admin.
- Scalability: mendukung ribuan transaksi/hari per entitas.
- Availability: uptime 99.9% dengan backup harian.
- Compliance: standar akuntansi (SAK EMKM/SAK), regulasi pajak DJP, UU PDP.

## 9. Data Model (High-Level)
- **Account** (id, code, name, type[asset/liability/equity/income/expense], parent_id).
- **JournalEntry** (id, date, description, status, period_id) 1—N **JournalLine** (account_id, debit, credit).
- **Invoice/Bill** (id, party_id, amount, tax_amount, due_date, status).
- **Party** (id, name, npwp, type[customer/vendor]).
- **BankTransaction** (id, date, amount, matched_entry_id). Relasi: JournalEntry 1—N JournalLine—Account; Invoice/Bill menghasilkan JournalEntry.

## 10. Integrations
- e-Faktur / Coretax DJP (ekspor/impor pajak).
- Payment gateway & bank (Midtrans/Xendit, open banking feed).
- E-commerce & POS (Shopee/Tokopedia/kasir) untuk auto-jurnal penjualan.
- Ekspor ke Excel/PDF & API untuk BI eksternal.

## 11. Success Metrics (KPI)
- Waktu tutup buku bulanan turun >= 50%.
- >= 90% transaksi bank ter-rekonsiliasi otomatis.
- Akurasi laporan pajak (error rate) < 1%.
- Adopsi aktif: >= 80% user login mingguan.
- Pengurangan waktu pembuatan laporan keuangan >= 70% vs spreadsheet.

## 12. Risks & Assumptions
- Risiko perubahan regulasi pajak -> perlu modul pajak yang mudah dikonfigurasi.
- Risiko data finansial sensitif -> keamanan & backup jadi prioritas.
- Kompleksitas mapping COA lintas industri.
- Asumsi: user memiliki pemahaman dasar akuntansi atau dibantu template.

## 13. Roadmap / Phases
- **Phase 1 (MVP):** COA + jurnal, AP/AR, rekonsiliasi bank, laporan inti, rekap PPN.
- **Phase 2:** OCR faktur, bank feed otomatis, PPh 21/payroll ringkas, budgeting.
- **Phase 3:** Multi-currency, multi-entity konsolidasi, forecasting, integrasi Coretax penuh.
