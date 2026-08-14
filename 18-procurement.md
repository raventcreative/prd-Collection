# PRD: Procurement Management

## 1. Ringkasan
Procurement Management adalah modul untuk mengelola pengadaan barang/jasa mulai dari purchase requisition, purchase order, manajemen vendor, hingga approval workflow. Ditujukan untuk tim procurement, finance, dan approver di perusahaan menengah-besar. Nilai utamanya: proses pengadaan yang transparan, terkendali budget, dan mengurangi maverick spending.

## 2. Problem Statement
Proses pengadaan manual (email, spreadsheet, WhatsApp) menyebabkan permintaan pembelian sulit dilacak, approval lambat, tidak ada kontrol budget, dan riwayat vendor tersebar. Akibatnya rawan pembelian di luar prosedur, harga tidak kompetitif, dan audit trail lemah.

## 3. Goals & Non-Goals
**Goals:**
- Digitalisasi alur PR -> PO -> penerimaan barang (3-way matching).
- Approval workflow berjenjang berbasis nilai dan kategori.
- Kontrol budget per departemen/cost center secara real-time.
- Database vendor terpusat dengan rating dan histori transaksi.

**Non-Goals:**
- Bukan sistem akuntansi/GL penuh (hanya integrasi).
- Tidak mencakup manajemen inventory gudang detail (lihat modul SCM).
- Tidak menangani e-invoicing pajak secara langsung (via integrasi).

## 4. Target Users / Personas
- **Requester (Staff Departemen):** membuat PR cepat tanpa paham detail procurement.
- **Procurement Officer:** mengubah PR jadi PO, negosiasi vendor, bandingkan penawaran.
- **Approver (Manager/Finance):** menyetujui/menolak berdasarkan budget & kebijakan.
- **Vendor Manager:** kelola data vendor, kontrak, dan evaluasi kinerja.

## 5. Key Features
### MVP
- Purchase Requisition dengan item, qty, estimasi harga, cost center.
- Approval workflow berjenjang (rule berdasarkan nominal).
- Konversi PR -> PO dan kirim PO ke vendor (PDF/email).
- Master vendor dengan kontak, kategori, term pembayaran.
- Goods Receipt Note (GRN) dan 3-way matching (PR/PO/GRN).
- Dashboard status pengadaan dan spend per kategori.

### Future / Phase 2+
- RFQ/tender dengan perbandingan penawaran multi-vendor otomatis.
- Kontrak & catalog procurement (punch-out).
- Vendor portal self-service (konfirmasi PO, upload invoice).
- Vendor scorecard otomatis (OTIF, kualitas, harga).

## 6. User Stories
- Sebagai Requester, saya ingin membuat PR dari template agar permintaan cepat dan konsisten.
- Sebagai Approver, saya ingin menyetujui PR dari mobile agar tidak menghambat operasional.
- Sebagai Procurement Officer, saya ingin membandingkan penawaran vendor agar dapat harga terbaik.
- Sebagai Finance, saya ingin melihat sisa budget cost center agar tidak overspend.
- Sebagai Vendor Manager, saya ingin melihat histori kinerja vendor agar keputusan seleksi objektif.

## 7. Functional Requirements
- FR-1: Sistem dapat membuat, mengedit, dan submit PR dengan multiple line item.
- FR-2: Sistem menjalankan approval workflow berbasis rule (nominal, kategori, cost center).
- FR-3: Sistem mengonversi PR yang disetujui menjadi satu atau beberapa PO per vendor.
- FR-4: Sistem melakukan 3-way matching dan menandai selisih (quantity/price mismatch).
- FR-5: Sistem memvalidasi ketersediaan budget cost center sebelum PR di-approve.
- FR-6: Sistem menyimpan master vendor beserta term, kategori, dan dokumen legal.
- FR-7: Sistem mengirim notifikasi (email/in-app) pada setiap perubahan status.
- FR-8: Sistem menyediakan audit trail lengkap untuk setiap transaksi.

## 8. Non-Functional Requirements
- Performance: pencarian PR/PO dan dashboard tampil < 2 detik untuk 10rb+ transaksi.
- Scalability: mendukung 500+ user konkuren dan 100rb transaksi/tahun.
- Security: RBAC granular, enkripsi data at-rest & in-transit, segregation of duties.
- Availability: uptime 99.5%, backup harian, RTO < 4 jam.
- Compliance: audit trail immutable, dukungan kebijakan procurement & pajak lokal.

## 9. Data Model (High-Level)
- **PurchaseRequisition** (nomor, requester, cost_center, status, total). Relasi: punya banyak PRItem, memicu Approval.
- **PurchaseOrder** (nomor, vendor_id, PR_id, term, status, total). Relasi: milik satu Vendor, punya banyak POItem.
- **Vendor** (nama, kategori, term, rating, dokumen). Relasi: punya banyak PO & Contract.
- **GoodsReceipt** (PO_id, tanggal, qty_diterima). Relasi: milik satu PO.
- **Approval** (entity_id, approver, level, keputusan, timestamp). Relasi: polimorfik ke PR/PO.
- **Budget** (cost_center, periode, alokasi, terpakai). Relasi: dipakai validasi PR.

## 10. Integrations
- ERP/Accounting (SAP, Oracle, Accurate) untuk posting GL & AP.
- Email/SMTP untuk pengiriman PO dan notifikasi.
- SSO/Identity (SAML/OIDC) untuk autentikasi.
- e-Signature untuk approval berdokumen.
- Payment gateway/bank untuk instruksi pembayaran (Phase 2).

## 11. Success Metrics (KPI)
- Rata-rata cycle time PR -> PO turun > 40%.
- Persentase maverick spending < 5%.
- Rasio PO dengan 3-way match sukses > 95%.
- Adopsi user aktif > 80% dalam 3 bulan.
- Penghematan harga (cost avoidance) via RFQ terukur.

## 12. Risks & Assumptions
- Risiko: resistensi user terbiasa proses manual -> perlu change management.
- Risiko: data vendor kotor saat migrasi -> perlu data cleansing.
- Asumsi: struktur cost center & budget sudah terdefinisi di finance.
- Asumsi: kebijakan approval matrix disepakati sebelum go-live.

## 13. Roadmap / Phases
- **Phase 1 (MVP):** PR, approval workflow, PO, GRN, 3-way matching, master vendor, dashboard.
- **Phase 2:** RFQ/tender, kontrak & catalog, vendor portal, budget control lanjutan.
- **Phase 3:** vendor scorecard otomatis, analitik spend, integrasi payment & e-invoicing.
