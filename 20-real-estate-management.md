# PRD: Real Estate Management

## 1. Ringkasan
Real Estate Management adalah platform untuk mengelola portofolio properti, listing, penyewa, kontrak sewa (lease), dan permintaan maintenance dalam satu sistem. Ditujukan untuk property manager, pemilik properti, dan tim operasional. Nilai utamanya: sentralisasi data properti, otomasi penagihan sewa, dan penanganan maintenance yang terlacak.

## 2. Problem Statement
Pengelolaan properti masih tersebar di spreadsheet dan dokumen fisik: jadwal sewa, jatuh tempo pembayaran, dan permintaan perbaikan sulit dipantau. Akibatnya sering telat penagihan, okupansi tidak optimal, komunikasi penyewa lambat, dan riwayat maintenance hilang.

## 3. Goals & Non-Goals
**Goals:**
- Sentralisasi portofolio properti dan unit beserta statusnya.
- Kelola siklus lease penuh: draft, aktif, perpanjangan, terminasi.
- Otomasi tagihan sewa, reminder, dan pencatatan pembayaran.
- Sistem tiket maintenance dari request penyewa hingga selesai.

**Non-Goals:**
- Bukan platform marketplace/jual-beli properti publik.
- Tidak mencakup akuntansi penuh (hanya integrasi finance).
- Tidak menangani penilaian/appraisal properti otomatis.

## 4. Target Users / Personas
- **Property Manager:** kelola banyak properti, okupansi, dan lease.
- **Owner/Landlord:** pantau performa aset dan arus kas sewa.
- **Tenant (Penyewa):** ajukan maintenance, lihat tagihan, bayar sewa.
- **Maintenance Staff:** terima dan tutup tiket perbaikan di lapangan.

## 5. Key Features
### MVP
- Master properti & unit (tipe, luas, harga, status okupansi).
- Manajemen listing unit tersedia dengan foto & detail.
- Manajemen tenant & lease (tanggal, nominal, deposit, term).
- Penagihan sewa otomatis + reminder jatuh tempo.
- Pencatatan pembayaran & tunggakan.
- Tiket maintenance dengan status & penugasan.

### Future / Phase 2+
- Portal & aplikasi mobile tenant (self-service).
- Pembayaran online (virtual account, e-wallet).
- Analitik okupansi, yield, dan proyeksi arus kas.
- Manajemen dokumen legal & e-signature lease.
- Integrasi listing ke portal publik (Rumah123, dll).

## 6. User Stories
- Sebagai Property Manager, saya ingin melihat status okupansi semua unit agar cepat isi unit kosong.
- Sebagai Tenant, saya ingin mengajukan permintaan perbaikan dari HP agar cepat ditangani.
- Sebagai Owner, saya ingin melihat laporan pendapatan sewa agar tahu performa aset.
- Sebagai Manager, saya ingin sistem mengirim reminder sewa agar tunggakan berkurang.
- Sebagai Maintenance Staff, saya ingin daftar tiket harian agar kerja terprioritas.

## 7. Functional Requirements
- FR-1: Sistem dapat mengelola hierarki properti -> unit dengan atribut lengkap.
- FR-2: Sistem membuat dan mengelola lease dengan periode, nominal, dan deposit.
- FR-3: Sistem menghasilkan tagihan sewa berkala secara otomatis.
- FR-4: Sistem mengirim reminder jatuh tempo dan menandai tunggakan.
- FR-5: Sistem mencatat pembayaran dan menghitung saldo terutang.
- FR-6: Sistem mengelola tiket maintenance (buat, assign, status, tutup).
- FR-7: Sistem menampilkan listing unit tersedia dengan media.
- FR-8: Sistem menyimpan riwayat lease & maintenance per unit.

## 8. Non-Functional Requirements
- Performance: dashboard portofolio 5rb+ unit tampil < 2 detik.
- Scalability: mendukung ratusan properti dan puluhan ribu unit.
- Security: RBAC, isolasi data per owner, enkripsi dokumen sensitif.
- Availability: uptime 99.5%, backup harian.
- Compliance: kepatuhan kontrak sewa & perlindungan data pribadi (PDP).

## 9. Data Model (High-Level)
- **Property** (nama, alamat, tipe, owner_id). Relasi: punya banyak Unit.
- **Unit** (property_id, nomor, luas, harga, status). Relasi: punya banyak Lease.
- **Tenant** (nama, kontak, identitas). Relasi: punya banyak Lease.
- **Lease** (unit_id, tenant_id, mulai, akhir, nominal, deposit). Relasi: memicu Invoice.
- **Invoice** (lease_id, periode, jumlah, status). Relasi: punya banyak Payment.
- **MaintenanceTicket** (unit_id, tenant_id, kategori, status, assignee). Relasi: milik satu Unit.

## 10. Integrations
- Payment gateway/bank (VA, e-wallet) untuk pembayaran sewa.
- Accounting (Accurate, Xero) untuk rekonsiliasi pendapatan.
- Email/WhatsApp/SMS untuk reminder & notifikasi.
- Peta/Geocoding untuk lokasi properti.
- Portal listing publik untuk sindikasi unit tersedia (Phase 2).

## 11. Success Metrics (KPI)
- Tingkat okupansi portofolio naik ke > 90%.
- Rasio pembayaran tepat waktu > 85%.
- Rata-rata waktu penyelesaian tiket maintenance < 3 hari.
- Waktu isi unit kosong (vacancy days) turun > 30%.
- Adopsi self-service tenant > 60% (Phase 2).

## 12. Risks & Assumptions
- Risiko: adopsi tenant rendah tanpa insentif -> perlu onboarding & edukasi.
- Risiko: kualitas data unit saat migrasi -> perlu verifikasi lapangan.
- Asumsi: struktur biaya sewa & deposit terstandardisasi.
- Asumsi: koneksi payment gateway tersedia untuk pembayaran online.

## 13. Roadmap / Phases
- **Phase 1 (MVP):** properti/unit, lease, penagihan otomatis, pembayaran, tiket maintenance.
- **Phase 2:** portal tenant, pembayaran online, sindikasi listing, e-signature.
- **Phase 3:** analitik okupansi & yield, proyeksi arus kas, mobile app penuh.
