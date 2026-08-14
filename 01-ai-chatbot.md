# PRD: AI Chatbot

## 1. Ringkasan
AI Chatbot adalah platform conversational AI berbasis LLM yang melayani pelanggan dan tim internal secara otomatis di banyak channel (web widget, WhatsApp, Instagram, Telegram, email). Target penggunanya adalah tim customer support dan sales UMKM hingga enterprise yang ingin menurunkan beban tiket manual. Nilai utamanya: jawaban akurat 24/7 dengan grounding ke knowledge base perusahaan (RAG) dan handoff mulus ke agen manusia.

## 2. Problem Statement
Tim support kewalahan menjawab pertanyaan berulang di banyak channel dengan SLA yang sulit dijaga, terutama di luar jam kerja. Chatbot rule-based lama kaku, sering salah paham intent, dan tidak bisa menjawab pertanyaan spesifik produk. Akibatnya biaya support tinggi, response time lambat, dan pengalaman pelanggan menurun.

## 3. Goals & Non-Goals
**Goals:**
- Menjawab >70% pertanyaan umum tanpa intervensi manusia (deflection rate).
- Grounding jawaban ke knowledge base perusahaan (RAG) untuk mengurangi halusinasi.
- Multi-channel dari satu inbox terpadu dengan handoff ke agen manusia.
- Analitik percakapan untuk mengukur kualitas dan menemukan gap knowledge.

**Non-Goals:**
- Bukan platform voice call / IVR pada MVP (fokus text/chat).
- Tidak melatih LLM foundation model dari nol (memakai API model komersial/open-source).
- Bukan full CRM/ticketing (integrasi, bukan menggantikan).

## 4. Target Users / Personas
- **Agent Support (Rina):** butuh takeover percakapan cepat lengkap dengan konteks & histori.
- **Admin/CS Lead (Budi):** butuh atur knowledge base, prompt, dan pantau SLA/kualitas.
- **Pelanggan akhir (Sari):** butuh jawaban cepat & akurat di channel favoritnya.
- **Developer (Andi):** butuh API/webhook untuk integrasi ke sistem internal.

## 5. Key Features
### MVP
- Web chat widget yang embeddable + koneksi WhatsApp Business API.
- RAG dari dokumen (PDF, URL, FAQ) dengan citation sumber.
- Intent detection & fallback, escalation/handoff ke agen manusia.
- Unified inbox untuk agen (assign, tag, catatan internal).
- Dashboard metrik dasar (volume, deflection, CSAT).

### Future / Phase 2+
- Multi-bahasa otomatis & language detection.
- Voice channel & speech-to-text.
- Proactive/outbound messaging & campaign.
- Agent assist (draft jawaban untuk agen manusia) & tool/function calling ke API eksternal (cek order, refund).

## 6. User Stories
- Sebagai pelanggan, saya ingin bertanya status pesanan di WhatsApp agar dapat jawaban tanpa menunggu agen.
- Sebagai agent support, saya ingin mengambil alih percakapan bot agar bisa menangani kasus kompleks.
- Sebagai CS lead, saya ingin mengunggah dokumen FAQ agar bot menjawab sesuai kebijakan terbaru.
- Sebagai CS lead, saya ingin melihat pertanyaan yang gagal dijawab agar bisa menutup gap knowledge.
- Sebagai pelanggan, saya ingin bot menyertakan sumber jawabannya agar saya percaya informasinya.
- Sebagai developer, saya ingin webhook saat handoff agar sistem tiket internal ikut terupdate.
- Sebagai agent, saya ingin melihat histori percakapan lintas channel agar konteksnya utuh.

## 7. Functional Requirements
- FR-1: Sistem harus menerima & mengirim pesan via web widget dan WhatsApp Business API.
- FR-2: Sistem harus mengindeks dokumen (PDF/URL/CSV) ke vector store dan melakukan retrieval saat menjawab.
- FR-3: Setiap jawaban RAG harus menyertakan referensi sumber (citation).
- FR-4: Sistem harus mendeteksi intent low-confidence dan memicu escalation ke antrian agen.
- FR-5: Agen harus dapat takeover, membalas, menandai (tag), dan menutup percakapan.
- FR-6: Sistem harus menyimpan histori percakapan lengkap per kontak lintas channel.
- FR-7: Admin harus dapat mengedit system prompt, persona, dan guardrail per bot.
- FR-8: Sistem harus menyediakan webhook event (message.created, handoff.requested, conversation.closed).

## 8. Non-Functional Requirements
- Performance: p95 latensi respons bot < 3 detik (streaming token untuk perceived speed).
- Scalability: mendukung minimal 1.000 percakapan konkuren.
- Security: enkripsi in-transit/at-rest, PII masking pada log, RBAC per role.
- Availability: uptime 99.9%, graceful fallback bila LLM provider down.
- Compliance: kepatuhan UU PDP (Indonesia) & retensi data yang dapat dikonfigurasi.

## 9. Data Model (High-Level)
- **Conversation** (id, channel, contact_id, status, assigned_agent_id, created_at).
- **Message** (id, conversation_id, sender_type, content, tokens, citations, created_at).
- **Contact** (id, name, phone, email, channel_ids, metadata).
- **KnowledgeDoc** (id, title, source_type, url, embedding_status).
- **Agent** (id, name, role, availability). Relasi: Contact 1—N Conversation 1—N Message; KnowledgeDoc N—N Conversation (via retrieval).

## 10. Integrations
- LLM provider (Anthropic Claude / OpenAI / model open-source via API).
- WhatsApp Business API, Instagram/Messenger, Telegram, email (SMTP/IMAP).
- Vector database (pgvector/Pinecone/Qdrant).
- CRM & ticketing (webhook/REST), Slack notification untuk eskalasi.

## 11. Success Metrics (KPI)
- Deflection rate >= 70% dalam 3 bulan.
- Median first response time < 5 detik.
- CSAT bot >= 4.2/5.
- Hallucination/complaint rate < 2% percakapan.
- Pengurangan biaya support per tiket >= 30%.

## 12. Risks & Assumptions
- Risiko halusinasi LLM pada pertanyaan di luar knowledge base -> mitigasi guardrail + fallback ke agen.
- Ketergantungan pada uptime & pricing LLM provider pihak ketiga.
- Kualitas jawaban bergantung pada kualitas knowledge base.
- Asumsi: pelanggan bersedia berinteraksi via chat dan channel utama adalah WhatsApp.

## 13. Roadmap / Phases
- **Phase 1 (MVP):** Web widget + WhatsApp, RAG dengan citation, unified inbox, handoff, dashboard dasar.
- **Phase 2:** Multi-bahasa, agent assist, function calling ke API eksternal, analitik lanjutan.
- **Phase 3:** Voice channel, outbound campaign, auto-improvement dari feedback loop.
