# PRD: Hotel Landing Page - Direct Booking & Hotel Operations

**Project:** Hotel Website, Booking Request & Laravel Admin CMS  
**Aesthetic:** Warm Hospitality, Premium, Trustworthy, Visual, Mobile-First.

> **Product assumption:** MVP ditujukan untuk satu properti hotel. Sistem menerima booking request langsung, menghitung estimasi harga, memeriksa inventori berdasarkan tipe kamar, dan dikelola manual oleh admin. Payment gateway, sinkronisasi OTA/channel manager, multi-property, dan dynamic pricing kompleks ditempatkan pada fase lanjutan agar biaya awal tetap rendah.

## Core Layout

### 1. User & Role

- **Visitor/Guest:** Melihat hotel, kamar, fasilitas, promo, lokasi, dan mengirim booking request.
- **Reservation Admin:** Melihat serta memperbarui status booking, blok inventori, dan data tamu.
- **Content Admin:** Mengelola konten hotel, tipe kamar, fasilitas, galeri, promo, FAQ, SEO, dan site settings.
- **Super Admin:** Mengelola akun admin dan seluruh konfigurasi. Pada MVP, role dapat digabung bila hanya ada satu operator.

### 2. Public Website

- **Sticky Header:** Logo, Rooms, Facilities, Gallery, Offers, Location, Contact, language selector opsional, dan tombol Book Now.
- **Hero:** Foto/video properti, positioning, lokasi singkat, trust indicator, dan booking search bar.
- **Booking Search Bar:** Check-in, check-out, jumlah kamar, dewasa, anak, promo code opsional.
- **Hotel Overview:** Deskripsi, rating/review summary opsional, keunggulan, dan quick facts.
- **Featured Rooms:** Foto, tipe kamar, kapasitas, bed, ukuran, fasilitas, harga mulai, dan CTA detail/book.
- **Facilities:** Restaurant, pool, Wi-Fi, parking, meeting room, spa, atau fasilitas relevan.
- **Gallery:** Kategori kamar, property, facilities, dining, event; lightbox dan lazy loading.
- **Special Offers:** Promo aktif dengan periode, syarat, kode promo, dan CTA.
- **Experience/Attractions:** Destinasi sekitar dan jarak/estimasi waktu.
- **Testimonials:** Review pilihan, nama/initial, asal platform opsional, rating.
- **Location:** Alamat, map embed, directions, nearby landmarks, airport/station information.
- **FAQ:** Check-in/out, cancellation, children, extra bed, pet, parking, breakfast, payment.
- **Contact & Footer:** Telepon, WhatsApp, email, address, social links, policies, dan legal links.
- **Mobile Sticky Book CTA:** Tampil pada mobile tanpa menutup control penting.

### 3. Room Detail Page

- Room title, gallery, short description, room size, occupancy, bed type, view, amenities, inclusions, policies, dan starting price.
- Date and guest selector untuk mengecek availability.
- Price summary dan promo badge bila berlaku.
- Related room types dan CTA WhatsApp sebagai fallback.
- Status sold-out/unavailable harus jelas tanpa menyembunyikan alternatif kamar.

### 4. MVP Booking Flow

1. Guest memilih check-in, check-out, jumlah kamar, dewasa, dan anak.
2. Sistem memvalidasi tanggal dan mencari tipe kamar dengan inventori tersedia.
3. Guest memilih tipe kamar dan jumlah unit.
4. Sistem menampilkan ringkasan malam, rate, diskon, pajak/service charge bila berlaku, dan estimasi total.
5. Guest mengisi nama, email, WhatsApp, special request, dan consent.
6. Sistem melakukan availability check ulang saat submit untuk menghindari stale inventory.
7. Booking dibuat dengan status `pending`, reference number unik, dan snapshot harga.
8. Guest melihat confirmation page dan menerima email bila SMTP tersedia.
9. Admin menerima notifikasi dan menghubungi guest untuk pembayaran/konfirmasi.
10. Admin mengubah status menjadi `confirmed`, `cancelled`, `checked_in`, `completed`, atau `no_show`.

