# PRD: Personal Landing Page - Personal Brand & Lead Generation

**Project:** Personal Brand Website with Laravel CMS  
**Aesthetic:** Premium, Modern, Credible, Minimal, Conversion-Focused.

> **Product assumption:** Website ditujukan untuk founder, profesional, kreator, konsultan, atau pembicara yang ingin membangun kredibilitas dan menghasilkan inquiry. Nama, identitas visual, isi final, domain, dan tujuan CTA utama dikonfirmasi saat kickoff.

## Core Layout

### 1. User & Role

- **Visitor:** Melihat profil, layanan, portofolio, testimonial, artikel, dan mengirim inquiry.
- **Admin:** Login ke CMS untuk mengelola seluruh konten serta melihat lead.
- **Super Admin (opsional):** Hanya diperlukan bila website dikelola lebih dari satu akun. Untuk MVP, satu akun Admin sudah cukup.

### 2. Public Website

- **Sticky Header:** Logo/nama, Home, About, Services, Portfolio, Insights, Contact, dan CTA utama.
- **Hero Section:** Foto/video utama, positioning statement, value proposition, social proof singkat, CTA primer, dan CTA sekunder.
- **About Section:** Bio singkat, pengalaman, pencapaian, statistik, dan link ke profil lengkap.
- **Services Section:** Daftar layanan dengan manfaat, target klien, deliverables ringkas, dan CTA konsultasi.
- **Featured Portfolio:** Proyek unggulan, kategori, ringkasan tantangan, solusi, hasil, dan detail studi kasus.
- **Social Proof:** Logo klien/partner, testimonial, angka pencapaian, penghargaan, atau media coverage.
- **Insights/Articles:** Daftar artikel terbaru untuk membangun authority dan organic traffic.
- **Speaking/Experience (opsional):** Pengalaman workshop, event, sertifikasi, atau kolaborasi.
- **Contact/Lead Form:** Nama, email, WhatsApp, perusahaan, kebutuhan, budget range, pesan, dan consent.
- **Footer:** Kontak, social links, quick links, copyright, privacy policy, dan terms.
- **Mobile Floating CTA:** Tombol WhatsApp atau booking consultation yang tidak menutup konten.

### 3. Detail Pages

- **About Page:** Profil lengkap, perjalanan, prinsip kerja, pengalaman, dan milestone.
- **Service Detail:** Deskripsi, problem yang diselesaikan, process, deliverables, FAQ, dan CTA.
- **Portfolio/Case Study Detail:** Cover, client/project summary, challenge, solution, gallery, result, dan CTA.
- **Article List & Detail:** Kategori, search sederhana, cover, author, tanggal, content, related articles, dan share link.
- **Contact Page:** Form, WhatsApp, email, alamat opsional, jam respons, dan embedded map opsional.
- **Legal Pages:** Privacy Policy dan Terms & Conditions yang dapat diedit dari CMS.

### 4. Admin CMS

- **Dashboard:** Total inquiry, inquiry baru, published projects, published articles, dan shortcut CRUD.
- **Profile & Hero:** Nama, headline, bio, foto, CTA, statistik, dan CV/company profile download.
- **Services:** Create, read, update, delete, urutan tampil, featured, publish/unpublish.
- **Portfolio:** CRUD proyek, kategori, cover, gallery, hasil, external link, featured, dan status publikasi.
- **Testimonials & Client Logos:** CRUD, urutan tampil, status aktif, dan upload media.
- **Articles:** CRUD, draft/published, slug, cover, kategori, SEO metadata, dan scheduled publish opsional.
- **Leads/Inquiry:** List, detail, filter status, search, internal note, update status, export CSV.
- **Navigation & Social Links:** Ubah label, link, urutan, dan status aktif.
- **SEO & Site Settings:** Site title, meta description, OG image, favicon, analytics ID, contact, dan default social preview.
- **Media Library:** Upload, preview, alt text, replace, dan delete media yang tidak terpakai.
- **Account:** Update profil admin, password, password reset, dan logout.

### 5. Navigation Structure

