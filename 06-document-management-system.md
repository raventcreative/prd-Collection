# PRD: Document Management System (DMS)

## 1. Ringkasan
DMS adalah sistem penyimpanan dokumen terpusat dengan versioning, kontrol akses, dan full-text search. Ditujukan untuk perusahaan dan tim yang perlu mengelola dokumen resmi, kontrak, dan SOP secara aman dan terlacak. Nilai utamanya: satu sumber kebenaran dokumen yang mudah dicari dan diaudit.

## 2. Problem Statement
Dokumen tersebar di email, shared drive, dan laptop personal tanpa versi yang jelas. Karyawan sulit menemukan dokumen terbaru, sering memakai versi kadaluarsa, dan tidak ada jejak siapa mengubah apa. Ini menimbulkan risiko compliance dan kehilangan data.

## 3. Goals & Non-Goals
**Goals:**
- Sediakan penyimpanan dokumen terpusat dengan folder/struktur.
- Kelola versi dokumen dengan riwayat lengkap.
- Kontrol akses granular per folder/dokumen.
- Full-text search termasuk isi dokumen (PDF/Office).

**Non-Goals:**
- Editor dokumen kolaboratif real-time (seperti Google Docs) di MVP.
- E-signature bawaan (gunakan integrasi).
- Sistem manajemen proyek/task.

## 4. Target Users / Personas
- **Karyawan (Tari):** butuh cari dan akses dokumen terbaru dengan cepat.
- **Manager (Hendra):** butuh approval dan kontrol siapa mengakses dokumen sensitif.
- **Legal/Compliance (Sari):** butuh audit trail dan retensi dokumen.
- **IT Admin (Fajar):** butuh kelola user, permission, dan storage.

## 5. Key Features
### MVP
- Upload, download, dan organisasi folder.
- Versioning otomatis dengan riwayat dan restore.
- Permission berbasis role/user/group.
- Full-text search (metadata + isi dokumen).
- Preview dokumen umum (PDF, DOCX, gambar).
- Audit log akses dan perubahan.

### Future / Phase 2+
- Workflow approval dokumen.
- Retensi & auto-archival policy.
- OCR untuk dokumen scan.
- E-signature integration.
- Tagging otomatis & klasifikasi AI.
- Check-in/check-out (lock editing).

## 6. User Stories
- Sebagai karyawan, saya ingin cari dokumen berdasarkan isi agar cepat menemukan yang benar.
- Sebagai manager, saya ingin batasi akses folder tertentu agar dokumen sensitif aman.
- Sebagai compliance, saya ingin lihat riwayat versi agar tahu perubahan dan penanggung jawab.
- Sebagai IT admin, saya ingin kelola permission per group agar onboarding cepat.

## 7. Functional Requirements
- FR-1: Sistem menyimpan file dengan struktur folder bertingkat.
- FR-2: Sistem membuat versi baru setiap upload/update dan menyimpan riwayat.
- FR-3: Sistem mendukung restore ke versi sebelumnya.
- FR-4: Sistem menerapkan permission (view/edit/delete/share) per user/group.
- FR-5: Sistem mengindeks isi dokumen untuk full-text search.
- FR-6: Sistem menampilkan preview tanpa perlu download.
- FR-7: Sistem mencatat audit log (siapa, apa, kapan).
- FR-8: Sistem mendukung metadata kustom dan tag per dokumen.

## 8. Non-Functional Requirements
- Performance: hasil search < 1 detik untuk korpus 1 juta dokumen.
- Scalability: storage terukur hingga terabyte, indexing async.
- Security: enkripsi at-rest & in-transit, RBAC, signed URL untuk download.
- Availability: SLA 99.9%, backup & disaster recovery.
- Compliance: retensi konfigurable, patuh UU PDP & standar ISO 27001.

## 9. Data Model (High-Level)
- **Document** (id, name, folder_id, current_version_id, tags, metadata). Relasi: 1-N ke Version.
- **Version** (id, document_id, version_no, file_ref, size, uploaded_by, created_at). Relasi: N-1 ke Document.
- **Folder** (id, name, parent_id). Relasi: self-referential tree, 1-N ke Document.
- **Permission** (id, resource_id, principal_id, level). Relasi: N-N user/group ke Document/Folder.
- **AuditLog** (id, actor_id, action, resource_id, timestamp). Relasi: N-1 ke Document/Folder.

## 10. Integrations
- Object storage (S3-compatible) untuk file.
- Search engine (Elasticsearch/OpenSearch) untuk full-text index.
- SSO/LDAP untuk autentikasi & group.
- E-signature provider (DocuSign/PrivyID) via integrasi.
- Antivirus scanning saat upload.

## 11. Success Metrics (KPI)
- Waktu rata-rata menemukan dokumen < 30 detik.
- Pengurangan penggunaan versi kadaluarsa (insiden) signifikan.
- Adopsi: persentase dokumen tersimpan di DMS vs drive lama.
- Kepatuhan audit: 100% dokumen sensitif memiliki access control.
- Uptime pencarian & storage sesuai SLA.

## 12. Risks & Assumptions
- Risk: migrasi dokumen lama besar dan berantakan; sediakan tools import bertahap.
- Risk: indexing isi dokumen berat pada file besar; batasi & async.
- Risk: permission salah konfigurasi membocorkan data; audit berkala.
- Asumsi: user mau menyimpan dokumen di sistem baru.
- Asumsi: format dokumen mayoritas didukung untuk indexing/preview.

## 13. Roadmap / Phases
- **Phase 1 (MVP):** upload/folder, versioning, permission, full-text search, preview, audit log.
- **Phase 2:** workflow approval, retensi policy, OCR, check-in/check-out.
- **Phase 3:** e-signature, klasifikasi AI & auto-tagging, analitik penggunaan dokumen.
