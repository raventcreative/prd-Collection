# PRD: Content Management System (CMS)

## 1. Ringkasan
CMS adalah platform headless-capable untuk membuat, mengelola, dan mempublikasikan konten web (halaman, artikel, landing page) menggunakan sistem block modular dan media library terpusat. Target penggunanya adalah tim marketing, editor, dan developer yang butuh publish cepat tanpa deploy ulang. Nilai utamanya: time-to-publish yang singkat dengan kontrol workflow dan versioning yang rapi.

## 2. Problem Statement
Tim marketing bergantung pada developer untuk setiap perubahan konten kecil, menyebabkan bottleneck rilis dan biaya engineering yang tinggi. Konten tersebar di banyak channel tanpa single source of truth, media di-upload berulang tanpa organisasi, dan tidak ada jejak audit siapa mengubah apa sebelum tayang.

## 3. Goals & Non-Goals
**Goals:**
- Editor visual berbasis block untuk menyusun halaman tanpa coding.
- Publishing workflow dengan status draft, review, scheduled, published.
- Media library terpusat dengan reuse, tagging, dan transformasi gambar.
- Versioning konten dan rollback ke revisi sebelumnya.
- Delivery via API (headless) dan/atau rendered pages.

**Non-Goals:**
- Bukan page-builder drag-and-drop pixel-perfect setingkat Webflow.
- Tidak menangani e-commerce checkout (lihat PRD E-Commerce).
- Tidak menyediakan CDN/hosting infrastruktur sendiri (mengandalkan pihak ketiga).
- Tidak melakukan A/B testing native pada Phase 1.

## 4. Target Users / Personas
- **Content Editor:** menulis dan menyusun artikel/halaman; butuh editor intuitif dan preview.
- **Marketing Manager:** approve konten sebelum tayang; butuh review workflow dan scheduling.
- **Developer:** konsumsi konten via API; butuh schema stabil dan webhook.
- **Admin:** kelola user, role, dan struktur konten; butuh RBAC dan audit log.

## 5. Key Features
### MVP
- Block editor (heading, text, image, video embed, CTA, gallery, HTML custom).
- Content types konfigurabel (Page, Article, Landing Page).
- Media library upload, tag, search, dan reuse.
- Workflow status: Draft -> In Review -> Scheduled -> Published.
- Versioning + rollback.
- Preview draft via link privat.
- Content Delivery API (REST/JSON) + webhook on publish.

### Future / Phase 2+
- Multi-language / localization (i18n).
- A/B testing dan personalization.
- GraphQL delivery layer.
- Approval chain multi-level dan komentar inline.
- AI-assisted drafting dan alt-text generation.

## 6. User Stories
- Sebagai Content Editor, saya ingin menyusun halaman dari block agar bisa publish tanpa bantuan developer.
- Sebagai Content Editor, saya ingin menyimpan draft dan preview agar bisa cek tampilan sebelum tayang.
- Sebagai Marketing Manager, saya ingin men-schedule publish di tanggal tertentu agar kampanye tayang otomatis.
- Sebagai Marketing Manager, saya ingin approve/reject konten agar kualitas terjaga sebelum live.
- Sebagai Developer, saya ingin mengambil konten via API agar bisa render di frontend apa pun.
- Sebagai Developer, saya ingin menerima webhook saat konten publish agar cache bisa di-invalidate.
- Sebagai Admin, saya ingin rollback ke revisi lama agar bisa memperbaiki kesalahan cepat.
- Sebagai Admin, saya ingin melihat audit log agar tahu siapa mengubah konten dan kapan.

## 7. Functional Requirements
- FR-1: Sistem harus menyediakan block editor dengan minimal 7 tipe block dan reorder via drag.
- FR-2: Sistem harus mendukung content type kustom dengan field bertipe (text, rich text, image, reference, date, boolean).
- FR-3: Sistem harus menyimpan setiap perubahan sebagai revisi dan mengizinkan rollback.
- FR-4: Sistem harus mendukung status workflow dan transisi yang dibatasi oleh role.
- FR-5: Sistem harus mendukung scheduled publish dengan timezone-aware datetime.
- FR-6: Media library harus mendukung upload, tag, search, dan menghasilkan varian ukuran (thumbnail, medium, large).
- FR-7: Sistem harus mengekspos Content Delivery API dengan filter, pagination, dan field selection.
- FR-8: Sistem harus mengirim webhook pada event publish/unpublish.
- FR-9: Sistem harus mencatat audit log (user, aksi, timestamp, entity).
- FR-10: Sistem harus menghasilkan preview URL bertoken untuk draft.

## 8. Non-Functional Requirements
- **Performance:** Delivery API p95 < 200ms dengan caching; media di-serve via CDN.
- **Security:** RBAC, signed preview token, sanitasi HTML custom block terhadap XSS.
- **Scalability:** Mendukung >100k item konten dan >1M media asset.
- **Availability:** 99.9% uptime untuk delivery layer.
- **Compliance:** Audit trail dan retensi revisi; dukungan GDPR untuk PII di komentar/user.

## 9. Data Model (High-Level)
- **Content:** id, type, slug, title, status, locale, current_version_id, published_at, author_id.
- **ContentVersion:** id, content_id, blocks(JSON), created_by, created_at.
- **Block:** type, order, data(JSON) — tersimpan di dalam version.
- **MediaAsset:** id, filename, mime, url, variants(JSON), tags[], uploaded_by, size.
- **User/Role:** id, name, role, permissions[].
- **AuditLog:** id, user_id, action, entity_type, entity_id, timestamp.
- Relasi: Content 1—N ContentVersion; Content N—N MediaAsset (via block reference).

## 10. Integrations
- CDN & object storage (S3-compatible / Cloudflare) untuk media.
- Webhook consumer (Next.js/Vercel, Netlify) untuk ISR/cache invalidation.
- Image transformation service (imgproxy / Cloudinary).
- SSO/Identity provider (OAuth2/SAML) untuk login.
- Analytics (GA4) via embed script pada rendered pages.

## 11. Success Metrics (KPI)
- Median time-to-publish < 10 menit dari draft ke live.
- >80% perubahan konten dilakukan tanpa keterlibatan developer.
- Delivery API p95 latency < 200ms.
- Reuse rate media asset > 40% (mengurangi duplikasi upload).
- < 1% konten publish membutuhkan rollback dalam 24 jam.

## 12. Risks & Assumptions
- **Risiko:** HTML custom block membuka celah XSS jika sanitasi lemah — mitigasi dengan allowlist.
- **Risiko:** Migrasi konten legacy kompleks dan memakan waktu.
- **Asumsi:** Frontend consumer siap mengonsumsi headless API.
- **Asumsi:** Tim marketing bersedia adopsi model block editor.

## 13. Roadmap / Phases
- **Phase 1 (MVP):** Block editor, content types, media library, workflow, versioning, delivery API + webhook.
- **Phase 2:** Localization/i18n, approval multi-level, komentar inline, GraphQL.
- **Phase 3:** A/B testing, personalization, AI-assisted authoring, analytics native.
