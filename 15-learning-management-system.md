# PRD: Learning Management System (LMS)

## 1. Ringkasan
LMS adalah platform untuk membuat, mendistribusikan, dan melacak pembelajaran online berupa kursus, kuis, dan sertifikat. Ditujukan untuk institusi pendidikan, tim L&D korporat, dan penyedia pelatihan. Nilai utamanya: mengelola pembelajaran end-to-end dengan pelacakan progres dan sertifikasi otomatis.

## 2. Problem Statement
Materi pelatihan tersebar di file, video, dan email tanpa cara terstruktur melacak siapa sudah belajar apa. Pengajar sulit menilai progres peserta, dan peserta kehilangan bukti penyelesaian. Onboarding dan pelatihan kepatuhan jadi manual, lambat, dan tidak terukur.

## 3. Goals & Non-Goals
**Goals:**
- Memungkinkan pembuatan kursus terstruktur (modul, lesson, materi multimedia).
- Mengelola enrollment peserta secara manual dan otomatis.
- Menyediakan kuis/asesmen dengan penilaian otomatis.
- Melacak progres belajar dan menerbitkan sertifikat penyelesaian.

**Non-Goals:**
- Bukan tool authoring video/konten canggih (integrasi saja).
- Tidak mencakup video conferencing live penuh (integrasi Zoom).
- Bukan sistem SIS/akademik penuh (nilai rapor, jadwal kelas fisik).

## 4. Target Users / Personas
- **Instruktur/Course Creator (Pak Anwar):** butuh membuat kursus, kuis, dan memantau peserta.
- **Peserta/Learner (Maya):** butuh mengakses materi, mengerjakan kuis, dan melihat progres.
- **L&D Admin (Ibu Tuti):** butuh mengelola enrollment, laporan kepatuhan, dan sertifikat.
- **Manager (Doni):** butuh laporan penyelesaian pelatihan tim-nya.

## 5. Key Features
### MVP
- Course builder (modul, lesson, upload video/PDF/teks).
- Enrollment (self-enroll & admin-assign).
- Kuis dengan multiple choice & penilaian otomatis.
- Progress tracking per lesson dan per kursus.
- Sertifikat penyelesaian otomatis (PDF).

### Future / Phase 2+
- Gamifikasi (badge, poin, leaderboard).
- Learning path & prerequisite antar kursus.
- Diskusi/forum dan penilaian peer.
- SCORM/xAPI import dan integrasi konten eksternal.
- Live session, kelas cohort, dan mobile app native.

## 6. User Stories
- Sebagai instruktur, saya ingin membuat kursus dengan modul dan kuis agar peserta belajar terstruktur.
- Sebagai peserta, saya ingin melanjutkan kursus dari titik terakhir agar tidak mengulang.
- Sebagai peserta, saya ingin mendapat sertifikat otomatis setelah lulus kuis agar punya bukti.
- Sebagai L&D admin, saya ingin melihat laporan siapa yang belum menyelesaikan pelatihan wajib.

## 7. Functional Requirements
- FR-1: Sistem dapat membuat kursus berisi modul dan lesson dengan konten multimedia.
- FR-2: Sistem mendukung enrollment manual oleh admin dan self-enrollment oleh peserta.
- FR-3: Sistem menampilkan dan menyimpan progres per lesson (started, completed).
- FR-4: Sistem menyediakan kuis dengan tipe soal dan menilai otomatis berdasarkan passing score.
- FR-5: Sistem menerbitkan sertifikat PDF saat kriteria penyelesaian terpenuhi.
- FR-6: Sistem menghasilkan laporan progres dan penyelesaian per peserta/kelompok.
- FR-7: Sistem membatasi akses konten sesuai enrollment dan role.

## 8. Non-Functional Requirements
- Performance: Halaman kursus & video termuat < 2 detik; streaming adaptif.
- Scalability: Mendukung 100k peserta dan 10k concurrent saat ujian.
- Security: RBAC, proteksi konten, anti-cheating dasar pada kuis.
- Availability: Uptime 99.5%; auto-save jawaban kuis.
- Compliance: GDPR/UU PDP untuk data peserta; aksesibilitas WCAG 2.1 AA.

## 9. Data Model (High-Level)
- **Course** (id, title, description, status, creator_id). Relasi: punya banyak Module.
- **Module** (id, course_id, order). Relasi: punya banyak Lesson.
- **Lesson** (id, module_id, type, content_url, order). Relasi: dilacak oleh Progress.
- **Enrollment** (id, user_id, course_id, status, enrolled_at). Relasi: menghubungkan User & Course.
- **Quiz** (id, lesson_id, passing_score). Relasi: punya banyak Question & Attempt.
- **Certificate** (id, user_id, course_id, issued_at, serial). Relasi: milik satu Enrollment.

## 10. Integrations
- SSO (Google/Microsoft/SAML) untuk login.
- Video hosting/CDN (Vimeo, Cloudflare Stream) untuk konten.
- Payment gateway untuk kursus berbayar (Phase 2).
- Video conferencing (Zoom) untuk sesi live (Phase 2).
- Email/notifikasi untuk reminder dan sertifikat.

## 11. Success Metrics (KPI)
- Course completion rate > 70%.
- Rata-rata skor kuis dan tingkat kelulusan meningkat.
- Waktu onboarding pegawai baru turun 30%.
- Retensi bulanan peserta aktif > 60%.

## 12. Risks & Assumptions
- Risiko: engagement rendah bila konten tidak menarik/interaktif.
- Risiko: kecurangan pada kuis tanpa proctoring.
- Asumsi: peserta punya koneksi internet memadai untuk video.
- Asumsi: instruktur mampu menyiapkan konten berkualitas.

## 13. Roadmap / Phases
- **Phase 1 (MVP):** Course builder, enrollment, kuis, progress, sertifikat.
- **Phase 2:** Gamifikasi, learning path, forum, payment, SCORM/xAPI.
- **Phase 3:** Live cohort, mobile app native, AI rekomendasi & adaptive learning.
