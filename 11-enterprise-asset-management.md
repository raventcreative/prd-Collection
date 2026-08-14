# PRD: Enterprise Asset Management (EAM)

## 1. Ringkasan
EAM adalah platform untuk mengelola siklus hidup aset fisik (mesin, kendaraan, fasilitas) dari pengadaan hingga disposal. Ditujukan untuk tim maintenance, operations, dan finance di perusahaan manufaktur, utilitas, dan fasilitas. Nilai utamanya: menurunkan downtime lewat preventive maintenance terjadwal dan visibilitas biaya per aset.

## 2. Problem Statement
Aset kritis sering rusak tanpa peringatan karena maintenance masih reaktif dan dicatat manual di spreadsheet. Riwayat perbaikan, jadwal servis, dan biaya per aset terpencar, sehingga sulit menghitung total cost of ownership dan merencanakan penggantian. Downtime tak terencana langsung memukul produksi.

## 3. Goals & Non-Goals
**Goals:**
- Mendata seluruh aset beserta lokasi, status, dan hierarki (parent-child).
- Menjalankan preventive maintenance (PM) berbasis kalender dan meter usage.
- Mengelola work order end-to-end: request, assign, eksekusi, close.
- Melacak biaya, downtime, dan riwayat per aset untuk analisis TCO.

**Non-Goals:**
- Bukan sistem akuntansi/GL penuh (hanya integrasi biaya).
- Tidak mencakup manajemen SDM/payroll teknisi.
- Bukan platform IoT/SCADA real-time (hanya konsumsi data meter).

## 4. Target Users / Personas
- **Maintenance Manager (Budi):** butuh dashboard downtime, backlog work order, dan kepatuhan jadwal PM.
- **Teknisi Lapangan (Andi):** butuh akses mobile ke work order, checklist, dan input parts terpakai.
- **Asset/Reliability Engineer (Sari):** butuh analisis MTBF/MTTR dan riwayat kegagalan.
- **Procurement/Finance (Rina):** butuh data biaya aset dan spare part untuk budgeting.

## 5. Key Features
### MVP
- Asset registry dengan hierarki lokasi & kategori.
- Preventive maintenance scheduler (calendar & meter-based).
- Work order management (create, assign, status, close).
- Spare parts / inventory dasar terkait work order.
- Mobile-friendly work order execution + checklist.

### Future / Phase 2+
- Predictive maintenance dari data sensor/IoT.
- Barcode/QR & RFID scanning untuk aset dan parts.
- Warranty & contract management vendor.
- Failure mode analysis (FMEA) dan reliability analytics.

## 6. User Stories
- Sebagai maintenance manager, saya ingin melihat semua PM yang jatuh tempo minggu ini agar bisa menjadwalkan teknisi.
- Sebagai teknisi, saya ingin menerima work order di HP dan mengisi checklist agar tidak perlu kertas.
- Sebagai reliability engineer, saya ingin melihat MTBF per aset agar bisa memprioritaskan perbaikan.
- Sebagai finance, saya ingin melihat total biaya maintenance per aset agar bisa memutuskan replace vs repair.

## 7. Functional Requirements
- FR-1: Sistem dapat membuat, mengedit, dan menonaktifkan record aset dengan atribut lokasi, kategori, dan status.
- FR-2: Sistem mendukung hierarki aset (parent-child) dan lokasi bertingkat.
- FR-3: Sistem membuat work order otomatis saat PM jatuh tempo (tanggal atau meter reading).
- FR-4: Sistem dapat meng-assign work order ke teknisi dan melacak status (open, in-progress, on-hold, closed).
- FR-5: Sistem mencatat parts, labor hours, dan biaya per work order.
- FR-6: Sistem menyimpan riwayat lengkap maintenance per aset.
- FR-7: Sistem menghitung metrik downtime, MTBF, dan MTTR per aset.

## 8. Non-Functional Requirements
- Performance: List work order dan aset (10k+ record) termuat < 2 detik.
- Scalability: Mendukung 100k+ aset dan 500 pengguna concurrent.
- Security: RBAC per role dan per lokasi/site; audit trail perubahan aset.
- Availability: Uptime 99.5%; mobile mendukung mode offline dengan sinkronisasi.
- Compliance: Retensi riwayat maintenance sesuai regulasi industri (mis. keselamatan alat).

## 9. Data Model (High-Level)
- **Asset** (id, name, category, location_id, parent_id, status, purchase_cost). Relasi: punya banyak WorkOrder & MeterReading.
- **WorkOrder** (id, asset_id, type, priority, status, assignee_id, cost). Relasi: milik satu Asset, punya banyak PartUsage.
- **MaintenancePlan** (id, asset_id, trigger_type, interval, checklist_id). Relasi: menghasilkan WorkOrder.
- **Part** (id, sku, name, stock_qty, unit_cost). Relasi: dipakai di banyak WorkOrder.
- **Location** (id, name, parent_id). Relasi: menampung banyak Asset.

## 10. Integrations
- ERP/Finance (SAP, Oracle) untuk biaya dan procurement.
- Inventory/warehouse system untuk stok spare part.
- IoT/sensor gateway untuk meter reading (Phase 2).
- SSO (SAML/OIDC) dan notifikasi (email/WhatsApp).

## 11. Success Metrics (KPI)
- Penurunan unplanned downtime 20% dalam 6 bulan.
- PM compliance rate > 90%.
- Rata-rata waktu penyelesaian work order (MTTR) turun 15%.
- 80% teknisi aktif menggunakan mobile app.

## 12. Risks & Assumptions
- Risiko: kualitas data aset awal buruk saat migrasi dari spreadsheet.
- Risiko: adopsi teknisi rendah bila UI mobile tidak ramah lapangan.
- Asumsi: setiap aset kritis punya identifier unik dan lokasi jelas.
- Asumsi: konektivitas lapangan terbatas sehingga offline mode wajib.

## 13. Roadmap / Phases
- **Phase 1 (MVP):** Asset registry, PM scheduler, work order, mobile execution.
- **Phase 2:** Barcode/QR, inventory penuh, warranty & contract, analytics reliability.
- **Phase 3:** Predictive maintenance IoT, FMEA, integrasi ERP mendalam, multi-site enterprise.