> Booking `pending` memiliki hold duration yang dapat diatur, misalnya 60 menit atau sesuai kebijakan hotel. Hold yang kedaluwarsa otomatis menjadi `expired` agar inventori kembali tersedia.

### 5. Public Supporting Pages

- **Rooms List & Detail**
- **Facilities**
- **Gallery**
- **Offers & Offer Detail**
- **About Hotel**
- **Location & Attractions**
- **Contact**
- **Booking Search, Guest Details, Confirmation**
- **Booking Lookup:** Reference number + email/WhatsApp, hanya menampilkan data minimum.
- **Policies:** Privacy, Terms, Cancellation, Check-in Policy.

### 6. Admin CMS & Reservation Dashboard

- **Dashboard:** Arrival today, departure today, pending booking, confirmed booking, occupancy estimate, dan revenue estimate.
- **Bookings:** Search/filter, detail, status update, notes, resend confirmation, export CSV.
- **Calendar:** Tampilan jumlah sold/available per room type dan manual inventory block.
- **Room Types:** CRUD, images, description, capacity, bed, size, amenities, base rate, total units, status.
- **Rates:** Base rate, weekend override, date-range override, minimum stay opsional.
- **Inventory Blocks:** Block tipe kamar untuk maintenance, group booking, atau manual closure.
- **Promotions:** Code/non-code promo, fixed/percentage discount, validity, usage rules, status.
- **Facilities & Amenities:** CRUD dan urutan tampil.
- **Gallery:** Upload, category, alt text, featured, sort order.
- **Testimonials, Attractions & FAQ:** CRUD dan publish control.
- **Hotel Settings:** Name, address, contact, check-in/out time, tax, service charge, currency, cancellation text.
- **SEO & Analytics:** Global/per-page metadata, social preview, analytics ID.
- **Users & Roles:** Account, role, password reset; minimal pada MVP.
- **Audit Log (minimum):** Mencatat siapa mengubah booking status, rate, inventory, dan promo.

### 7. Navigation Structure

```text
Public
├── Home
├── Rooms
│   └── Room Detail
├── Facilities
├── Gallery
├── Offers
│   └── Offer Detail
├── About
├── Location
├── Contact
├── Booking
│   ├── Search
│   ├── Guest Details
│   ├── Confirmation
│   └── Lookup
├── Privacy Policy
├── Terms & Conditions
└── Cancellation Policy

Admin
├── Dashboard
├── Bookings
├── Availability Calendar
├── Room Types
├── Rates
├── Inventory Blocks
├── Promotions
├── Facilities & Amenities
├── Gallery
├── Testimonials
├── Attractions
├── FAQ
├── Hotel, SEO & Analytics Settings
├── Audit Log
└── Users & Account
```

## Data Structure (Mock Only)

> Interface berikut adalah kontrak konseptual untuk UI dan backend. Implementasi menggunakan Eloquent Models, database constraints, enum/cast, dan transaction-safe booking service.

