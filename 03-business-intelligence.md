# PRD: Business Intelligence

## 1. Ringkasan
Platform Business Intelligence (BI) memungkinkan tim bisnis menghubungkan berbagai sumber data, membangun dashboard interaktif, dan melakukan self-service analytics tanpa harus menulis SQL kompleks. Target penggunanya analis data, manajer/eksekutif, dan tim operasional. Nilai utamanya: satu sumber kebenaran (single source of truth) untuk pengambilan keputusan berbasis data secara real-time.

## 2. Problem Statement
Data bisnis tersebar di banyak sistem (database, spreadsheet, SaaS) sehingga pelaporan dilakukan manual, lambat, dan tidak konsisten. Tim non-teknis bergantung pada analis untuk setiap pertanyaan data, menciptakan bottleneck. Akibatnya keputusan sering terlambat atau berdasarkan data usang.

## 3. Goals & Non-Goals
**Goals:**
- Koneksi mudah ke banyak sumber data (database, API, file, SaaS).
- Dashboard interaktif dengan filter, drill-down, dan sharing.
- Self-service analytics dengan drag-and-drop untuk user non-teknis.
- Semantic layer/metric definitions agar metrik konsisten lintas tim.

**Non-Goals:**
- Bukan platform ETL/data engineering berat (integrasi dengan tools ETL, bukan menggantikan).
- Bukan data science/ML platform pada MVP.
- Tidak menyediakan data warehouse sendiri (mendukung yang eksisting).

## 4. Target Users / Personas
- **Analis data (Maya):** butuh query, modeling, dan membangun dashboard.
- **Manajer/Eksekutif (Pak Hendra):** butuh dashboard ringkas & alert KPI.
- **Tim operasional (Rio):** butuh filter data sendiri tanpa minta analis.
- **Admin data (Fajar):** butuh atur koneksi, akses, dan governance.

## 5. Key Features
### MVP
- Data connectors: PostgreSQL, MySQL, Google Sheets, CSV upload, REST API.
- Query builder visual + editor SQL untuk power user.
- Dashboard builder drag-and-drop dengan chart library (bar, line, pie, table, KPI card).
- Filter global, drill-down, dan scheduled refresh.
- Sharing & permission per dashboard (view/edit).

### Future / Phase 2+
- Semantic layer (definisi metrik terpusat).
- Alerting berbasis threshold & anomaly detection.
- Natural language query (tanya data dengan bahasa natural).
- Embedded analytics (embed dashboard ke aplikasi lain).
- Row-level security & data lineage.

## 6. User Stories
- Sebagai analis, saya ingin menghubungkan database produksi agar bisa membuat laporan langsung.
- Sebagai manajer, saya ingin dashboard penjualan bulanan agar bisa pantau target.
- Sebagai user operasional, saya ingin memfilter data per cabang agar relevan dengan tim saya.
- Sebagai analis, saya ingin mendefinisikan metrik "revenue" sekali agar konsisten di semua dashboard.
- Sebagai manajer, saya ingin menerima alert saat KPI turun agar bisa bertindak cepat.
- Sebagai admin, saya ingin mengatur siapa bisa akses dashboard apa agar data sensitif terlindungi.
- Sebagai eksekutif, saya ingin export dashboard ke PDF agar bisa dibawa ke rapat.

## 7. Functional Requirements
- FR-1: Sistem harus terhubung ke minimal 5 tipe sumber data (SQL DB, file, Sheets, REST API).
- FR-2: Sistem harus menyediakan query builder visual dan editor SQL.
- FR-3: Sistem harus membangun dashboard multi-chart dengan filter global & drill-down.
- FR-4: Sistem harus mendukung penjadwalan refresh data (scheduled/interval).
- FR-5: Sistem harus mengatur permission per dashboard & per dataset (RBAC).
- FR-6: Sistem harus mendukung caching hasil query untuk performa.
- FR-7: Sistem harus mendukung ekspor dashboard/chart ke PDF/PNG/CSV.
- FR-8: Sistem harus menyimpan definisi metrik yang dapat dipakai ulang lintas dashboard.

## 8. Non-Functional Requirements
- Performance: render dashboard < 3 detik dengan caching; query dataset besar < 10 detik.
- Scalability: mendukung ratusan concurrent viewer & dataset jutaan baris.
- Security: RBAC, row-level security, kredensial koneksi terenkripsi, SSO.
- Availability: uptime 99.9%.
- Compliance: audit log akses data, UU PDP, opsi data residency.

## 9. Data Model (High-Level)
- **DataSource** (id, type, connection_config_encrypted, status).
- **Dataset** (id, source_id, query, schema, refresh_schedule).
- **Dashboard** (id, name, owner_id) 1—N **Widget** (id, dataset_id, chart_type, config).
- **Metric** (id, name, expression, dataset_id).
- **Permission** (id, resource_id, user_or_role, level). Relasi: DataSource 1—N Dataset 1—N Widget; Dashboard 1—N Widget.

## 10. Integrations
- Database: PostgreSQL, MySQL, SQL Server, BigQuery, Snowflake.
- SaaS: Google Sheets, Google Analytics, HubSpot, Stripe.
- Auth: SSO (Google, SAML/OAuth), Slack/email untuk alert & sharing.
- Embedding: iframe/SDK untuk embed ke aplikasi pihak ketiga.

## 11. Success Metrics (KPI)
- >= 60% dashboard dibuat oleh user non-teknis (self-service adoption).
- Waktu pembuatan laporan turun >= 60%.
- Weekly active users dashboard >= 70% dari lisensi.
- p95 load time dashboard < 3 detik.
- Jumlah sumber data terhubung per akun (rata-rata) >= 3.

## 12. Risks & Assumptions
- Risiko performa pada dataset sangat besar -> perlu strategi caching & aggregation.
- Risiko "dashboard sprawl" -> perlu governance & semantic layer.
- Ketergantungan pada kualitas & konsistensi data sumber.
- Asumsi: sumber data dapat diakses via koneksi jaringan yang aman.

## 13. Roadmap / Phases
- **Phase 1 (MVP):** Connectors inti, query builder, dashboard builder, sharing, scheduled refresh.
- **Phase 2:** Semantic layer, alerting, row-level security, embedded analytics.
- **Phase 3:** Natural language query, anomaly detection, data lineage & governance lanjutan.
