# PRD: Project Management & Collaboration

## 1. Ringkasan
Platform manajemen proyek dan kolaborasi tim yang menyatukan task, board, timeline, dan komunikasi dalam satu tempat. Ditujukan untuk tim produk, agency, dan startup 5-200 orang yang butuh visibilitas progres kerja tanpa berpindah-pindah tool. Nilai utamanya: satu sumber kebenaran untuk siapa mengerjakan apa, kapan deadline-nya, dan di mana progress-nya sekarang.

## 2. Problem Statement
Tim tersebar pekerjaannya di spreadsheet, chat, dan email sehingga status pekerjaan sulit dilacak, deadline terlewat, dan konteks diskusi hilang. Manajer menghabiskan waktu untuk menanyakan progress manual (status meeting) alih-alih melihatnya real-time, dan handoff antar anggota rawan miskomunikasi.

## 3. Goals & Non-Goals
**Goals:**
- Menyediakan task management dengan board (Kanban), list, dan timeline (Gantt) view yang sinkron.
- Memberi visibilitas progress real-time lintas proyek dan tim.
- Mengurangi koordinasi manual lewat komentar, mention, dan notifikasi terkontekstual.
- Mendukung kolaborasi async (komentar, attachment, activity log).

**Non-Goals:**
- Bukan tool akuntansi/invoicing atau payroll (walau bisa integrasi).
- Bukan video conferencing (integrasi ke Zoom/Meet, tidak dibangun sendiri).
- Bukan document editor kaya seperti Notion/Google Docs (dukung attachment & rich text ringan saja).
- Tidak menangani source-code hosting (integrasi ke GitHub/GitLab saja).

## 4. Target Users / Personas
- **Project Manager (Rina):** butuh melihat status semua proyek, mengatur deadline, dan mengidentifikasi bottleneck.
- **Team Member / IC (Dedi):** butuh daftar task yang jelas hari ini, tempat update progress, dan diskusi konteks.
- **Team Lead (Sari):** butuh mengatur workload tim, assign task, dan review sebelum done.
- **Stakeholder/Client (Bapak Anton):** butuh view read-only progress tanpa masuk ke detail teknis.

## 5. Key Features
### MVP
- Workspace, project, dan task hierarki (task + subtask).
- Board view (Kanban) drag-and-drop dengan custom status/column.
- List view dengan sort, filter, dan grouping.
- Timeline/Gantt view dengan dependency antar task.
- Assignee, due date, priority, label, dan attachment per task.
- Komentar dengan @mention dan activity log per task.
- Notifikasi in-app dan email untuk assignment, mention, dan due date.

### Future / Phase 2+
- Workload/capacity view per anggota.
- Automation rules (mis. pindah kolom otomatis saat status berubah).
- Custom fields dan template proyek.
- Dashboard & reporting (burndown, velocity).
- Time tracking dan integrasi kalender dua arah.
- Guest/client portal dengan izin granular.

## 6. User Stories
- Sebagai PM, saya ingin membuat proyek dan mengundang anggota agar tim punya ruang kerja bersama.
- Sebagai team member, saya ingin melihat semua task yang di-assign ke saya lintas proyek agar tahu prioritas hari ini.
- Sebagai team lead, saya ingin drag task antar kolom board agar status pekerjaan selalu ter-update.
- Sebagai PM, saya ingin menetapkan dependency antar task di timeline agar urutan pengerjaan jelas.
- Sebagai anggota, saya ingin mention rekan di komentar agar diskusi terjadi pada konteks task-nya.
- Sebagai team lead, saya ingin difilter task berdasarkan assignee dan due date agar bisa cek beban kerja.
- Sebagai stakeholder, saya ingin melihat progress proyek read-only agar tidak perlu tanya manual.
- Sebagai anggota, saya ingin menerima notifikasi saat di-assign agar tidak melewatkan pekerjaan.