```typescript
type PublishStatus = 'draft' | 'published' | 'archived';
type BookingStatus =
  | 'pending'
  | 'confirmed'
  | 'cancelled'
  | 'expired'
  | 'checked_in'
  | 'completed'
  | 'no_show';

interface HotelSetting {
  id: number;
  name: string;
  slug: string;
  description: string;
  email: string;
  phone: string;
  whatsapp: string;
  address: string;
  latitude?: number;
  longitude?: number;
  checkInTime: string;
  checkOutTime: string;
  currency: 'IDR';
  taxPercent: number;
  serviceChargePercent: number;
}

interface RoomType {
  id: number;
  name: string;
  slug: string;
  shortDescription: string;
  description: string;
  sizeSqm?: number;
  bedType: string;
  maxAdults: number;
  maxChildren: number;
  totalUnits: number;
  baseRate: number;
  coverPath?: string;
  status: PublishStatus;
}

interface RatePlan {
  id: number;
  roomTypeId: number;
  name: string;
  mealPlan: 'room_only' | 'breakfast_included';
  cancellationPolicy: string;
  baseRate: number;
  isRefundable: boolean;
  isActive: boolean;
}

interface RateOverride {
  id: number;
  ratePlanId: number;
  startDate: string;
  endDate: string;
  nightlyRate: number;
  minimumStay?: number;
}

interface InventoryBlock {
  id: number;
  roomTypeId: number;
  startDate: string;
  endDate: string;
  quantity: number;
  reason?: string;
}

interface Promotion {
  id: number;
  name: string;
  code?: string;
  discountType: 'percentage' | 'fixed';
  discountValue: number;
  startDate: string;
  endDate: string;
  minimumNights?: number;
  maxUses?: number;
  isActive: boolean;
}

interface Booking {
  id: number;
  reference: string;
  guestName: string;
  guestEmail: string;
  guestWhatsapp: string;
  checkInDate: string;
  checkOutDate: string;
  adults: number;
  children: number;
  status: BookingStatus;
  subtotal: number;
  discountTotal: number;
  taxTotal: number;
  serviceChargeTotal: number;
  grandTotal: number;
  currency: 'IDR';
  specialRequest?: string;
  expiresAt?: string;
  confirmedAt?: string;
  createdAt: string;
}

interface BookingItem {
  id: number;
  bookingId: number;
  roomTypeId: number;
  ratePlanId: number;
  quantity: number;
  nights: number;
  nightlyRateSnapshot: number;
  roomNameSnapshot: string;
  lineTotal: number;
}

interface AvailabilityResult {
  roomTypeId: number;
  date: string;
  totalUnits: number;
  confirmedUnits: number;
  activeHoldUnits: number;
  blockedUnits: number;
  availableUnits: number;
}
```

### Laravel Database Entities

| Table | Purpose | Key Fields |
| --- | --- | --- |
| `users` | Akun dan role admin | name, email, password, role |
| `hotel_settings` | Identitas dan kebijakan hotel | contact, address, time, tax, currency |
| `room_types` | Produk tipe kamar | capacity, total_units, base_rate, status |
| `room_images` | Galeri per tipe kamar | room_type_id, image_path, alt_text, sort_order |
| `amenities` | Master fasilitas kamar | name, icon, category |
| `amenity_room_type` | Relasi amenity dan kamar | room_type_id, amenity_id |
| `rate_plans` | Paket harga | meal_plan, refundability, policy, base_rate |
| `rate_overrides` | Harga per periode/tanggal | date range, nightly_rate, minimum_stay |
| `inventory_blocks` | Kamar yang tidak dijual | date range, quantity, reason |
| `promotions` | Aturan promo | code, type, value, validity, limits |
| `bookings` | Header reservasi dan guest | reference, dates, status, totals, expiry |
| `booking_items` | Snapshot kamar/rate yang dipesan | room_type, rate_plan, quantity, rate snapshot |
| `booking_status_logs` | Riwayat perubahan status | booking_id, from, to, user_id, note |
| `facilities` | Fasilitas hotel | name, description, icon, sort_order |
| `gallery_items` | Galeri umum | category, media, alt_text, sort_order |
| `offers` | Promo editorial publik | title, slug, content, validity, status |
| `testimonials` | Review pilihan | guest, quote, rating, source |
| `attractions` | Lokasi sekitar | name, distance, travel_time, map_url |
| `faqs` | Pertanyaan umum | question, answer, category, sort_order |
| `audit_logs` | Audit aktivitas sensitif | user, action, entity, before/after metadata |

### Availability Rule

```text
available_units_per_date =
  room_type.total_units
  - confirmed_or_checked_in_booking_units
  - active_pending_hold_units
  - inventory_block_units
```

- Availability booking harus memenuhi jumlah unit pada **setiap tanggal malam** dari check-in sampai satu hari sebelum check-out.
- Booking cancelled, expired, completed, dan no-show tidak mengurangi future inventory.
- Harga dan room name disimpan sebagai snapshot di `booking_items` agar perubahan rate berikutnya tidak mengubah booking lama.

## Implementation Details

### 1. Recommended Stack

