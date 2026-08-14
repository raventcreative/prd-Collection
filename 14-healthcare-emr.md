# PRD: Healthcare EMR (Electronic Medical Records)

## 1. Ringkasan
EMR adalah sistem rekam medis elektronik untuk klinik dan rumah sakit yang mengelola data pasien, riwayat kunjungan (encounter), diagnosis, dan resep elektronik (e-prescribing). Ditujukan untuk dokter, perawat, apoteker, dan staf administrasi. Nilai utamanya: menggantikan rekam medis kertas dengan data terstruktur, aman, dan dapat diakses lintas unit dengan jejak audit penuh.

## 2. Problem Statement
Fasilitas kesehatan masih banyak mengandalkan rekam medis kertas yang mudah hilang, sulit dibaca, dan tidak dapat diakses bersamaan. Riwayat pasien terfragmentasi antar poli, resep ditulis tangan sehingga rawan kesalahan dosis, dan pelaporan ke regulator dilakukan manual. Ini berisiko pada keselamatan pasien, kepatuhan regulasi, dan efisiensi operasional.

## 3. Goals & Non-Goals
**Goals:**
- Rekam medis pasien terpusat, terstruktur, dan aman.
- Dokumentasi encounter yang lengkap (SOAP, diagnosis ICD-10).
- E-prescribing dengan pengecekan interaksi & alergi obat.
- Jejak audit & kontrol akses sesuai regulasi privasi kesehatan.
- Akses cepat riwayat pasien lintas poli/unit.

**Non-Goals:**
- Bukan sistem billing/klaim asuransi penuh (integrasi saja di MVP).
- Bukan PACS/imaging viewer (hanya link/hasil).
- Tidak mencakup telemedicine video di MVP.
- Tidak menggantikan LIS/lab analyzer (integrasi hasil saja).

## 4. Target Users / Personas
- **Dokter:** dokumentasi encounter, diagnosis, buat resep, lihat riwayat.
- **Perawat:** input vital sign, triase, catat asuhan keperawatan.
- **Apoteker:** verifikasi & dispensing resep elektronik.
- **Admin/Front Office:** registrasi pasien, kelola jadwal & antrian.

## 5. Key Features
### MVP
- Master data pasien & demografi (unik per MRN).
- Registrasi & manajemen encounter (rawat jalan).
- Dokumentasi klinis SOAP + diagnosis ICD-10.
- Input vital sign & alergi.
- E-prescribing dengan drug interaction & allergy checking.
- Riwayat medis pasien lintas kunjungan.
- Audit log & RBAC berbasis peran klinis.

### Future / Phase 2+
- Modul rawat inap & bed management.
- Integrasi LIS (lab) & RIS/PACS (radiologi).
- Billing & klaim BPJS/asuransi.
- Telemedicine & patient portal.
- Clinical decision support & analitik populasi.

## 6. User Stories
- Sebagai Admin, saya ingin mendaftarkan pasien baru dengan MRN unik agar tidak ada duplikasi rekam.
- Sebagai Perawat, saya ingin mencatat vital sign & alergi agar dokter punya data lengkap.
- Sebagai Dokter, saya ingin menulis catatan SOAP dan diagnosis ICD-10 agar encounter terdokumentasi.
- Sebagai Dokter, saya ingin membuat resep elektronik agar apoteker menerima instruksi jelas & terbaca.
- Sebagai Dokter, saya ingin diperingatkan bila resep berinteraksi/alergi agar pasien aman.
- Sebagai Apoteker, saya ingin melihat & memverifikasi resep elektronik agar dispensing akurat.
- Sebagai Dokter, saya ingin melihat riwayat kunjungan sebelumnya agar keputusan klinis berdasar data.
- Sebagai Auditor, saya ingin melihat log akses rekam agar kepatuhan privasi terjaga.

