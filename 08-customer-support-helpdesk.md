# PRD: Customer Support Help Desk

## 1. Ringkasan
Help Desk adalah platform omnichannel untuk mengelola tiket dukungan pelanggan dari email, chat, WhatsApp, dan web form dalam satu antrian terpadu, dilengkapi SLA tracking dan knowledge base self-service. Target penggunanya adalah tim support, agent, dan supervisor di perusahaan B2B/B2C. Nilai utamanya: waktu respons lebih cepat, beban agent berkurang lewat deflection, dan visibilitas SLA yang jelas.

## 2. Problem Statement
Permintaan pelanggan masuk dari banyak channel terpisah (email, WA, DM) sehingga sulit dilacak, sering terlewat, dan tidak ada akuntabilitas SLA. Agent menjawab pertanyaan berulang yang sama tanpa knowledge base, sementara supervisor tidak punya data beban kerja dan performa tim secara real-time.

## 3. Goals & Non-Goals
**Goals:**
- Menyatukan semua channel ke satu inbox tiket.
- SLA policy dengan timer, eskalasi, dan alert breach.
- Knowledge base untuk self-service dan bantuan agent.
- Routing dan assignment tiket otomatis.
- Reporting performa agent dan kepuasan (CSAT).

**Non-Goals:**
- Bukan CRM penjualan lengkap (fokus support, bukan pipeline sales).
- Tidak menyediakan call-center telephony/PBX pada Phase 1.
- Tidak menangani billing/invoicing pelanggan.
- Tidak membangun live-chat widget desain kustom penuh (embed standar saja).

## 4. Target Users / Personas
- **Support Agent:** menangani tiket harian; butuh inbox terpadu dan canned response.
- **Support Supervisor:** monitor SLA dan beban tim; butuh dashboard dan reassignment.
- **Pelanggan:** cari solusi cepat; butuh knowledge base dan status tiket.
- **Admin:** konfigurasi channel, SLA, dan automasi; butuh setting dan integrasi.

## 5. Key Features
### MVP
- Omnichannel inbox (email, web form, chat widget, WhatsApp).
- Tiket dengan status, prioritas, tag, dan assignee.
- SLA policy: first response time, resolution time, timer, breach alert.
- Auto-routing berdasarkan rule (channel, keyword, load balancing).
- Knowledge base publik + article suggestion ke agent.
- Canned responses / macros.
- CSAT survey pasca-resolusi.

### Future / Phase 2+
- AI auto-reply dan intent classification.
- Chatbot deflection sebelum masuk agent.
- Voice/telephony integration.
- Community forum.
- Advanced analytics & forecasting staffing.

## 6. User Stories
- Sebagai Agent, saya ingin melihat semua tiket dari semua channel di satu inbox agar tidak ada yang terlewat.
- Sebagai Agent, saya ingin memakai canned response agar menjawab pertanyaan umum lebih cepat.
- Sebagai Agent, saya ingin melihat artikel KB terkait tiket agar bisa menjawab akurat.
- Sebagai Supervisor, saya ingin melihat tiket yang mendekati SLA breach agar bisa eskalasi tepat waktu.
- Sebagai Supervisor, saya ingin mereassign tiket agar beban agent seimbang.
- Sebagai Pelanggan, saya ingin mencari solusi di knowledge base agar tidak perlu menunggu agent.
- Sebagai Pelanggan, saya ingin melihat status tiket saya agar tahu progres penyelesaian.
- Sebagai Admin, saya ingin mengatur SLA policy per prioritas agar sesuai komitmen layanan.

## 7. Functional Requirements
- FR-1: Sistem harus menerima dan mengubah pesan dari email, web form, chat, dan WhatsApp menjadi tiket.
- FR-2: Sistem harus mendukung threading balasan pada tiket yang sama lintas channel.
- FR-3: Sistem harus menerapkan SLA policy dengan timer first-response dan resolution per prioritas.
- FR-4: Sistem harus mengirim alert saat SLA mendekati/melewati breach dan mengeskalasi otomatis.
- FR-5: Sistem harus melakukan auto-routing tiket berdasarkan rule yang dikonfigurasi.
- FR-6: Sistem harus menyediakan canned responses/macros yang dapat disisipkan agent.
- FR-7: Sistem harus menyediakan knowledge base publik dengan search dan kategori.
- FR-8: Sistem harus menyarankan artikel KB relevan ke agent berdasarkan isi tiket.
- FR-9: Sistem harus mengirim survey CSAT setelah tiket resolved.
- FR-10: Sistem harus menyediakan dashboard metrik (volume, response time, resolution, CSAT).

## 8. Non-Functional Requirements
- **Performance:** Inbox load p95 < 1s; pesan channel masuk ke tiket < 30 detik.
- **Security:** RBAC, enkripsi data in-transit/at-rest, redaksi PII, masking data kartu.
- **Scalability:** Mendukung >500 agent concurrent dan >1M tiket/tahun.
- **Availability:** 99.9% uptime; queue durable agar pesan tidak hilang saat downtime.
- **Compliance:** GDPR/PDP (hak hapus data pelanggan), audit log akses tiket.

## 9. Data Model (High-Level)
- **Ticket:** id, subject, status, priority, channel, requester_id, assignee_id, sla_due_at, created_at, resolved_at, tags[].
- **Message:** id, ticket_id, author_type(agent/customer), body, attachments[], channel, created_at.
- **Customer:** id, name, email, phone, org_id.
- **Agent:** id, name, team_id, availability_status.
- **SLAPolicy:** id, priority, first_response_target, resolution_target, business_hours.
- **KBArticle:** id, title, body, category, tags[], views, helpful_count.
- **CSAT:** id, ticket_id, score, comment.
- Relasi: Ticket 1—N Message; Customer 1—N Ticket; Agent 1—N Ticket.

## 10. Integrations
- Email (IMAP/SMTP, Gmail/Outlook API).
- WhatsApp Business API / Meta Cloud API.
- Chat widget embed (JS SDK).
- Slack/Teams notifikasi internal.
- CRM (Salesforce/HubSpot) sinkron data pelanggan.
- Webhook & REST API untuk integrasi kustom.

## 11. Success Metrics (KPI)
- Median first response time < 1 jam (business hours).
- SLA compliance rate > 95%.
- Self-service deflection rate (KB solve tanpa tiket) > 25%.
- CSAT score rata-rata > 4.5/5.
- Reopen rate tiket < 5%.

## 12. Risks & Assumptions
- **Risiko:** WhatsApp API rate limit/policy berubah — mitigasi abstraksi channel adapter.
- **Risiko:** KB kurang terawat sehingga deflection rendah — perlu owner konten.
- **Asumsi:** Pelanggan bersedia menggunakan self-service.
- **Asumsi:** Tim support disiplin mengisi tag/kategori untuk reporting akurat.

## 13. Roadmap / Phases
- **Phase 1 (MVP):** Omnichannel inbox, tiket + SLA, routing, KB, canned response, CSAT, dashboard.
- **Phase 2:** AI intent classification, chatbot deflection, advanced analytics.
- **Phase 3:** Telephony/voice, community forum, staffing forecasting.
