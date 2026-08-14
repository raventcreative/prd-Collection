# PRD: Personal Finance Tracker

## 1. Ringkasan
Personal Finance Tracker adalah aplikasi untuk mencatat transaksi, mengatur anggaran, dan mencapai target tabungan. Ditujukan untuk individu dan keluarga yang ingin memahami dan mengendalikan keuangan pribadi. Nilai utamanya: gambaran jelas ke mana uang mengalir dan disiplin anggaran lewat kategorisasi otomatis.

## 2. Problem Statement
Banyak orang tidak tahu ke mana uang mereka habis setiap bulan karena transaksi tersebar di berbagai rekening, e-wallet, dan tunai. Pencatatan manual melelahkan dan cepat ditinggalkan, sehingga anggaran gagal dan target tabungan tidak tercapai. Tidak ada satu tempat untuk melihat gambaran keuangan menyeluruh.

## 3. Goals & Non-Goals
**Goals:**
- Mencatat pemasukan dan pengeluaran dengan cepat dan mudah.
- Mengkategorikan transaksi secara otomatis dan manual.
- Menetapkan anggaran per kategori dan memantau realisasinya.
- Membuat dan melacak target tabungan (savings goals).

**Non-Goals:**
- Bukan platform investasi/trading saham.
- Tidak memberi nasihat keuangan berlisensi.
- Bukan software akuntansi bisnis/pajak.

## 4. Target Users / Personas
- **Young Professional (Rian):** butuh melacak pengeluaran harian dan menabung untuk tujuan.
- **Keluarga/Pasangan (Nita & suami):** butuh anggaran bersama dan pantau pengeluaran rumah tangga.
- **Mahasiswa (Bagas):** butuh kontrol uang saku terbatas dengan anggaran ketat.
- **Freelancer (Sinta):** butuh melacak pemasukan tidak tetap dan menyisihkan dana.

## 5. Key Features
### MVP
- Pencatatan transaksi manual (income/expense) dengan kategori.
- Kategori default + kustom dan kategorisasi otomatis berbasis rule.
- Budget per kategori dengan indikator sisa/over.
- Savings goals dengan progres.
- Dashboard ringkasan bulanan (cashflow, top kategori).

### Future / Phase 2+
- Auto-import transaksi via bank/e-wallet (Open Banking).
- Recurring transactions & reminder tagihan.
- Multi-currency dan akun bersama (shared budget).
- Insight/AI kategorisasi dan prediksi cashflow.
- Ekspor laporan (PDF/CSV) dan scan struk (OCR).

## 6. User Stories
- Sebagai pengguna, saya ingin mencatat pengeluaran dalam beberapa detik agar tidak malas.
- Sebagai pengguna, saya ingin transaksi otomatis masuk kategori agar tidak repot menyortir.
- Sebagai pengguna, saya ingin diberi peringatan saat anggaran kategori hampir habis.
- Sebagai pengguna, saya ingin melihat progres target tabungan agar termotivasi.

## 7. Functional Requirements
- FR-1: Sistem dapat mencatat transaksi dengan jumlah, tanggal, kategori, akun, dan catatan.
- FR-2: Sistem mengategorikan transaksi otomatis berdasarkan rule (mis. merchant keyword).
- FR-3: Sistem memungkinkan pembuatan budget bulanan per kategori dan menghitung realisasi.
- FR-4: Sistem memberi notifikasi saat pengeluaran melebihi ambang budget.
- FR-5: Sistem mengelola savings goal dengan target amount, deadline, dan progres.
- FR-6: Sistem menampilkan dashboard cashflow dan breakdown per kategori/periode.
- FR-7: Sistem mendukung banyak akun (bank, e-wallet, tunai) dengan saldo terpisah.

## 8. Non-Functional Requirements
- Performance: Input transaksi dan dashboard terbuka < 1 detik.
- Scalability: Mendukung jutaan pengguna dengan ribuan transaksi per akun.
- Security: Enkripsi data finansial, autentikasi biometrik/PIN, enkripsi at-rest.
- Availability: Uptime 99.5%; mode offline dengan sinkronisasi.
- Compliance: UU PDP/GDPR; jika Open Banking, patuh standar & lisensi terkait.

## 9. Data Model (High-Level)
- **User** (id, name, email, currency). Relasi: punya banyak Account & Budget.
- **Account** (id, user_id, name, type, balance). Relasi: menampung banyak Transaction.
- **Transaction** (id, account_id, amount, type, category_id, date, note). Relasi: milik satu Account & Category.
- **Category** (id, user_id, name, type, parent_id). Relasi: dipakai banyak Transaction.
- **Budget** (id, user_id, category_id, period, limit_amount). Relasi: memantau Transaction.
- **SavingsGoal** (id, user_id, name, target_amount, saved_amount, deadline). Relasi: milik satu User.

## 10. Integrations
- Open Banking / aggregator (mis. Brankas, Plaid) untuk auto-import (Phase 2).
- Payment/e-wallet API untuk sinkronisasi saldo.
- Push notification service (FCM/APNs).
- Cloud backup & auth (Firebase/Supabase).
- OCR service untuk scan struk (Phase 2).

## 11. Success Metrics (KPI)
- Retensi 30 hari > 40% (kebiasaan mencatat terbentuk).
- Rata-rata transaksi tercatat per pengguna aktif per minggu.
- % pengguna yang menetapkan minimal satu budget/goal > 60%.
- Tingkat penyelesaian savings goal.

## 12. Risks & Assumptions
- Risiko: pengguna berhenti mencatat manual bila terlalu merepotkan.
- Risiko: akses Open Banking terbatas regulasi dan biaya.
- Asumsi: pengguna bersedia mengategorikan sebagian transaksi.
- Asumsi: data finansial sensitif, kepercayaan keamanan jadi penentu adopsi.

## 13. Roadmap / Phases
- **Phase 1 (MVP):** Transaksi manual, kategori, budget, savings goal, dashboard.
- **Phase 2:** Auto-import bank/e-wallet, recurring, reminder, ekspor, OCR struk.
- **Phase 3:** Multi-currency, shared budget keluarga, AI insight & prediksi cashflow.