## 7. Functional Requirements
- FR-1: Sistem membuat MRN unik dan mendeteksi kemungkinan duplikat pasien.
- FR-2: Setiap kunjungan tercatat sebagai encounter dengan tipe, tanggal, dan penanggung jawab.
- FR-3: Dokumentasi klinis mendukung format SOAP dan pengkodean diagnosis ICD-10.
- FR-4: Modul e-prescribing menyimpan obat, dosis, frekuensi, durasi terstruktur.
- FR-5: Sistem mengecek interaksi obat-obat dan alergi pasien saat resep dibuat, menampilkan peringatan.
- FR-6: Riwayat medis pasien dapat ditelusuri kronologis lintas encounter.
- FR-7: Semua akses & perubahan rekam medis dicatat dalam audit log yang immutable.
- FR-8: RBAC membatasi akses data klinis sesuai peran dan relasi perawatan (minimum necessary).
- FR-9: Sistem mendukung amandemen catatan dengan menyimpan versi asli (no hard delete).

## 8. Non-Functional Requirements
- **Compliance:** patuh regulasi privasi kesehatan — HIPAA (jika beroperasi/berbagi data dengan pihak AS) dan UU PDP serta Permenkes 24/2022 tentang Rekam Medis Elektronik di Indonesia. Wajib audit trail, konsen pasien, dan enkripsi PHI.
- **Security:** enkripsi PHI at-rest (AES-256) & in-transit (TLS 1.2+); MFA untuk klinisi; prinsip minimum necessary access.
- **Availability:** uptime 99.9%; sistem klinis tidak boleh kehilangan data (RPO ~0).
- **Performance:** load rekam pasien < 2 detik.
- **Reliability:** backup & disaster recovery; data retention sesuai regulasi.
- **Interoperability:** dukungan standar HL7/FHIR untuk pertukaran data.

## 9. Data Model (High-Level)
- **Patient** (id, MRN, nama, DOB, gender, kontak, alergi[]).
- **Encounter** (id, patient_id, tipe, tanggal, provider_id, status).
- **ClinicalNote** (id, encounter_id, subjective, objective, assessment, plan, versi).
- **Diagnosis** (id, encounter_id, icd10_code, deskripsi).
- **Vital** (id, encounter_id, tekanan_darah, nadi, suhu, dll).
- **Prescription** (id, encounter_id, patient_id, status) → **PrescriptionItem** (obat, dosis, frekuensi, durasi).
- **AuditLog** (id, user_id, action, entity, timestamp).
- Relasi: Patient 1-N Encounter; Encounter 1-N ClinicalNote/Diagnosis/Vital/Prescription.

## 10. Integrations
- Standar HL7 / FHIR untuk interoperabilitas.
- Drug database (interaksi & alergi, mis. terminologi obat nasional/RxNorm).
- SATUSEHAT (platform interoperabilitas Kemenkes RI).
- LIS/lab & RIS/PACS (Phase 2).
- BPJS/asuransi untuk verifikasi kepesertaan & klaim (Phase 2).
- SSO & directory klinisi.

## 11. Success Metrics (KPI)
- Waktu dokumentasi encounter turun > 30%.
- 100% resep melewati drug-interaction check.
- Insiden medication error turun > 40%.
- Waktu akses riwayat pasien < 2 detik pada p95.
- 0 pelanggaran akses tanpa jejak audit.

## 12. Risks & Assumptions
- **Risiko:** kebocoran PHI berdampak hukum & reputasi berat.
- **Risiko:** resistensi klinisi terhadap perubahan alur kerja.
- **Risiko:** ketergantungan pada database obat yang harus selalu update.
- **Asumsi:** fasilitas memiliki infrastruktur & konektivitas memadai.
- **Asumsi:** tersedia konsen pasien untuk pemrosesan data kesehatan.

## 13. Roadmap / Phases
- **Phase 1 (MVP):** pasien, encounter rawat jalan, SOAP + ICD-10, e-prescribing dengan safety check, audit & RBAC.
- **Phase 2:** rawat inap, integrasi LIS/PACS, SATUSEHAT, billing/BPJS.
- **Phase 3:** telemedicine, patient portal, clinical decision support & analitik populasi.