## 7. Functional Requirements
- FR-1: Sistem harus mendukung hierarki Workspace > Project > Task > Subtask.
- FR-2: Sistem harus menyediakan minimal 3 view sinkron: Board, List, Timeline.
- FR-3: Task harus punya field: title, description, assignee(s), due date, priority, status, label, attachment.
- FR-4: Board harus mendukung drag-and-drop dan custom column/status.
- FR-5: Sistem harus mendukung dependency antar task (finish-to-start minimal).
- FR-6: Sistem harus menyediakan komentar dengan @mention dan activity log immutable.
- FR-7: Sistem harus mengirim notifikasi in-app dan email untuk assignment, mention, due date reminder.
- FR-8: Sistem harus mendukung filter, sort, dan grouping pada List dan Board view.
- FR-9: Sistem harus mendukung role-based access (Admin, Member, Guest) per workspace/project.
- FR-10: Perubahan task harus tersimpan real-time dan tampak ke anggota lain tanpa refresh.

## 8. Non-Functional Requirements
- **Performance:** Board dengan 500 task harus render < 2 detik; update real-time latency < 1 detik.
- **Scalability:** Mendukung 10.000 concurrent user per region.
- **Availability:** SLA 99.9% uptime.
- **Security:** Enkripsi in-transit (TLS 1.2+) dan at-rest; RBAC; audit log.
- **Compliance:** GDPR-ready (data export & deletion), SOC 2 target Phase 3.
- **Usability:** Mobile-responsive web + keyboard shortcut untuk power user.

## 9. Data Model (High-Level)
- **User** (id, name, email, avatar, role).
- **Workspace** (id, name, owner_id) — punya banyak Project.
- **Project** (id, workspace_id, name, status, start_date, end_date) — punya banyak Task.
- **Task** (id, project_id, parent_task_id, title, description, status, priority, due_date) — punya banyak Comment, Attachment.
- **TaskAssignee** (task_id, user_id) — many-to-many.
- **Dependency** (predecessor_task_id, successor_task_id, type).
- **Comment** (id, task_id, author_id, body, created_at) — mendukung mentions.
- **ActivityLog** (id, entity_type, entity_id, actor_id, action, timestamp).

## 10. Integrations
- Slack / Microsoft Teams (notifikasi & create task dari chat).
- Google Calendar / Outlook (sync due date & milestone).
- GitHub / GitLab (link commit/PR ke task).
- Google Drive / Dropbox (attachment).
- Zapier / webhook publik untuk automation kustom.
- SSO via Google, Microsoft, SAML.

## 11. Success Metrics (KPI)
- Weekly Active Users / Monthly Active Users (target WAU/MAU > 60%).
- Rata-rata task dibuat per user aktif per minggu.
- % task selesai tepat waktu (on-time completion rate).
- Time-to-first-value: waktu dari signup ke pembuatan proyek pertama < 10 menit.
- Retensi tim (team retention) 3 bulan > 70%.
- Pengurangan status meeting yang dilaporkan tim (survei).

## 12. Risks & Assumptions
- **Risiko:** Kompetisi ketat (Asana, Jira, Trello) — perlu diferensiasi jelas (kesederhanaan + real-time).
- **Risiko:** Real-time sync kompleks pada skala besar dan rawan konflik edit.
- **Risiko:** Adopsi rendah jika onboarding rumit.
- **Asumsi:** Tim target sudah familiar dengan konsep Kanban/board.
- **Asumsi:** Mayoritas akses via web/desktop; mobile pendukung.

## 13. Roadmap / Phases
- **Phase 1 (MVP):** Workspace/project/task, Board + List + Timeline, komentar, notifikasi, RBAC dasar.
- **Phase 2:** Automation rules, custom fields, template, dashboard & reporting, time tracking.
- **Phase 3:** Workload/capacity, client portal, integrasi lanjutan, mobile app native, SOC 2.