- **Backend & Server Rendering:** Laravel 13 atau versi stabil aktif saat kickoff.
- **Runtime:** PHP 8.3+ sesuai requirement Laravel yang dipakai.
- **Frontend:** Blade, Tailwind CSS, Alpine.js; Livewire dapat dipakai untuk availability calendar dan admin interactions.
- **Database:** MySQL atau MariaDB dengan index pada date, room type, status, reference, dan guest contact.
- **Authentication:** Laravel auth stack; public registration disabled.
- **Storage:** Local public disk untuk MVP; S3-compatible menjadi opsi bila galeri besar.
- **Email:** SMTP untuk guest confirmation, admin notification, cancellation/update, dan password reset.
- **Build Tool:** Vite.
- **Queue/Cache:** Database driver pada MVP; Redis hanya bila beban naik.
- **Scheduler:** Cron setiap menit untuk expiry hold, queue processing, dan scheduled publication.
- **Maps:** Google Maps embed/link tanpa API berbayar untuk MVP.

### 2. Cost-Control Architecture

- Satu monolithic Laravel application menangani website, CMS, dan booking admin.
- Tidak ada React/Vue SPA terpisah dan tidak ada mobile app pada MVP.
- Booking menggunakan manual confirmation; payment gateway tidak menjadi dependency go-live.
- Availability dihitung per room type, bukan assignment nomor kamar individual.
- Database queue/cache digunakan sehingga deployment awal tidak wajib Redis atau queue server terpisah.
- WhatsApp menggunakan click-to-chat; pesan otomatis via WhatsApp API tidak termasuk MVP.
- Galeri dibatasi dan dioptimasi agar storage serta bandwidth hosting tetap terkontrol.

### 3. Functional Requirements

- Search tidak menerima check-out yang sama/lebih awal dari check-in.
- Date range, occupancy, quantity, minimum stay, promo, tax, dan service charge tervalidasi server-side.
- Hasil availability tidak hanya mengandalkan data dari browser.
- Sistem melakukan re-check dan write booking dalam database transaction.
- Reference number unik, tidak mudah ditebak secara berurutan, dan digunakan untuk komunikasi tamu.
- Booking menyimpan price breakdown dan rate snapshot.
- Perubahan status booking tercatat pada status log/audit log.
- Hold pending memiliki expiry yang diproses otomatis oleh scheduler.
- Admin tidak dapat confirm booking bila inventori sudah tidak cukup; sistem memberikan konflik dan opsi tindakan.
- Promo hanya berlaku jika tanggal, minimum nights, code, usage cap, dan status memenuhi.
- Inquiry/booking tersimpan walaupun email gagal; email dapat dikirim ulang oleh admin.
- Booking lookup tidak mengekspos booking lain dan diberi rate limit.
- Data guest hanya terlihat oleh admin yang terautentikasi.

### 4. Concurrency & Double-Booking Protection

- Confirmation dan booking creation menggunakan database transaction.
- Baris inventori/availability yang relevan dikunci atau menggunakan strategi atomic check yang disepakati developer.
- Availability diperiksa ulang di server tepat sebelum commit.
- Idempotency token mencegah double submit dari refresh/double-click.
- Admin mendapat error eksplisit bila booking sudah diproses oleh operator lain.
- Automated feature test wajib mensimulasikan dua request pada inventori terakhir.

### 5. Security & Privacy

- CSRF protection, server-side validation, escaped output, dan auth middleware.
- Rate limiting untuk login, search, submit booking, booking lookup, resend, dan password reset.
- Honeypot/CAPTCHA ringan pada public form.
- Upload divalidasi MIME type, extension, size, dan disimpan dengan generated filename.
- Password hash dan secret hanya melalui environment configuration.
- Session secure, HTTP-only, SameSite pada production.
- Booking lookup menggunakan kombinasi reference + guest identifier dan data response minimum.
- Data pribadi memiliki retention policy dan opsi anonymization setelah periode legal/bisnis yang ditentukan hotel.
- Terms, privacy consent, dan cancellation acknowledgement dicatat dengan timestamp/version bila diperlukan.
- Production error tidak menampilkan stack trace atau query.

