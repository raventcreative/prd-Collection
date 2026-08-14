# PRD: E-Commerce Platform

## 1. Ringkasan
Platform e-commerce untuk penjualan produk online mencakup katalog, keranjang, checkout, pembayaran, dan manajemen pesanan. Ditujukan untuk brand dan merchant yang ingin toko online sendiri, bukan sekadar lapak marketplace. Nilai utamanya: kontrol penuh atas pengalaman belanja dan data pelanggan.

## 2. Problem Statement
Merchant yang hanya berjualan di marketplace tidak punya kontrol atas branding, data pelanggan, dan margin (potongan fee tinggi). Membangun toko online sendiri dari nol mahal dan rumit, terutama untuk integrasi pembayaran, pengiriman, dan manajemen stok/pesanan.

## 3. Goals & Non-Goals
**Goals:**
- Sediakan katalog produk dengan varian, kategori, dan pencarian.
- Alur keranjang dan checkout yang cepat dan minim friksi.
- Integrasi payment gateway dan metode pembayaran lokal.
- Manajemen pesanan, stok, dan status pengiriman.

**Non-Goals:**
- Marketplace multi-merchant (fokus single/ multi-store brand) di MVP.
- Modul akuntansi penuh.
- Fitur social commerce/live shopping di MVP.

## 4. Target Users / Personas
- **Pembeli (Nia):** butuh belanja cepat, checkout mudah, dan pembayaran aman.
- **Merchant/Owner (Bayu):** butuh kelola produk, harga, dan promo.
- **Staf Gudang (Eko):** butuh proses pesanan dan update stok/pengiriman.
- **Admin Toko (Lia):** butuh laporan penjualan dan kelola pelanggan.

## 5. Key Features
### MVP
- Katalog produk dengan varian, kategori, dan gambar.
- Pencarian & filter produk.
- Keranjang belanja & checkout guest/registered.
- Integrasi payment gateway (kartu, VA, e-wallet).
- Manajemen pesanan & status (paid, packed, shipped, done).
- Manajemen stok dasar & notifikasi order.

### Future / Phase 2+
- Kupon, diskon, dan program loyalitas.
- Multi-currency & multi-warehouse.
- Abandoned cart recovery.
- Rekomendasi produk & upsell.
- Review & rating produk.
- Integrasi omnichannel (marketplace sync).

## 6. User Stories
- Sebagai pembeli, saya ingin filter produk agar cepat menemukan yang dicari.
- Sebagai pembeli, saya ingin checkout tanpa daftar agar prosesnya cepat.
- Sebagai merchant, saya ingin atur varian dan stok agar tidak oversell.
- Sebagai staf gudang, saya ingin update status pesanan agar pembeli terinformasi.

## 7. Functional Requirements
- FR-1: Sistem mengelola produk dengan varian (ukuran/warna), harga, dan SKU.
- FR-2: Sistem menyediakan pencarian, filter, dan kategori produk.
- FR-3: Sistem mengelola keranjang persisten per user/session.
- FR-4: Sistem memproses checkout dengan perhitungan ongkir & pajak.
- FR-5: Sistem terintegrasi payment gateway dan menangani callback status.
- FR-6: Sistem mengurangi stok saat order terbayar dan mencegah oversell.
- FR-7: Sistem mengelola lifecycle pesanan dan notifikasi ke pembeli.
- FR-8: Sistem menyediakan dashboard penjualan dan manajemen pelanggan.

## 8. Non-Functional Requirements
- Performance: page load produk < 2 detik, checkout < 3 detik (p95).
- Scalability: tahan lonjakan traffic flash sale via caching & queue.
- Security: PCI-DSS untuk pembayaran, HTTPS, proteksi fraud dasar.
- Availability: SLA 99.9%, graceful degradation saat gateway down.
- Compliance: UU PDP, kebijakan refund & consumer protection.

## 9. Data Model (High-Level)
- **Product** (id, name, description, category_id, base_price). Relasi: 1-N ke Variant.
- **Variant** (id, product_id, sku, attributes, price, stock). Relasi: N-1 ke Product.
- **Cart** (id, user_id/session, items). Relasi: N-N ke Variant via CartItem.
- **Order** (id, customer_id, total, status, shipping_addr). Relasi: 1-N ke OrderItem, 1-1 ke Payment.
- **Payment** (id, order_id, method, status, amount). Relasi: N-1 ke Order.
- **Customer** (id, nama, email, addresses). Relasi: 1-N ke Order.

## 10. Integrations
- Payment gateway (Midtrans, Xendit, Stripe) untuk pembayaran.
- Kurir/logistik (JNE, SiCepat, RajaOngkir) untuk ongkir & tracking.
- Email/SMS/WhatsApp untuk notifikasi order.
- Analytics (GA4, pixel) untuk conversion tracking.
- Marketplace API (opsional) untuk sinkron stok.

## 11. Success Metrics (KPI)
- Conversion rate checkout >= target (mis. 2-3%).
- Cart abandonment rate menurun.
- Average order value (AOV) meningkat.
- Uptime saat flash sale sesuai SLA.
- Akurasi stok (insiden oversell mendekati nol).

## 12. Risks & Assumptions
- Risk: oversell saat traffic tinggi; mitigasi stock lock & idempotency.
- Risk: kegagalan payment callback; sediakan reconciliation.
- Risk: fraud & chargeback; tambahkan fraud check.
- Asumsi: merchant punya katalog & foto produk siap.
- Asumsi: metode pembayaran lokal cukup untuk target pasar.

## 13. Roadmap / Phases
- **Phase 1 (MVP):** katalog, keranjang, checkout, payment, order & stok dasar.
- **Phase 2:** kupon/diskon, review, abandoned cart, rekomendasi produk.
- **Phase 3:** multi-warehouse, multi-currency, omnichannel sync, loyalitas.
