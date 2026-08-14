# PRD: HRM (Human Resource Management)

## 1. Ringkasan
HRM adalah platform pengelolaan sumber daya manusia terpusat untuk perusahaan skala kecil-menengah yang menyatukan data karyawan, payroll, cuti, dan penilaian kinerja dalam satu sistem. Ditujukan untuk tim HR, manajer lini, dan karyawan. Nilai utamanya: menghilangkan spreadsheet manual, memastikan payroll akurat & patuh pajak, serta memberi self-service bagi karyawan.

## 2. Problem Statement
Banyak perusahaan mengelola data karyawan di spreadsheet terpisah, menghitung payroll dan potongan pajak (PPh 21) secara manual, dan menyetujui cuti lewat chat/email tanpa jejak audit. Akibatnya sering terjadi salah hitung gaji, saldo cuti tidak konsisten, penilaian kinerja tidak terdokumentasi, dan HR kehilangan banyak waktu untuk pekerjaan administratif berulang.

## 3. Goals & Non-Goals
**Goals:**
- Single source of truth untuk data master karyawan.
- Otomasi perhitungan payroll termasuk potongan pajak & BPJS.
- Workflow pengajuan & approval cuti dengan saldo otomatis.
- Siklus penilaian kinerja terstruktur (goal, review, rating).
- Employee self-service (slip gaji, ajukan cuti, update data).

**Non-Goals:**
- Bukan sistem rekrutmen/ATS (di luar scope MVP).
- Bukan LMS/training management penuh.
- Tidak menangani manajemen shift/rostering kompleks di MVP.
- Tidak menggantikan software akuntansi umum (hanya export ke sana).

## 4. Target Users / Personas
- **HR Admin:** kelola data karyawan, jalankan payroll bulanan, kelola kebijakan cuti.
- **Manajer Lini:** approve cuti, isi & kelola penilaian kinerja tim.
- **Karyawan:** akses slip gaji, ajukan cuti, lihat saldo & data pribadi.
- **Finance:** verifikasi hasil payroll, export jurnal ke sistem akuntansi.

## 5. Key Features
### MVP
- Master data karyawan (profil, kontrak, kompensasi, dokumen).
- Payroll engine: komponen gaji, tunjangan, potongan, PPh 21, BPJS.
- Manajemen cuti: tipe cuti, saldo, pengajuan, approval berjenjang.
- Slip gaji digital & riwayat.
- Penilaian kinerja: goal setting, review periodik, rating.
- Role-based access & employee self-service portal.

### Future / Phase 2+
- Attendance & integrasi mesin absensi/fingerprint.
- Rekrutmen/ATS & onboarding otomatis.
- Reimbursement & claim expense.
- OKR/360-feedback lanjutan.
- Analitik turnover & headcount forecasting.

## 6. User Stories
- Sebagai HR Admin, saya ingin menambah karyawan baru beserta komponen gajinya agar payroll langsung memasukkannya.
- Sebagai HR Admin, saya ingin menjalankan payroll satu klik agar semua gaji & potongan terhitung otomatis.
- Sebagai Karyawan, saya ingin mengajukan cuti dari portal agar tidak perlu email manual.
- Sebagai Manajer, saya ingin menyetujui/menolak cuti tim saya agar saldo terupdate otomatis.
- Sebagai Karyawan, saya ingin mengunduh slip gaji bulanan agar punya bukti pendapatan.
- Sebagai Manajer, saya ingin menetapkan goal dan memberi rating kinerja agar review terdokumentasi.
- Sebagai Finance, saya ingin export jurnal payroll agar bisa dicatat di sistem akuntansi.

## 7. Functional Requirements
- FR-1: Sistem menyimpan data master karyawan dengan versi histori perubahan kontrak & gaji.
- FR-2: Payroll engine menghitung gaji bruto, potongan PPh 21, BPJS Kesehatan & Ketenagakerjaan, dan gaji netto.
- FR-3: Sistem mendukung run payroll per periode dengan status draft, review, finalized.
- FR-4: Karyawan dapat mengajukan cuti; sistem memvalidasi saldo sebelum submit.
- FR-5: Approval cuti mengikuti hierarki organisasi (approver = atasan langsung).
- FR-6: Slip gaji dihasilkan otomatis (PDF) dan dapat diunduh karyawan.
- FR-7: Modul kinerja mendukung siklus goal, mid-review, final rating dengan skala terkonfigurasi.
- FR-8: RBAC membatasi akses data per peran (HR, manajer, karyawan, finance).
- FR-9: Sistem menyediakan export payroll ke format CSV/jurnal akuntansi.

## 8. Non-Functional Requirements
- **Security:** enkripsi data sensitif (gaji, NIK) at-rest & in-transit; audit log untuk perubahan payroll.
- **Compliance:** patuh regulasi ketenagakerjaan & pajak Indonesia (PPh 21, BPJS, UU PDP).
- **Performance:** run payroll 500 karyawan < 60 detik.
- **Availability:** uptime 99.5%; backup harian.
- **Scalability:** mendukung multi-company/multi-cabang.
- **Usability:** self-service portal mobile-friendly.

## 9. Data Model (High-Level)
- **Employee** (id, nama, NIK, jabatan, department_id, join_date, status, manager_id).
- **Contract** (id, employee_id, tipe, mulai, akhir, base_salary).
- **PayComponent** (id, employee_id, tipe [tunjangan/potongan], nominal, taxable).
- **PayrollRun** (id, periode, status, total) → **Payslip** (id, run_id, employee_id, bruto, potongan, netto).
- **LeaveType** (id, nama, kuota) & **LeaveRequest** (id, employee_id, type_id, tanggal, status, approver_id).
- **PerformanceReview** (id, employee_id, cycle, goals, rating, reviewer_id).
- Relasi: Employee 1-N Contract, Employee 1-N Payslip, Employee 1-N LeaveRequest.

## 10. Integrations
- Sistem akuntansi (Accurate, Jurnal, Xero) via export/API.
- Payment gateway/bank untuk disbursement gaji (transfer massal).
- Mesin absensi/fingerprint (Phase 2).
- Email & notifikasi (SMTP, WhatsApp Business API) untuk approval.
- SSO (Google Workspace / Microsoft Entra).

## 11. Success Metrics (KPI)
- Waktu proses payroll bulanan turun > 70%.
- Tingkat error payroll < 0.5% per run.
- Adopsi self-service > 80% karyawan aktif.
- Waktu rata-rata approval cuti < 24 jam.
- Penyelesaian siklus penilaian kinerja > 90% tepat waktu.

## 12. Risks & Assumptions
- **Risiko:** perubahan regulasi pajak/BPJS memerlukan update engine cepat.
- **Risiko:** data payroll salah berdampak legal & kepercayaan karyawan.
- **Risiko:** migrasi data dari spreadsheet lama rawan error.
- **Asumsi:** perusahaan punya struktur organisasi jelas untuk hierarki approval.
- **Asumsi:** karyawan punya akses email/smartphone untuk self-service.

## 13. Roadmap / Phases
- **Phase 1 (MVP):** master data, payroll engine, cuti, slip gaji, self-service, RBAC.
- **Phase 2:** attendance & absensi, reimbursement, integrasi bank disbursement.
- **Phase 3:** rekrutmen/ATS, OKR & 360-feedback, analitik HR & forecasting.
