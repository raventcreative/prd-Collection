# PRD: CV Extractor / Resume Parser

## 1. Ringkasan
CV Extractor adalah layanan yang mengubah file resume (PDF/DOCX/gambar) menjadi structured data yang siap dipakai sistem ATS. Ditujukan untuk tim rekrutmen dan HR-tech yang perlu memproses ribuan lamaran tanpa entri manual. Nilai utamanya: mempercepat screening dan menstandarkan data kandidat.

## 2. Problem Statement
Recruiter menerima CV dalam format bebas (layout, bahasa, dan struktur berbeda-beda) sehingga proses ekstraksi data kandidat masih manual, lambat, dan rawan salah ketik. Data yang tidak terstruktur menyulitkan pencarian, matching ke lowongan, dan pelaporan pipeline rekrutmen.

## 3. Goals & Non-Goals
**Goals:**
- Parse CV ke schema terstruktur (kontak, pengalaman, edukasi, skill) dengan akurasi tinggi.
- Dukung PDF, DOCX, dan hasil scan (OCR) dalam Bahasa Indonesia & Inggris.
- Sediakan skor kecocokan kandidat terhadap job description.
- Ekspor data ke format yang kompatibel ATS (JSON, HR-XML).

**Non-Goals:**
- Membangun ATS end-to-end (hanya modul parsing/matching).
- Sourcing kandidat dari job board eksternal.
- Keputusan hiring otomatis tanpa review manusia.

## 4. Target Users / Personas
- **Recruiter (Sinta):** butuh screening cepat dan pencarian kandidat berbasis skill.
- **HR Ops (Budi):** butuh data kandidat konsisten untuk pelaporan dan compliance.
- **Developer HR-tech (Raka):** butuh API andal untuk integrasi ke ATS internal.
- **Hiring Manager (Dewi):** butuh ringkasan kandidat dan skor match yang mudah dibaca.

## 5. Key Features
### MVP
- Upload dan parsing PDF/DOCX ke field terstruktur.
- Ekstraksi kontak, work experience, education, skills, sertifikasi.
- OCR untuk CV hasil scan.
- Normalisasi tanggal, job title, dan skill (taxonomy dasar).
- API REST untuk submit dan retrieve hasil parsing.
- Confidence score per field.

### Future / Phase 2+
- Job matching & ranking berbasis JD.
- Deteksi duplikat kandidat.
- Deteksi gap tanggal & red flag.
- Multi-bahasa tambahan (Mandarin, Arab).
- Bulk import via ZIP dan webhook callback.
- Fine-tuning taxonomy skill per industri.

## 6. User Stories
- Sebagai recruiter, saya ingin upload 200 CV sekaligus agar bisa screening massal tanpa entri manual.
- Sebagai HR Ops, saya ingin data kandidat dalam format seragam agar laporan pipeline akurat.
- Sebagai developer, saya ingin API dengan confidence score agar bisa auto-flag hasil yang perlu review.
- Sebagai hiring manager, saya ingin skor match ke JD agar cepat memilih kandidat prioritas.

## 7. Functional Requirements
- FR-1: Sistem menerima file PDF, DOCX, PNG/JPG maksimal 10 MB per file.
- FR-2: Sistem mengekstrak minimal: nama, email, telepon, pengalaman, edukasi, skills.
- FR-3: Sistem menjalankan OCR untuk dokumen berbasis gambar.
- FR-4: Sistem menormalisasi tanggal ke ISO-8601 dan skill ke taxonomy internal.
- FR-5: Sistem memberikan confidence score 0-1 per field.
- FR-6: Sistem mengekspor hasil dalam JSON dan HR-XML.
- FR-7: API mendukung batch submit dan async retrieval via job ID.
- FR-8: Sistem menandai field ambigu untuk review manual.

## 8. Non-Functional Requirements
- Performance: parsing satu CV < 5 detik (p95), batch async untuk >50 file.
- Scalability: mampu memproses 10.000 CV/jam dengan horizontal scaling worker.
- Security: enkripsi at-rest & in-transit, akses via API key/OAuth, PII masking di log.
- Availability: SLA 99.5%, retry otomatis untuk job gagal.
- Compliance: patuh UU PDP Indonesia & GDPR, retensi data dapat dikonfigurasi.

## 9. Data Model (High-Level)
- **Candidate** (id, nama, email, phone, location). Relasi: 1-N ke Experience, Education, Skill.
- **Experience** (company, title, start_date, end_date, description). Relasi: N-1 ke Candidate.
- **Education** (institution, degree, field, year). Relasi: N-1 ke Candidate.
- **Skill** (name, level, taxonomy_id). Relasi: N-1 ke Candidate.
- **ParseJob** (id, status, file_ref, confidence_avg, created_at). Relasi: 1-1 ke Candidate.

## 10. Integrations
- ATS pihak ketiga (Greenhouse, Lever, ATS internal) via API/HR-XML.
- Layanan OCR (Tesseract/cloud OCR) untuk dokumen scan.
- Object storage (S3-compatible) untuk file mentah.
- Webhook untuk notifikasi selesai parsing.
- SSO/OAuth provider untuk autentikasi.

## 11. Success Metrics (KPI)
- Akurasi ekstraksi field kunci >= 95%.
- Waktu rata-rata parsing < 5 detik/CV.
- Pengurangan waktu screening manual >= 70%.
- Rasio CV yang butuh koreksi manual < 10%.
- Adopsi API: jumlah dokumen diproses per bulan.

## 12. Risks & Assumptions
- Risk: layout CV sangat bervariasi menurunkan akurasi; mitigasi dengan model dan fallback review.
- Risk: kualitas scan buruk menurunkan hasil OCR.
- Risk: bias parsing pada CV non-standar/bahasa campur.
- Asumsi: user punya hak legal memproses data kandidat.
- Asumsi: volume awal cukup untuk validasi akurasi model.

## 13. Roadmap / Phases
- **Phase 1 (MVP):** parsing PDF/DOCX, OCR dasar, ekstraksi field inti, API + confidence score.
- **Phase 2:** job matching & ranking, deteksi duplikat, bulk import, webhook.
- **Phase 3:** multi-bahasa lanjutan, taxonomy per industri, analitik pipeline & dashboard.