```text
Public
├── Home
├── About
├── Services
│   └── Service Detail
├── Portfolio
│   └── Case Study Detail
├── Insights
│   └── Article Detail
├── Contact
├── Privacy Policy
└── Terms & Conditions

Admin
├── Dashboard
├── Profile & Hero
├── Services
├── Portfolio
├── Testimonials
├── Client Logos
├── Articles & Categories
├── Leads
├── Media
├── SEO & Site Settings
└── Account
```

## Data Structure (Mock Only)

> Interface berikut adalah representasi data untuk menyamakan pemahaman UI dan backend. Implementasi backend menggunakan Eloquent Model, migration, enum/cast, dan relational database.

```typescript
type PublishStatus = 'draft' | 'published' | 'archived';
type LeadStatus = 'new' | 'contacted' | 'qualified' | 'won' | 'lost' | 'spam';

interface SiteSetting {
  id: number;
  siteName: string;
  siteTitle: string;
  metaDescription: string;
  logoPath?: string;
  faviconPath?: string;
  defaultOgImagePath?: string;
  primaryEmail?: string;
  primaryWhatsapp?: string;
  analyticsId?: string;
}

interface Profile {
  id: number;
  fullName: string;
  headline: string;
  shortBio: string;
  fullBio?: string;
  photoPath?: string;
  heroMediaPath?: string;
  primaryCtaLabel: string;
  primaryCtaUrl: string;
  secondaryCtaLabel?: string;
  secondaryCtaUrl?: string;
}

interface Service {
  id: number;
  title: string;
  slug: string;
  excerpt: string;
  description: string;
  icon?: string;
  coverPath?: string;
  ctaLabel?: string;
  ctaUrl?: string;
  sortOrder: number;
  isFeatured: boolean;
  status: PublishStatus;
}

interface Project {
  id: number;
  categoryId?: number;
  title: string;
  slug: string;
  clientName?: string;
  excerpt: string;
  challenge?: string;
  solution?: string;
  result?: string;
  coverPath?: string;
  externalUrl?: string;
  completedAt?: string;
  isFeatured: boolean;
  status: PublishStatus;
}

interface Testimonial {
  id: number;
  name: string;
  role?: string;
  company?: string;
  quote: string;
  photoPath?: string;
  rating?: number;
  sortOrder: number;
  isActive: boolean;
}

interface Article {
  id: number;
  categoryId?: number;
  title: string;
  slug: string;
  excerpt: string;
  content: string;
  coverPath?: string;
  status: PublishStatus;
  publishedAt?: string;
  metaTitle?: string;
  metaDescription?: string;
}

interface Inquiry {
  id: number;
  name: string;
  email: string;
  whatsapp?: string;
  company?: string;
  serviceId?: number;
  budgetRange?: string;
  message: string;
  source?: string;
  status: LeadStatus;
  adminNote?: string;
  consentAt: string;
  createdAt: string;
}
```

### Laravel Database Entities

| Table | Purpose | Key Fields |
| --- | --- | --- |
| `users` | Akun admin | name, email, password, role, last_login_at |
| `site_settings` | Pengaturan global website | key/value atau kolom setting utama |
| `profiles` | Konten hero dan profil | headline, bio, CTA, media |
| `services` | Daftar layanan | title, slug, content, status, sort_order |
| `project_categories` | Kategori portofolio | name, slug |
| `projects` | Studi kasus/proyek | client, challenge, solution, result, status |
| `project_images` | Gallery proyek | project_id, image_path, alt_text, sort_order |
| `testimonials` | Testimonial klien | identity, quote, rating, is_active |
| `client_logos` | Logo klien/partner | name, logo_path, url, sort_order |
| `article_categories` | Kategori artikel | name, slug |
| `articles` | Artikel/insight | title, slug, content, SEO, published_at |
| `inquiries` | Lead dari form | contact fields, message, status, admin_note |
| `social_links` | Link sosial | platform, url, icon, sort_order |
| `media` | Metadata media upload | path, mime_type, size, alt_text |

## Implementation Details

### 1. Recommended Stack