### 6. SEO, Analytics & Performance

- Semantic HTML, canonical URL, sitemap, robots.txt, breadcrumb, Hotel/LocalBusiness schema, Offer, FAQ, dan room/product markup bila sesuai.
- Meta title, description, OG image per room/offer/page.
- Analytics events minimum: booking_search, room_view, select_room, begin_booking, booking_submit_success, booking_submit_error, WhatsApp click.
- Responsive WebP/AVIF images, gallery lazy loading, caching, asset minification, dan server-rendered HTML.
- Hero image diprioritaskan dan tidak lazy-load; media di bawah fold lazy-load.
- Target Core Web Vitals: LCP < 2.5 detik, CLS < 0.1, INP < 200 ms pada kondisi pengujian yang disepakati.

### 7. MVP Scope

**Included:** Responsive hotel website, CMS, room types, room amenities, rates, manual inventory blocks, promo rules sederhana, availability search, pending booking hold, confirmation flow, reservation admin, email notifications, audit minimum, SEO/analytics hooks, and deployment guide.

**Not Included / Phase 2:** Payment gateway, refund automation, invoice tax integration, OTA/channel manager (Booking.com/Agoda/Traveloka), PMS integration, multi-property, room-number assignment/housekeeping, loyalty program, member login, complex revenue management, dynamic pricing engine, WhatsApp Business API, restaurant/spa booking, mobile app, AI concierge.

### 8. Minimum Test Coverage

- Authentication, authorization, role access, and password reset.
- CRUD room type, rate, inventory block, promotion, content, and publish status.
- Availability across one/multiple nights, blocked dates, active hold, expired hold, and boundary dates.
- Price calculation: rate override, quantity, nights, discount, tax, service charge, rounding.
- Booking: valid flow, invalid guest/date, sold out, double submit, concurrent final inventory, email failure, expiry.
- Status transition rules dan audit log.
- Lookup privacy/rate limit.
- Manual responsive QA pada mobile, tablet, desktop.
- Production smoke test: search → book → notification → admin confirm → lookup.

### 9. Deployment & Operations

- Production menggunakan SSL, environment production, database migrations, storage link, cache warm-up, dan cron.
- Daily database backup dan media backup sesuai kemampuan hosting; restore procedure didokumentasikan.
- Uptime/error monitoring sederhana dan log rotation.
- Admin handbook singkat: mengubah rate, block inventory, confirm/cancel booking, resend email, dan export data.
- Staging environment direkomendasikan untuk acceptance test; bila budget sangat terbatas, dapat memakai temporary protected subdomain.

## Styling & Theming

