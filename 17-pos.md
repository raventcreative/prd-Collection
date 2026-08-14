# PRD: Point of Sale (POS)

## 1. Ringkasan
POS adalah sistem kasir untuk memproses penjualan, mengelola inventori, dan mencetak struk di toko ritel dan F&B. Ditujukan untuk UMKM hingga bisnis multi-outlet dengan beberapa terminal kasir. Nilai utamanya: transaksi cepat dan andal bahkan tanpa internet (offline mode), dengan stok dan penjualan yang selalu sinkron.

## 2. Problem Statement
Pemilik toko kesulitan melacak penjualan dan stok secara real-time, terutama saat memiliki beberapa kasir atau cabang. Sistem kasir lama lambat, sering gagal saat internet mati, dan tidak menyatukan data penjualan dengan inventori. Akibatnya terjadi kehabisan stok, selisih kas, dan laporan yang tidak akurat.

## 3. Goals & Non-Goals
**Goals:**
- Memproses transaksi penjualan cepat dengan berbagai metode pembayaran.
- Mengelola inventori dengan pengurangan stok otomatis saat penjualan.
- Mendukung banyak terminal/kasir dan multi-outlet.
- Beroperasi offline dan sinkron otomatis saat online kembali.

**Non-Goals:**
- Bukan sistem akuntansi/pajak penuh (integrasi saja).
- Tidak mencakup e-commerce/online store lengkap (Phase 2+).
- Bukan platform CRM/marketing mendalam.

## 4. Target Users / Personas
- **Kasir (Wati):** butuh proses transaksi cepat, akurat, dan mudah dipelajari.
- **Pemilik Toko (Pak Hasan):** butuh laporan penjualan, stok, dan kinerja per outlet.
- **Store Manager (Lia):** butuh kelola produk, harga, promo, dan shift kasir.
- **Staf Gudang (Tono):** butuh update stok, stock opname, dan transfer antar outlet.

## 5. Key Features
### MVP
- Transaksi penjualan (scan barcode, cari produk, diskon, multi-payment).
- Manajemen produk & inventori dengan pengurangan stok otomatis.
- Cetak/kirim struk (thermal printer & e-receipt).
- Multi-terminal dalam satu outlet.
- Offline mode dengan sinkronisasi otomatis.

### Future / Phase 2+
- Multi-outlet dengan konsolidasi laporan & transfer stok.
- Program loyalitas & membership pelanggan.
- Integrasi pembayaran digital (QRIS, e-wallet) & EDC.
- Manajemen shift kas, cash drawer reconciliation.
- Integrasi online store & inventory omnichannel.

## 6. User Stories
- Sebagai kasir, saya ingin scan barcode dan langsung tambah ke keranjang agar antrean cepat.
- Sebagai kasir, saya ingin tetap bisa bertransaksi saat internet mati agar tidak kehilangan penjualan.
- Sebagai pemilik, saya ingin melihat total penjualan hari ini per outlet agar tahu performa.
- Sebagai manager, saya ingin stok berkurang otomatis saat terjual agar data akurat.

## 7. Functional Requirements
- FR-1: Sistem dapat membuat transaksi dengan menambah item via barcode/pencarian, diskon, dan pajak.
- FR-2: Sistem mendukung banyak metode pembayaran (tunai, kartu, QRIS) termasuk split payment.
- FR-3: Sistem mengurangi stok produk otomatis saat transaksi selesai.
- FR-4: Sistem mencetak struk termal dan/atau mengirim e-receipt.
- FR-5: Sistem menyimpan transaksi secara lokal saat offline dan sinkron ke server saat online.
- FR-6: Sistem mendukung banyak terminal yang berbagi katalog dan stok per outlet.
- FR-7: Sistem menghasilkan laporan penjualan harian, per produk, dan per kasir.

## 8. Non-Functional Requirements
- Performance: Tambah item dan proses bayar < 300 ms; siap dipakai walau perangkat low-end.
- Scalability: Mendukung ratusan outlet dan ribuan terminal concurrent.
- Security: RBAC (kasir/manager/owner), audit transaksi, proteksi void/refund.
- Availability: Berfungsi penuh offline; uptime server sync 99.9%.
- Compliance: Sesuai regulasi pajak lokal (mis. PPN/faktur), keamanan data pembayaran (PCI-DSS bila proses kartu).

## 9. Data Model (High-Level)
- **Product** (id, sku, name, price, stock_qty, outlet_id, barcode). Relasi: muncul di banyak SaleItem.
- **Sale** (id, outlet_id, terminal_id, cashier_id, total, status, created_at, synced). Relasi: punya banyak SaleItem & Payment.
- **SaleItem** (id, sale_id, product_id, qty, unit_price, discount). Relasi: milik satu Sale.
- **Payment** (id, sale_id, method, amount). Relasi: milik satu Sale.
- **Outlet** (id, name, address). Relasi: punya banyak Terminal & Product.
- **Terminal** (id, outlet_id, name). Relasi: mencatat banyak Sale.

## 10. Integrations
- Payment gateway/EDC & QRIS (Midtrans, Xendit) untuk pembayaran digital.
- Accounting software (Accurate, Jurnal) untuk pembukuan.
- Barcode scanner & thermal printer (ESC/POS).
- Cloud backend untuk sinkronisasi multi-outlet.
- Notifikasi (email/WhatsApp) untuk e-receipt.

## 11. Success Metrics (KPI)
- Rata-rata waktu proses transaksi < 30 detik.
- Uptime transaksi (termasuk offline) mendekati 100%.
- Selisih kas (cash variance) turun signifikan.
- Akurasi stok > 98% dibanding stock opname.

## 12. Risks & Assumptions
- Risiko: konflik data saat sinkronisasi offline dari banyak terminal.
- Risiko: kompatibilitas hardware printer/scanner beragam.
- Asumsi: setiap outlet punya perangkat lokal yang cukup untuk menyimpan data offline.
- Asumsi: kasir dilatih minimal untuk operasi dasar.

## 13. Roadmap / Phases
- **Phase 1 (MVP):** Transaksi, inventori dasar, struk, multi-terminal, offline sync.
- **Phase 2:** Multi-outlet, transfer stok, loyalitas, pembayaran digital, shift & cash reconciliation.
- **Phase 3:** Omnichannel/online store, analytics lanjutan, integrasi akuntansi & pajak penuh.