- **Backend & Server Rendering:** Laravel 13 atau versi stabil aktif saat kickoff.
- **Runtime:** PHP 8.3+ sesuai requirement Laravel yang dipakai.
- **Frontend:** Blade, Tailwind CSS, Alpine.js; Livewire hanya bila interaksi CMS memerlukannya.
- **Database:** MySQL atau MariaDB.
- **Authentication:** Laravel starter/auth stack untuk admin; public registration dimatikan.
- **Storage:** Local public disk pada MVP; S3-compatible storage menjadi opsi saat trafik/media meningkat.
- **Email:** SMTP untuk notifikasi inquiry dan password reset.
- **Build Tool:** Vite.
- **Web Server:** Nginx atau Apache pada shared hosting/VPS yang mendukung Laravel.
- **Queue/Cache:** Database driver pada MVP agar tidak wajib Redis.

### 2. Cost-Control Architecture

- Satu repository, satu Laravel application, satu database, dan satu deployment.
- Public website menggunakan server-rendered HTML agar SEO baik dan tidak memerlukan API/frontend terpisah.
- Admin CMS dibuat custom hanya untuk modul yang dibutuhkan; tidak membuat page builder bebas yang mahal dan rawan rusak.
- Media dikompresi ke WebP/AVIF saat upload bila library server mendukung.
- Email inquiry dapat diproses melalui database queue; cron menjalankan queue dan scheduler.
- WhatsApp CTA menggunakan deep link; WhatsApp Business API bukan scope MVP.

### 3. Functional Requirements

- Admin dapat memperbarui konten utama tanpa mengubah source code.
- Slug harus unik dan dapat dibuat otomatis dari judul.
- Perubahan konten memiliki status draft/published agar konten belum siap tidak tampil publik.
- Form inquiry wajib tervalidasi, memiliki honeypot/CAPTCHA ringan, dan rate limiting.
- Inquiry tersimpan di database walaupun pengiriman email gagal.
- Admin menerima email notifikasi ketika lead baru berhasil masuk.
- Semua gambar memiliki alt text dan fallback image.
- Delete data penting menggunakan soft delete bila relevan.
- Export lead menghasilkan CSV dengan filter aktif.
- URL lama proyek/artikel dapat diberi redirect ketika slug berubah.

### 4. Security & Reliability

- CSRF protection untuk semua form internal.
- Server-side validation dan output escaping.
- Rate limit untuk login, inquiry, dan password reset.
- Pembatasan MIME type, extension, dan ukuran upload.
- Password di-hash; credential hanya disimpan melalui environment configuration.
- Admin route dilindungi auth middleware.
- Session cookie secure, HTTP-only, dan SameSite pada production.
- Scheduled database backup minimal harian sesuai kemampuan hosting.
- Error production tidak menampilkan stack trace atau secret.

### 5. SEO, Analytics & Performance

- Semantic HTML, canonical URL, sitemap XML, robots.txt, schema markup Person/Organization/Article bila sesuai.
- Meta title, meta description, OG/Twitter card per halaman.
- Google Analytics/Tag Manager ID dapat diatur dari CMS.
- Event tracking minimum: primary CTA click, WhatsApp click, contact form success, service view, portfolio view.
- Lazy loading untuk media non-hero, responsive image sizes, CSS/JS minification, dan page caching untuk halaman publik.
- Target Core Web Vitals: LCP < 2.5 detik, CLS < 0.1, INP < 200 ms pada kondisi pengujian yang disepakati.

### 6. MVP Scope

**Included:** Public pages, responsive design, admin auth, CMS modules, media upload, inquiry management, email notification, basic SEO, analytics hooks, deployment guide, and smoke tests.

**Not Included / Phase 2:** Multi-admin approval workflow, visual drag-and-drop page builder, member area, online payment, advanced CRM, newsletter automation, WhatsApp Business API, AI chatbot, complex multilingual content, mobile app.

### 7. Minimum Test Coverage