- **Palette:** Warm Ivory (#FAF7F2), Deep Charcoal (#1F2933), Sand/Gold Accent (#B68A4A), dan satu warna brand hotel.
- **Typography:** Elegant display font untuk heading dan clean sans-serif untuk body; maksimal dua font family.
- **Layout:** Visual-led, editorial grid, max-width 1280px, full-bleed hero, mobile-first.
- **Spacing:** 8px system; section padding 80–120px desktop dan 48–72px mobile.
- **Photography:** Foto properti asli, bright natural lighting, consistent color grade, dan tidak menyesatkan kondisi kamar.
- **Room Information:** Informasi inti terbaca tanpa membuka accordion berulang.
- **Price Display:** Format `Rp1.250.000 / malam`, jelaskan apakah pajak/service termasuk.
- **Accessibility:** WCAG AA contrast, keyboard navigation, alt text, visible focus, form label, reduced motion.
- **Responsive Breakpoints:** Mobile 360px+, tablet 768px+, desktop 1024px+, wide 1440px+.

## UI Components Specification

- **Header:** Logo, navigation, contact/language optional, Book Now; mobile drawer dan sticky state.
- **Booking Search Bar:** Date picker, guest/room selector, promo code optional, search button; compact sticky variant opsional.
- **Room Card:** Cover, room name, capacity, bed, size, key amenities, starting rate, tax note, CTA.
- **Room Gallery:** Thumbnail, lightbox, swipe, keyboard controls, image count, alt text.
- **Amenity Chip:** Icon + label; tooltip hanya untuk info sekunder.
- **Availability Card:** Available unit indicator, rate plan, inclusions, cancellation summary, quantity selector, Select Room.
- **Price Summary:** Nights, quantity, room subtotal, promo, tax, service, grand total, currency; recalculates on server validation.
- **Guest Form:** Contact fields, special request, policy acknowledgement, consent, inline error.
- **Booking Status Badge:** Pending, confirmed, cancelled, expired, checked-in, completed, no-show dengan color + text, bukan color saja.
- **Confirmation Card:** Reference, guest, stay, room, total, status, next step, contact hotel.
- **Admin Booking Table:** Reference, guest, dates, room, total, status, source, created; search/filter/pagination/export.
- **Availability Calendar:** Room type rows, date columns, sold/blocked/available count, filter, legend, horizontal mobile scroll.
- **Inventory Block Modal:** Date range, room type, quantity, reason, validation against total units.
- **Rate Editor:** Base rate dan date-range override; preview effective rate.
- **Promo Editor:** Type/value/date/minimum stay/max uses/status dengan rule summary.
- **Toast/Alert:** Success, warning, error, booking conflict; conflict tidak auto-dismiss.
- **Empty/Error/Skeleton:** Clear action, retry, and safe fallback.

## Interactions & States

### Guest Booking Flow

1. Guest memasukkan tanggal dan occupancy.
2. Search button masuk loading state dan mencegah double-click.
3. Result menampilkan available room atau sold-out alternatives.
4. Guest memilih rate/quantity; sticky price summary diperbarui.
5. Guest mengisi data dan menyetujui policy.
6. Sistem re-check availability dan price.
7. Jika valid, booking pending dibuat dan confirmation ditampilkan.
8. Jika inventori berubah, sistem menampilkan konflik dan kembali ke result terbaru tanpa kehilangan data kontak bila aman.

### Admin Reservation Flow

1. Admin membuka pending booking dari dashboard.
2. Admin memeriksa detail, note, expiry, dan availability.
3. Admin menghubungi guest dan mencatat note.
4. Admin confirm/cancel; sistem menyimpan actor, timestamp, dan transition log.
5. Sistem mengirim update email dan memperbarui calendar.

### Required States

- **Initial:** Search bar dengan default guest count dan tanpa tanggal palsu.
- **Loading:** Skeleton/result placeholder; controls disabled seperlunya.
- **No Availability:** Alternatif tanggal/kamar dan WhatsApp contact.
- **Invalid Date/Occupancy:** Inline error yang spesifik.
- **Price Changed:** Tampilkan old/new total dan minta reconfirm sebelum submit.
- **Inventory Conflict:** Tidak membuat booking ganda; tawarkan refresh alternatives.
- **Pending:** Reference, expiry/next step, dan disclaimer belum confirmed.
- **Expired/Cancelled:** Status dan cara membuat booking baru.
- **Email Failed:** Booking tetap sukses; admin melihat delivery warning dan tombol resend.
- **404/500:** Branded recovery page dan contact fallback.
- **Maintenance Mode:** Informasi kontak/reservasi alternatif bila booking sementara tidak tersedia.

### Motion

- Gallery transition 200–300ms; scroll reveal ringan.
- Button/hover/focus transition 150–200ms.
- No autoplay audio; video hero muted dan memiliki poster/fallback bila digunakan.
- Hormati `prefers-reduced-motion`.

## Language & Localization

- **Primary Language:** Bahasa Indonesia.
- **Tone:** Ramah, meyakinkan, jelas, dan tidak overclaim.
- **Date Format:** `DD MMM YYYY`; date picker tetap menggunakan ISO pada backend.
- **Timezone:** Asia/Jakarta atau timezone properti yang dikonfirmasi.
- **Currency:** IDR dengan integer minor-unit policy yang konsisten.
- **Guest Communication:** Bahasa Indonesia pada MVP.
- **Phase 2:** Bahasa Inggris dengan localized URL dan localized content/rate policy.

## Example Mock Data

```json
{
  "hotel": {
    "name": "Hotel Arunika",
    "description": "A comfortable urban stay with warm Indonesian hospitality.",
    "checkInTime": "14:00",
    "checkOutTime": "12:00",
    "currency": "IDR",
    "taxPercent": 11,
    "serviceChargePercent": 10
  },
  "roomType": {
    "id": 1,
    "name": "Deluxe King",
    "slug": "deluxe-king",
    "sizeSqm": 32,
    "bedType": "1 King Bed",
    "maxAdults": 2,
    "maxChildren": 1,
    "totalUnits": 10,
    "baseRate": 850000,
    "status": "published"
  },
  "availability": {
    "checkInDate": "2026-09-12",
    "checkOutDate": "2026-09-14",
    "roomTypeId": 1,
    "availableUnits": 3,
    "nightlyRate": 850000
  },
  "booking": {
    "reference": "HTL-8F4K2Q",
    "guestName": "Budi Santoso",
    "guestEmail": "budi@example.com",
    "guestWhatsapp": "+6281234567890",
    "checkInDate": "2026-09-12",
    "checkOutDate": "2026-09-14",
    "adults": 2,
    "children": 0,
    "status": "pending",
    "subtotal": 1700000,
    "discountTotal": 0,
    "taxTotal": 187000,
    "serviceChargeTotal": 170000,
    "grandTotal": 2057000,
    "currency": "IDR"
  }
}
```

## Success Criteria

- ✅ Guest memahami positioning hotel, lokasi, tipe kamar, fasilitas, harga mulai, dan CTA booking dalam 5–10 detik.
- ✅ Guest dapat menyelesaikan search sampai booking confirmation dengan nyaman pada mobile 360px.
- ✅ Availability menghitung confirmed booking, active hold, dan inventory block pada seluruh malam yang dipilih.
- ✅ Booking tidak dibuat jika stok tidak cukup saat final server-side re-check.
- ✅ Concurrent request pada unit terakhir tidak menghasilkan overbooking.
- ✅ Booking menyimpan price snapshot, breakdown, unique reference, consent, dan status history.
- ✅ Pending hold kedaluwarsa otomatis dan inventori kembali tersedia.
- ✅ Admin dapat mengelola konten, kamar, rate, promo, block inventory, booking, dan hotel settings tanpa mengubah source code.
- ✅ Booking tetap tersimpan ketika email gagal dan admin dapat mengirim ulang notifikasi.
- ✅ Private guest data, admin routes, dan booking lookup terlindungi.
- ✅ SEO metadata, schema, sitemap, analytics events, dan optimized media berfungsi.
- ✅ Sistem dapat berjalan pada hosting Laravel ekonomis tanpa payment gateway, Redis, atau service premium wajib untuk MVP.

## Failed Criteria

- ❌ Website hanya menampilkan informasi tanpa CMS atau backend reservation yang dapat digunakan admin.
- ❌ Availability hanya dihitung di browser atau tidak memeriksa seluruh malam dalam date range.
- ❌ Dua booking dapat menghabiskan unit terakhir secara bersamaan dan menyebabkan overbooking.
- ❌ Perubahan harga setelah booking mengubah total booking lama karena tidak ada snapshot.
- ❌ Status `pending` ditampilkan sebagai confirmed atau guest tidak diberi next step yang jelas.
- ❌ Email gagal menyebabkan booking hilang/tidak tersimpan.
- ❌ Guest dapat melihat data booking orang lain hanya dengan menebak reference number.
- ❌ Admin tanpa permission dapat mengubah rate, inventory, booking, atau user.
- ❌ Tampilan date picker, search, room selection, atau price summary rusak pada mobile.
- ❌ Tax, service charge, promo, nights, atau quantity menghasilkan total yang tidak konsisten antara result, submit, admin, dan email.
- ❌ Payment gateway, OTA sync, PMS, multi-property, housekeeping, atau dynamic pricing masuk MVP tanpa change request.
- ❌ File sensitif, guest data, stack trace, environment secret, atau upload berbahaya terekspos.

