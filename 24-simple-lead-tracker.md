# PRD: Simple Lead Tracker - Catat & pantau lead tanpa ribet spreadsheet

**Project:** Simple Lead Tracker
**Aesthetic:** Clean, minimal, "notebook rapi" — banyak whitespace, kartu lembut, aksen satu warna. Terasa ringan seperti Notion/Linear, bukan CRM berat.

## Core Layout
- **Primary view — Board (Kanban):** kolom per stage (New → Contacted → Qualified → Won/Lost). Kartu lead bisa di-drag antar kolom.
- **Secondary view — List/Table:** tabel lead dengan sort & filter, untuk yang lebih suka baris ketimbang kartu.
- **Detail panel (slide-over kanan):** klik lead → panel geser berisi info kontak, aktivitas, catatan, dan next follow-up.
- **Top bar:** search, filter (stage/owner/source), toggle Board/List, tombol `+ New Lead`.

```
+---------------------------------------------------------------+
| Lead Tracker    [search...]  [Board|List]  [Filter v]  [+ New]|
+---------------------------------------------------------------+
|  NEW (4)      | CONTACTED (3) | QUALIFIED (2) | WON (5)        |
|  +---------+  | +---------+   | +---------+   | +---------+    |
|  | Budi    |  | | Sari    |   | | Andi    |   | | Rina    |    |
|  | Rp 5jt  |  | | Rp 12jt |   | | Rp 8jt  |   | | Rp 20jt |    |
|  | ● IG    |  | | ● Web   |   | | ● Ref   |   | | ● WA    |    |
|  +---------+  | +---------+   | +---------+   | +---------+    |
+---------------------------------------------------------------+
```

## Data Structure (Mock Only)
```typescript
type Stage = "new" | "contacted" | "qualified" | "won" | "lost";
type Source = "instagram" | "whatsapp" | "web_form" | "referral" | "other";

interface Lead {
  id: string;
  name: string;
  company?: string;
  phone?: string;
  email?: string;
  value: number;            // estimasi nilai deal (IDR)
  stage: Stage;
  source: Source;
  owner: string;            // nama PIC
  nextFollowUp?: string;    // ISO date
  notes: string;
  createdAt: string;        // ISO date
  activities: Activity[];
}

interface Activity {
  id: string;
  type: "note" | "call" | "message" | "meeting" | "stage_change";
  text: string;
  at: string;               // ISO datetime
}
```

## Implementation Details
- **Stack:** React + Vite + TypeScript, Tailwind CSS.
- **State:** `useState`/`useReducer` + Context. Persist ke `localStorage` (satu key `leads`). Tanpa backend di MVP.
- **Drag-and-drop:** `@dnd-kit/core` (ringan) untuk pindah stage antar kolom.
- **Date:** `Intl.DateTimeFormat` native, tanpa lib tanggal.
- **Icons:** `lucide-react`.
- **ID:** `crypto.randomUUID()`.

## Styling & Theming
- **Palette:** background `#FAFAF9`, kartu `#FFFFFF`, teks `#1C1917`, muted `#78716C`, border `#E7E5E4`, aksen `#4F46E5` (indigo). Stage color: New `#94A3B8`, Contacted `#F59E0B`, Qualified `#3B82F6`, Won `#22C55E`, Lost `#EF4444`.
- **Typography:** Inter. Judul 18–20px semibold, body 14px, meta 12px.
- **Spacing:** grid 8px. Kartu radius 12px, shadow tipis (`0 1px 2px rgba(0,0,0,.06)`). Padding kolom 16px.

## UI Components Specification
**Lead Card**
```
+-----------------------------+
| Budi Santoso           ⋮    |
| PT Maju Jaya                |
| Rp 5.000.000                |
| ● Instagram   👤 Andi       |
| 🔔 Follow-up: 15 Sep        |
+-----------------------------+
```
- Klik kartu → buka detail panel. Ikon `⋮` → menu (edit, pindah stage, hapus).
- Badge follow-up merah jika `nextFollowUp` sudah lewat.

**New/Edit Lead Modal:** field name (wajib), company, phone, email, value, source, owner, stage, next follow-up (`<input type="date">`), notes. Validasi: name tidak boleh kosong; salah satu dari phone/email harus diisi.

**Detail Panel:** header nama + stage dropdown, blok kontak, timeline aktivitas (terbaru di atas), input tambah note cepat.

## Interactions & States
- **Drag kartu antar kolom** → update `stage` + auto-log activity `stage_change`.
- **Tambah lead** → muncul di kolom sesuai stage, animasi fade-in.
- **Follow-up jatuh tempo** → badge merah + count di top bar ("3 lead perlu follow-up").
- **Empty state (belum ada lead):** ilustrasi ringan + teks "Belum ada lead. Tambah lead pertamamu" + tombol `+ New Lead`.
- **Empty kolom:** dashed placeholder "Tarik lead ke sini".
- **Delete:** konfirmasi dulu ("Hapus lead Budi? Tidak bisa dibatalkan.").
- **Search kosong hasil:** "Tidak ada lead cocok dengan pencarian."

## Language & Localization
- Single language: **Bahasa Indonesia**. Format mata uang `Rp` dengan pemisah ribuan (`Intl.NumberFormat('id-ID')`). Format tanggal `id-ID`.

## Example Mock Data
```typescript
const mockLeads: Lead[] = [
  { id: "1", name: "Budi Santoso", company: "PT Maju Jaya", phone: "0812xxxx",
    value: 5000000, stage: "new", source: "instagram", owner: "Andi",
    nextFollowUp: "2026-09-15", notes: "Tanya harga paket A", createdAt: "2026-09-10",
    activities: [{ id: "a1", type: "note", text: "Masuk dari DM IG", at: "2026-09-10T09:00:00" }] },
  { id: "2", name: "Sari Dewi", company: "Toko Sari", email: "sari@mail.com",
    value: 12000000, stage: "contacted", source: "web_form", owner: "Maya",
    nextFollowUp: "2026-09-13", notes: "Minta proposal", createdAt: "2026-09-08",
    activities: [{ id: "a2", type: "call", text: "Telepon perkenalan 10 mnt", at: "2026-09-11T14:00:00" }] },
  { id: "3", name: "Andi Wijaya", value: 8000000, stage: "qualified", source: "referral",
    owner: "Andi", notes: "Referral dari klien lama", createdAt: "2026-09-05", activities: [] },
];
```

## Success Criteria
- ✅ User bisa tambah, edit, hapus lead, dan pindah stage via drag-and-drop.
- ✅ Data bertahan setelah refresh (localStorage).
- ✅ Board & List view menampilkan lead yang sama, bisa di-search & filter.
- ✅ Lead dengan follow-up jatuh tempo ditandai jelas.

## Failed Criteria
- ❌ Butuh setup backend/login sebelum bisa dipakai.
- ❌ Data hilang saat reload halaman.
- ❌ Tidak ada validasi → bisa simpan lead tanpa nama/kontak.
- ❌ UI seberat CRM enterprise (banyak menu, lambat, membingungkan).