- Feature test untuk admin authentication dan authorization.
- Feature test untuk create/update/publish core content.
- Feature test untuk inquiry success, invalid input, spam/rate limit, dan email failure fallback.
- Test slug uniqueness dan visibility draft/published.
- Manual responsive QA pada mobile, tablet, dan desktop.
- Broken-link, metadata, sitemap, form, and upload smoke test sebelum production.

## Styling & Theming

- **Palette:** Neutral base (White #FFFFFF, Off White #F8FAFC, Charcoal #111827) dengan satu accent color dari brand.
- **Typography:** Display font yang berkarakter untuk heading dan sans-serif mudah dibaca untuk body; maksimal dua font family.
- **Layout:** 12-column desktop grid, max-width 1200–1280px, mobile-first, generous whitespace.
- **Spacing:** 8px spacing system; section gap 72–120px desktop dan 48–72px mobile.
- **Border Radius:** 12–20px untuk cards; button konsisten 10–14px.
- **Imagery:** Foto autentik, resolusi tinggi, konsisten tone, tanpa crop wajah yang buruk.
- **Accessibility:** Minimum contrast WCAG AA, visible focus state, keyboard-friendly navigation, dan reduced-motion support.
- **Responsive Breakpoints:** Mobile 360px+, tablet 768px+, desktop 1024px+, wide 1440px+.

## UI Components Specification

- **Header:** Transparan/solid sesuai scroll; active navigation; mobile drawer; CTA selalu terlihat namun tidak dominan berlebihan.
- **Hero:** Kicker, H1, supporting copy, 1–2 CTA, trust indicator, portrait/media; H1 hanya satu per page.
- **Stat Card:** Angka utama, label, optional source note.
- **Service Card:** Icon, title, short benefit, link; seluruh card dapat diklik dan memiliki focus state.
- **Portfolio Card:** Cover, category, title, result highlight; hover hanya enhancement, bukan satu-satunya cara mengakses detail.
- **Testimonial Card/Slider:** Quote, identity, company, rating opsional; slider dapat di-swipe dan dikontrol keyboard.
- **Article Card:** Cover, category, date, title, excerpt, read time opsional.
- **Lead Form:** Label selalu terlihat, inline error, loading state, success state, privacy consent.
- **WhatsApp CTA:** Membuka pesan pre-filled dengan source page bila memungkinkan.
- **Admin Data Table:** Search, filter, pagination, status badge, bulk action terbatas, empty state.
- **CMS Editor:** Plain rich-text editor, media insert, preview, slug, SEO fields, draft/publish controls.
- **Toast/Alert:** Success, warning, error; tidak hilang terlalu cepat untuk pesan penting.
- **Skeleton/Loading:** Hanya untuk komponen async; halaman Blade utama tidak bergantung pada loading spinner.

## Interactions & States

### Public Flows

1. Visitor membuka landing page dan langsung memahami siapa pemilik website, apa value yang ditawarkan, dan tindakan berikutnya.
2. Visitor memilih layanan atau portfolio untuk melihat detail.
3. Visitor mengklik CTA WhatsApp atau mengisi inquiry form.
4. Sistem memvalidasi input, menyimpan lead, lalu mengirim notifikasi email.
5. Visitor melihat success state dan estimasi waktu respons.

### Admin Flows

1. Admin login melalui `/admin/login`.
2. Admin melihat lead baru atau memilih modul konten.
3. Admin menyimpan draft, preview, lalu publish.
4. Admin dapat mengubah urutan item featured tanpa mengedit code.
5. Admin mengubah status lead dan menambahkan internal note.

### Required States

- **Loading:** Button disabled dan indikator proses saat submit.
- **Empty:** Pesan jelas dan CTA create pertama pada CMS.
- **Validation Error:** Pesan per field dalam Bahasa Indonesia.
- **System Error:** Pesan aman tanpa detail teknis, dengan opsi coba lagi.
- **No Search Result:** Menampilkan reset filter.
- **Draft/Unpublished:** Tidak dapat diakses publik; admin dapat preview melalui signed/private preview URL.
- **Offline/Network Failure:** Input form tidak dianggap berhasil dan pengguna diberi arahan retry.
- **404/500:** Halaman branded, memiliki link kembali ke Home/Contact.

### Motion

- Scroll reveal ringan 150–300ms, tidak menghambat membaca.
- Hover/focus transition 150–200ms.
- Hormati `prefers-reduced-motion`.
- Tidak memakai autoplay audio atau animasi berat.

## Language & Localization

- **Primary Language:** Bahasa Indonesia.
- **Content Tone:** Profesional, human, percaya diri, ringkas, dan berorientasi manfaat.
- **Date Format:** `DD MMMM YYYY`.
- **Timezone:** Asia/Jakarta.
- **Currency:** IDR hanya bila harga layanan ditampilkan.
- **Phase 2:** Bahasa Inggris dengan localized URL (`/en/...`) dan field translation; tidak termasuk MVP untuk menjaga biaya.

## Example Mock Data

```json
{
  "profile": {
    "fullName": "Nama Profesional",
    "headline": "Helping Businesses Grow Through Strategy, Technology, and Execution",
    "shortBio": "Founder dan consultant dengan pengalaman membangun solusi yang berdampak langsung pada bisnis.",
    "primaryCtaLabel": "Jadwalkan Konsultasi",
    "primaryCtaUrl": "/contact"
  },
  "services": [
    {
      "title": "Business Consulting",
      "slug": "business-consulting",
      "excerpt": "Strategi praktis untuk menyelesaikan bottleneck dan mempercepat pertumbuhan.",
      "isFeatured": true,
      "status": "published"
    },
    {
      "title": "Workshop & Speaking",
      "slug": "workshop-speaking",
      "excerpt": "Sesi yang dirancang sesuai kebutuhan tim dan outcome bisnis.",
      "isFeatured": true,
      "status": "published"
    }
  ],
  "featuredProject": {
    "title": "Operational Transformation",
    "slug": "operational-transformation",
    "clientName": "Confidential Client",
    "result": "Proses pelaporan menjadi lebih rapi dan waktu persiapan berkurang.",
    "status": "published"
  }
}
```

## Success Criteria

- ✅ Visitor memahami positioning, layanan utama, dan CTA dalam 5–10 detik pertama.
- ✅ Seluruh halaman utama responsif dan usable pada mobile, tablet, dan desktop.
- ✅ Admin dapat mengelola profil, layanan, portfolio, testimonial, artikel, logo klien, SEO, dan site settings tanpa bantuan developer.
- ✅ Inquiry valid tersimpan di database dan menghasilkan notifikasi ke admin.
- ✅ Spam dasar ditekan melalui validation, honeypot/CAPTCHA, dan rate limiting.
- ✅ Draft tidak tampil publik dan published content dapat ditemukan melalui URL yang stabil.
- ✅ Metadata SEO, sitemap, canonical URL, social preview, dan analytics event dasar berfungsi.
- ✅ Media upload aman, terkompresi, dan tidak merusak layout.
- ✅ Website dapat di-deploy pada hosting Laravel ekonomis tanpa service berbayar wajib selain domain/hosting/email bila diperlukan.
- ✅ Tidak ada critical/high severity issue pada security review dan production smoke test.

## Failed Criteria

- ❌ Website hanya berupa halaman statis sehingga konten utama harus diubah melalui source code.
- ❌ CTA utama tidak jelas atau form inquiry tidak menyimpan data ketika email gagal.
- ❌ Admin yang belum login dapat mengakses CMS atau endpoint mutasi data.
- ❌ Draft, data pribadi lead, stack trace, credential, atau file sensitif terekspos publik.
- ❌ Tampilan rusak, horizontal overflow, CTA tertutup, atau form sulit digunakan pada mobile 360px.
- ❌ Upload menerima file berbahaya/tidak tervalidasi atau file terlalu besar tanpa batas.
- ❌ Halaman penting tidak memiliki title, description, canonical, atau tidak tercakup dalam sitemap.
- ❌ Performa halaman utama gagal mencapai target yang disepakati tanpa alasan third-party yang terdokumentasi.
- ❌ Scope MVP diam-diam berkembang menjadi CRM, page builder, payment system, atau aplikasi mobile tanpa change request.

