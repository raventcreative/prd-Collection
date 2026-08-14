# PRD: ERP (Enterprise Resource Planning)

## 1. Ringkasan
ERP adalah sistem terintegrasi yang menyatukan modul finance, inventory, HR, dan manufacturing dalam satu platform. Ditujukan untuk perusahaan manufaktur dan distribusi skala menengah yang datanya masih terpisah antar departemen. Nilai utamanya: satu sumber data operasional yang konsisten dan real-time.

## 2. Problem Statement
Departemen menjalankan sistem sendiri-sendiri (akuntansi di satu software, stok di spreadsheet, HR terpisah) sehingga data tidak sinkron, laporan lambat, dan keputusan berbasis angka usang. Rekonsiliasi manual antar modul memakan waktu dan rawan error.

## 3. Goals & Non-Goals
**Goals:**
- Integrasikan finance, inventory, HR, dan manufacturing dalam satu database.
- Sediakan pelaporan real-time lintas modul.
- Otomatiskan alur dari purchase-order hingga jurnal keuangan.
- Kontrol akses per modul dan per role.

**Non-Goals:**
- Menggantikan CRM/e-commerce (integrasi, bukan duplikasi).
- Modul industri sangat spesifik (mis. mining) di MVP.
- BI/analytics canggih (gunakan integrasi eksternal) di MVP.

## 4. Target Users / Personas
- **Finance/Accounting (Wati):** butuh jurnal otomatis dan laporan keuangan akurat.
- **Warehouse Manager (Doni):** butuh stok real-time dan manajemen gudang.
- **HR (Sari):** butuh data karyawan, payroll, dan absensi.
- **Production Planner (Agus):** butuh BOM, work order, dan kapasitas produksi.
- **Direksi (Pak Rudi):** butuh dashboard konsolidasi lintas divisi.

## 5. Key Features
### MVP
- Finance: general ledger, AP/AR, jurnal otomatis.
- Inventory: stok multi-gudang, mutasi, stock opname.
- HR: data karyawan, absensi, payroll dasar.
- Manufacturing: Bill of Materials (BOM) & work order.
- Procurement: purchase order & goods receipt.
- Role-based access & audit trail.

### Future / Phase 2+
- Financial consolidation multi-entity.
- Advanced manufacturing (MRP, capacity planning).
- Fixed asset management.
- Budgeting & forecasting.
- Self-service portal karyawan.
- Analitik & dashboard BI native.

## 6. User Stories
- Sebagai finance, saya ingin jurnal terbentuk otomatis dari transaksi agar tak input ganda.
- Sebagai warehouse manager, saya ingin stok real-time agar tidak salah janji pengiriman.
- Sebagai HR, saya ingin proses payroll dari data absensi agar akurat dan cepat.
- Sebagai production planner, saya ingin work order dari BOM agar kebutuhan material jelas.

## 7. Functional Requirements
- FR-1: Sistem mengelola chart of accounts dan mencatat jurnal double-entry.
- FR-2: Sistem membentuk jurnal otomatis dari transaksi inventory/procurement/payroll.
- FR-3: Sistem melacak stok per gudang dengan mutasi dan valuasi (FIFO/average).
- FR-4: Sistem mengelola master karyawan, absensi, dan perhitungan payroll.
- FR-5: Sistem mendukung BOM, work order, dan konsumsi material produksi.
- FR-6: Sistem mengelola PO, approval, dan goods receipt yang memutakhirkan stok.
- FR-7: Sistem menerapkan RBAC per modul dan mencatat audit trail.
- FR-8: Sistem menghasilkan laporan keuangan (neraca, laba rugi) dan stok.

## 8. Non-Functional Requirements
- Performance: posting transaksi & laporan standar < 3 detik (p95).
- Scalability: multi-entity/multi-cabang, ribuan transaksi harian.
- Security: RBAC granular, enkripsi, audit trail immutable, segregation of duties.
- Availability: SLA 99.9%, backup harian & disaster recovery.
- Compliance: standar akuntansi (PSAK), perpajakan Indonesia (PPN/PPh), UU PDP.

## 9. Data Model (High-Level)
- **Account** (id, code, name, type). Relasi: 1-N ke JournalLine.
- **JournalEntry** (id, date, ref, description). Relasi: 1-N ke JournalLine.
- **Item** (id, sku, name, uom, valuation). Relasi: 1-N ke StockMovement.
- **StockMovement** (id, item_id, warehouse_id, qty, type). Relasi: N-1 ke Item & Warehouse.
- **Employee** (id, nama, position, salary). Relasi: 1-N ke Attendance & Payroll.
- **WorkOrder** (id, product_id, qty, status). Relasi: N-1 ke BOM, 1-N ke MaterialConsumption.
- **PurchaseOrder** (id, vendor_id, status, total). Relasi: 1-N ke POLine, 1-1 ke GoodsReceipt.

## 10. Integrations
- Bank & payment untuk rekonsiliasi kas.
- Sistem perpajakan (e-Faktur/e-Bupot) untuk pelaporan pajak.
- CRM/e-commerce untuk sales order masuk.
- Mesin absensi/biometrik untuk data kehadiran.
- BI tools eksternal via API/data warehouse.

## 11. Success Metrics (KPI)
- Waktu tutup buku bulanan (month-end close) berkurang >= 40%.
- Akurasi stok (selisih stock opname) < 2%.
- Pengurangan entri manual & error rekonsiliasi.
- Laporan lintas modul tersedia real-time.
- Adopsi modul oleh seluruh departemen target.

## 12. Risks & Assumptions
- Risk: implementasi ERP kompleks & lama; mitigasi rollout modular bertahap.
- Risk: resistensi perubahan proses; butuh change management & training.
- Risk: migrasi data legacy tidak bersih; validasi & reconciliation.
- Asumsi: proses bisnis dapat distandarkan di seluruh divisi.
- Asumsi: manajemen berkomitmen pada adopsi lintas departemen.

## 13. Roadmap / Phases
- **Phase 1 (MVP):** finance (GL/AP/AR), inventory, HR dasar, BOM/work order, procurement.
- **Phase 2:** payroll lengkap, fixed asset, MRP, budgeting, self-service portal.
- **Phase 3:** multi-entity consolidation, analitik/BI native, capacity planning lanjutan.
