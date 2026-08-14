# PRD: Social Media Analytics

## 1. Ringkasan
Social Media Analytics adalah platform untuk mengumpulkan, menganalisis, dan melaporkan metrik dari berbagai platform sosial (Instagram, TikTok, Facebook, X, YouTube). Ditujukan untuk social media manager, tim marketing, dan agency. Nilai utamanya: satu dashboard untuk performa multi-platform, analisis sentimen, benchmarking kompetitor, dan laporan otomatis.

## 2. Problem Statement
Data performa sosial tersebar di dashboard native tiap platform dengan definisi metrik berbeda. Tim harus mengunduh dan menggabungkan manual di spreadsheet, membuat pelaporan lambat, tidak konsisten, dan sulit membandingkan performa antar platform maupun kompetitor.

## 3. Goals & Non-Goals
**Goals:**
- Agregasi metrik multi-platform ke satu dashboard konsisten.
- Analisis sentimen komentar/mention secara otomatis.
- Benchmarking terhadap akun kompetitor.
- Laporan terjadwal otomatis (PDF/link) yang bisa di-branding.

**Non-Goals:**
- Bukan tool scheduling/publishing konten (hanya analitik).
- Tidak mencakup manajemen ads/campaign spend detail (integrasi opsional).
- Tidak menyimpan DM/pesan privat pengguna.

## 4. Target Users / Personas
- **Social Media Manager:** pantau performa harian & optimasi konten.
- **Marketing Lead:** butuh ringkasan KPI & tren untuk keputusan strategi.
- **Agency Account Manager:** kelola banyak klien & kirim laporan berkala.
- **Content Creator/Analyst:** cari konten & waktu posting berperforma terbaik.

## 5. Key Features
### MVP
- Koneksi akun multi-platform via OAuth resmi.
- Dashboard metrik inti (reach, engagement, follower growth, impressions).
- Analisis performa per postingan dengan filter tanggal & tipe konten.
- Analisis sentimen komentar (positif/negatif/netral).
- Benchmarking dasar terhadap 1-3 kompetitor.
- Laporan terjadwal (PDF/email) dengan template.

### Future / Phase 2+
- Sentimen berbasis topik & deteksi krisis (spike negatif).
- Rekomendasi waktu & tipe konten optimal (AI).
- Social listening mention brand di luar akun sendiri.
- White-label dashboard untuk klien agency.
- Prediksi tren engagement.

## 6. User Stories
- Sebagai SM Manager, saya ingin melihat semua platform di satu dashboard agar hemat waktu.
- Sebagai Marketing Lead, saya ingin membandingkan performa dengan kompetitor agar tahu posisi brand.
- Sebagai Analyst, saya ingin tahu jenis konten paling engaging agar konten berikutnya lebih baik.
- Sebagai Agency AM, saya ingin laporan otomatis per klien agar tidak menyusun manual.
- Sebagai SM Manager, saya ingin peringatan lonjakan sentimen negatif agar cepat merespons.

## 7. Functional Requirements
- FR-1: Sistem menghubungkan akun via OAuth dan menarik data secara berkala.
- FR-2: Sistem menormalkan metrik lintas platform ke skema seragam.
- FR-3: Sistem menghitung engagement rate & pertumbuhan follower per periode.
- FR-4: Sistem mengklasifikasi sentimen teks komentar/mention.
- FR-5: Sistem membandingkan metrik akun sendiri vs kompetitor.
- FR-6: Sistem menghasilkan laporan terjadwal dalam PDF & link.
- FR-7: Sistem mengirim alert saat metrik melewati threshold.
- FR-8: Sistem menyimpan histori data untuk analisis tren jangka panjang.

## 8. Non-Functional Requirements
- Performance: dashboard interaktif < 3 detik untuk rentang 12 bulan.
- Scalability: mendukung ribuan akun terhubung & jutaan data point/hari.
- Security: token OAuth terenkripsi, RBAC, isolasi data per workspace/klien.
- Availability: uptime 99.5%, retry & queue saat rate limit platform.
- Compliance: patuh ToS & rate limit API platform, GDPR/PDP untuk data publik.

## 9. Data Model (High-Level)
- **Workspace** (nama, owner, plan). Relasi: punya banyak SocialAccount.
- **SocialAccount** (platform, handle, token, workspace_id). Relasi: punya banyak Post & Metric.
- **Post** (account_id, tipe, caption, published_at). Relasi: punya banyak MetricSnapshot & Comment.
- **MetricSnapshot** (post/account_id, tanggal, reach, engagement). Relasi: dipakai tren.
- **Comment** (post_id, teks, sentimen). Relasi: milik satu Post.
- **Competitor** (workspace_id, platform, handle). Relasi: punya MetricSnapshot publik.

## 10. Integrations
- API resmi Instagram/Meta Graph, TikTok, YouTube Data, X, LinkedIn.
- OAuth/Identity untuk otorisasi akun.
- Email/Slack untuk laporan & alert.
- Google Sheets/BigQuery untuk ekspor data (opsional).
- Model NLP/sentimen (internal atau layanan pihak ketiga).

## 11. Success Metrics (KPI)
- Waktu penyusunan laporan turun > 70%.
- Jumlah akun terhubung aktif per workspace.
- Retensi bulanan pengguna > 85%.
- Akurasi klasifikasi sentimen > 80% pada dataset uji.
- Waktu respons terhadap sentimen negatif membaik terukur.

## 12. Risks & Assumptions
- Risiko: perubahan/deprecation API platform -> perlu abstraksi konektor.
- Risiko: rate limit membatasi kesegaran data -> perlu queue & caching.
- Asumsi: pengguna punya akun bisnis/kreator dengan akses API.
- Asumsi: data metrik publik kompetitor tersedia via API resmi.

## 13. Roadmap / Phases
- **Phase 1 (MVP):** koneksi multi-platform, dashboard metrik, sentimen dasar, benchmarking, laporan terjadwal.
- **Phase 2:** social listening, deteksi krisis, white-label, rekomendasi konten AI.
- **Phase 3:** prediksi tren, analitik atribusi ke konversi, integrasi ads spend.
