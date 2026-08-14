# PRD: Vehicle Fleet Management

## 1. Ringkasan
Vehicle Fleet Management adalah platform untuk mengelola armada kendaraan, pengemudi, pelacakan GPS, jadwal maintenance, dan konsumsi bahan bakar. Ditujukan untuk fleet manager, dispatcher, dan tim operasional logistik/transportasi. Nilai utamanya: visibilitas armada real-time, efisiensi biaya operasional, dan keselamatan pengemudi.

## 2. Problem Statement
Pengelolaan armada manual membuat lokasi kendaraan tidak diketahui real-time, jadwal servis terlewat, konsumsi BBM sulit diaudit (rawan penyelewengan), dan kinerja pengemudi tidak terpantau. Akibatnya biaya perawatan membengkak, downtime tinggi, dan risiko keselamatan meningkat.

## 3. Goals & Non-Goals
**Goals:**
- Registri kendaraan & pengemudi terpusat dengan dokumen.
- Pelacakan GPS real-time dan riwayat rute.
- Manajemen maintenance preventif & korektif berbasis jadwal/odometer.
- Monitoring konsumsi & biaya bahan bakar.

**Non-Goals:**
- Bukan sistem route optimization/last-mile penuh (integrasi opsional).
- Tidak mencakup penjualan/asuransi kendaraan.
- Tidak menangani payroll pengemudi (hanya data kinerja).

## 4. Target Users / Personas
- **Fleet Manager:** pantau seluruh armada, biaya, dan utilisasi.
- **Dispatcher:** tugaskan kendaraan & pengemudi ke perjalanan.
- **Driver (Pengemudi):** terima tugas, catat BBM, lapor kendala.
- **Maintenance Officer:** jadwalkan & catat servis kendaraan.

## 5. Key Features
### MVP
- Master kendaraan (plat, tipe, odometer, dokumen STNK/KIR).
- Master pengemudi (SIM, kontak, status, riwayat).
- Pelacakan GPS real-time & histori rute (via device/telematics).
- Jadwal maintenance preventif (berbasis waktu/odometer) + reminder.
- Pencatatan BBM (liter, biaya, odometer) & laporan konsumsi.
- Dashboard utilisasi, status kendaraan, dan biaya.

### Future / Phase 2+
- Deteksi perilaku berkendara (harsh braking, speeding) & skor driver.
- Geofencing & alert keluar area.
- Integrasi fuel card & deteksi anomali BBM otomatis.
- Route optimization & penjadwalan perjalanan.
- Prediksi kegagalan komponen (predictive maintenance).

## 6. User Stories
- Sebagai Fleet Manager, saya ingin melihat lokasi semua kendaraan real-time agar utilisasi optimal.
- Sebagai Maintenance Officer, saya ingin reminder servis otomatis agar tidak ada yang terlewat.
- Sebagai Dispatcher, saya ingin menugaskan kendaraan yang tersedia agar operasi lancar.
- Sebagai Fleet Manager, saya ingin laporan konsumsi BBM agar deteksi pemborosan.
- Sebagai Driver, saya ingin mencatat pengisian BBM dari HP agar cepat & akurat.

## 7. Functional Requirements
- FR-1: Sistem mengelola master kendaraan beserta dokumen & masa berlaku.
- FR-2: Sistem menampilkan posisi GPS real-time dan histori rute.
- FR-3: Sistem membuat jadwal maintenance berbasis waktu atau odometer.
- FR-4: Sistem mengirim reminder servis & masa berlaku dokumen (STNK/KIR/SIM).
- FR-5: Sistem mencatat transaksi BBM dan menghitung konsumsi (km/liter).
- FR-6: Sistem menugaskan kendaraan & pengemudi ke perjalanan.
- FR-7: Sistem menandai anomali (BBM tidak wajar, kendaraan idle lama).
- FR-8: Sistem menghitung biaya operasional per kendaraan (TCO).

## 8. Non-Functional Requirements
- Performance: peta live 500+ kendaraan update < 5 detik.
- Scalability: mendukung ribuan kendaraan & jutaan titik GPS/hari.
- Security: RBAC, enkripsi data lokasi, audit akses.
- Availability: uptime 99.7%, buffering data saat device offline.
- Compliance: perlindungan data lokasi pengemudi & regulasi transportasi.

## 9. Data Model (High-Level)
- **Vehicle** (plat, tipe, odometer, status, dokumen). Relasi: punya Trip, Maintenance, FuelLog.
- **Driver** (nama, SIM, kontak, status). Relasi: ditugaskan ke Trip.
- **Trip** (vehicle_id, driver_id, mulai, akhir, rute). Relasi: punya banyak GPSPoint.
- **GPSPoint** (trip_id, lat, lng, speed, timestamp). Relasi: milik satu Trip.
- **MaintenanceRecord** (vehicle_id, tipe, jadwal, biaya, status). Relasi: milik satu Vehicle.
- **FuelLog** (vehicle_id, liter, biaya, odometer, tanggal). Relasi: milik satu Vehicle.

## 10. Integrations
- GPS/telematics device & API (Teltonika, dll) untuk data lokasi.
- Peta (Google Maps/Mapbox) untuk visualisasi & geocoding.
- Fuel card provider untuk transaksi BBM (Phase 2).
- ERP/Accounting untuk biaya operasional & aset.
- WhatsApp/SMS/push untuk reminder & alert.

## 11. Success Metrics (KPI)
- Utilisasi armada naik > 15%.
- Biaya bahan bakar per km turun > 10%.
- Kepatuhan jadwal maintenance preventif > 95%.
- Downtime kendaraan turun > 25%.
- Insiden/pelanggaran berkendara turun (Phase 2).

## 12. Risks & Assumptions
- Risiko: akurasi/kualitas device GPS bervariasi -> perlu kalibrasi & buffering.
- Risiko: pencatatan BBM manual rawan salah -> integrasi fuel card di Phase 2.
- Asumsi: setiap kendaraan terpasang perangkat telematics.
- Asumsi: data dokumen & odometer awal tersedia saat onboarding.

## 13. Roadmap / Phases
- **Phase 1 (MVP):** master kendaraan/driver, GPS tracking, maintenance terjadwal, pencatatan BBM, dashboard.
- **Phase 2:** driver behavior scoring, geofencing, fuel card, deteksi anomali BBM.
- **Phase 3:** route optimization, predictive maintenance, analitik TCO lanjutan.
