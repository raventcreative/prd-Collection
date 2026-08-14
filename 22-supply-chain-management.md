# PRD: Supply Chain Management (SCM)

## 1. Ringkasan
Supply Chain Management adalah platform terintegrasi untuk perencanaan permintaan (demand planning), pengelolaan inventory, logistik/pengiriman, dan manajemen supplier. Ditujukan untuk tim supply chain, planner, dan operasional gudang di perusahaan manufaktur/distribusi. Nilai utamanya: visibilitas end-to-end rantai pasok, optimasi stok, dan pengiriman tepat waktu.

## 2. Problem Statement
Rantai pasok dikelola dengan sistem terpisah dan spreadsheet, membuat perencanaan permintaan tidak akurat, stok sering kekurangan atau berlebih, status pengiriman tidak terpantau, dan kinerja supplier tidak terukur. Akibatnya biaya inventory tinggi, stockout, dan keterlambatan pengiriman.

## 3. Goals & Non-Goals
**Goals:**
- Demand planning berbasis histori & forecast.
- Visibilitas inventory real-time multi-gudang.
- Pelacakan logistik/pengiriman inbound & outbound.
- Manajemen supplier dengan evaluasi kinerja (OTIF).

**Non-Goals:**
- Bukan sistem procurement penuh (integrasi dengan modul Procurement).
- Tidak mencakup MES/manufacturing execution di lantai produksi.
- Tidak menggantikan TMS eksternal (integrasi carrier).

## 4. Target Users / Personas
- **Demand Planner:** susun forecast & rencana replenishment.
- **Inventory/Warehouse Officer:** kelola stok, penerimaan, dan pengeluaran.
- **Logistics Coordinator:** atur & pantau pengiriman dan carrier.
- **Supply Chain Manager:** pantau KPI end-to-end & kinerja supplier.

## 5. Key Features
### MVP
- Demand forecasting berbasis histori penjualan (statistik dasar).
- Inventory management multi-gudang (stok, batch, reorder point).
- Purchase/replenishment plan otomatis dari reorder point.
- Pelacakan shipment inbound/outbound dengan status.
- Master supplier & lead time.
- Dashboard KPI (fill rate, stock level, OTIF).

### Future / Phase 2+
- Forecast berbasis ML (musiman, promosi, faktor eksternal).
- Optimasi safety stock & multi-echelon inventory.
- Integrasi carrier/TMS untuk tracking real-time & ongkir.
- Control tower & alert disrupsi rantai pasok.
- Supplier collaboration portal.

## 6. User Stories
- Sebagai Demand Planner, saya ingin forecast otomatis agar rencana stok lebih akurat.
- Sebagai Warehouse Officer, saya ingin melihat stok real-time agar hindari stockout.
- Sebagai Logistics Coordinator, saya ingin memantau status pengiriman agar bisa antisipasi telat.
- Sebagai SC Manager, saya ingin KPI OTIF supplier agar evaluasi kerja sama objektif.
- Sebagai Planner, saya ingin rekomendasi replenishment agar tidak over/understock.

## 7. Functional Requirements
- FR-1: Sistem menghasilkan forecast permintaan per SKU per periode.
- FR-2: Sistem melacak stok real-time per gudang, batch, dan lokasi.
- FR-3: Sistem menghitung reorder point & membuat usulan replenishment.
- FR-4: Sistem mencatat & melacak status shipment inbound/outbound.
- FR-5: Sistem menyimpan master supplier beserta lead time & term.
- FR-6: Sistem menghitung KPI (fill rate, inventory turnover, OTIF).
- FR-7: Sistem memberi alert saat stok di bawah safety stock.
- FR-8: Sistem mendukung transfer stok antar gudang.

## 8. Non-Functional Requirements
- Performance: query stok & dashboard < 2 detik untuk 100rb+ SKU.
- Scalability: mendukung banyak gudang, jutaan transaksi stok/tahun.
- Security: RBAC, audit trail pergerakan stok, enkripsi data.
- Availability: uptime 99.7%, sinkronisasi offline gudang.
- Compliance: ketertelusuran batch/lot (traceability) & standar industri.

## 9. Data Model (High-Level)
- **Product/SKU** (kode, nama, UoM, kategori). Relasi: punya banyak StockItem.
- **Warehouse** (nama, lokasi, tipe). Relasi: menampung StockItem.
- **StockItem** (sku_id, warehouse_id, qty, batch, reorder_point). Relasi: dicatat StockMovement.
- **Forecast** (sku_id, periode, qty_prediksi). Relasi: dasar ReplenishmentPlan.
- **Shipment** (tipe, origin, dest, status, carrier). Relasi: punya banyak ShipmentItem.
- **Supplier** (nama, lead_time, term, rating). Relasi: memasok SKU & Shipment.

## 10. Integrations
- ERP (SAP, Oracle) untuk order, penjualan, dan finance.
- Modul Procurement untuk PO & penerimaan.
- Carrier/TMS (JNE, SiCepat, DHL) untuk tracking & ongkir.
- WMS/barcode scanner untuk operasi gudang.
- BI/Data warehouse untuk analitik lanjutan.

## 11. Success Metrics (KPI)
- Akurasi forecast (MAPE) membaik > 15%.
- Inventory turnover naik & carrying cost turun.
- Fill rate/order fulfillment > 95%.
- OTIF (On-Time In-Full) supplier > 90%.
- Frekuensi stockout turun > 40%.

## 12. Risks & Assumptions
- Risiko: data histori penjualan tidak bersih -> forecast meleset.
- Risiko: integrasi ERP/carrier kompleks -> perlu fase bertahap.
- Asumsi: master data SKU & gudang terstandardisasi.
- Asumsi: proses gudang siap digitalisasi (barcode/scanner).

## 13. Roadmap / Phases
- **Phase 1 (MVP):** forecasting dasar, inventory multi-gudang, replenishment, shipment tracking, supplier & KPI.
- **Phase 2:** forecast ML, optimasi safety stock, integrasi carrier/TMS, control tower.
- **Phase 3:** multi-echelon optimization, supplier portal, prediksi disrupsi.
