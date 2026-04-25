<!DOCTYPE html>
<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, user-scalable=yes">
    <title>Inventory Management System - Barang Masuk & Stok Consumable</title>
    <!-- Font Awesome 6 (free) -->
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0-beta3/css/all.min.css">
    <!-- Google Fonts -->
    <link href="https://fonts.googleapis.com/css2?family=Inter:opsz,wght@14..32,400;14..32,500;14..32,600;14..32,700&display=swap" rel="stylesheet">
    <!-- SheetJS (XLSX) untuk export Excel -->
    <script src="https://cdn.sheetjs.com/xlsx-0.20.2/package/dist/xlsx.full.min.js"></script>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            font-family: 'Inter', sans-serif;
        }

        body {
            background: #f1f5f9;
            padding: 24px 20px;
            color: #0f172a;
        }

        .container {
            max-width: 1600px;
            margin: 0 auto;
        }

        /* Dashboard header + filter global */
        .dashboard-header {
            margin-bottom: 28px;
        }

        h1 {
            font-size: 1.8rem;
            font-weight: 700;
            background: linear-gradient(135deg, #1e3c72, #2b3b6e);
            -webkit-background-clip: text;
            background-clip: text;
            color: transparent;
            margin-bottom: 8px;
        }

        .sub {
            color: #334155;
            margin-bottom: 20px;
            border-left: 4px solid #3b82f6;
            padding-left: 14px;
            font-weight: 500;
        }

        /* dua panel card */
        .two-panels {
            display: flex;
            flex-wrap: wrap;
            gap: 28px;
            margin-bottom: 40px;
        }

        .panel {
            flex: 1;
            min-width: 340px;
            background: white;
            border-radius: 28px;
            box-shadow: 0 12px 30px rgba(0,0,0,0.05);
            overflow: hidden;
            transition: all 0.2s;
            border: 1px solid #e2e8f0;
        }

        .panel-header {
            background: #ffffff;
            padding: 18px 24px;
            border-bottom: 2px solid #eef2ff;
            display: flex;
            justify-content: space-between;
            align-items: center;
            flex-wrap: wrap;
            gap: 12px;
        }

        .panel-header h2 {
            font-size: 1.4rem;
            font-weight: 700;
            color: #0f3b5c;
            display: flex;
            align-items: center;
            gap: 10px;
        }

        .btn-add {
            background: #3b82f6;
            border: none;
            color: white;
            padding: 8px 18px;
            border-radius: 40px;
            font-weight: 600;
            font-size: 0.85rem;
            cursor: pointer;
            transition: 0.2s;
            display: inline-flex;
            align-items: center;
            gap: 8px;
        }

        .btn-add:hover {
            background: #2563eb;
            transform: scale(1.02);
        }

        .table-wrapper {
            overflow-x: auto;
            padding: 0 16px 20px 16px;
        }

        table {
            width: 100%;
            border-collapse: collapse;
            font-size: 0.85rem;
        }

        th, td {
            padding: 12px 8px;
            text-align: left;
            border-bottom: 1px solid #e9eef3;
        }

        th {
            background-color: #f8fafc;
            color: #1e293b;
            font-weight: 600;
        }

        .action-icons i {
            margin: 0 6px;
            cursor: pointer;
            font-size: 1rem;
            transition: 0.1s;
        }

        .fa-edit { color: #3b82f6; }
        .fa-trash-alt { color: #ef4444; }
        .fa-edit:hover { color: #1e40af; }
        .fa-trash-alt:hover { color: #b91c1c; }

        /* Filter & dropdown bar */
        .filter-bar {
            background: white;
            border-radius: 24px;
            padding: 16px 24px;
            margin-bottom: 28px;
            display: flex;
            flex-wrap: wrap;
            align-items: flex-end;
            gap: 20px;
            box-shadow: 0 2px 6px rgba(0,0,0,0.03);
            border: 1px solid #e2e8f0;
        }

        .filter-group {
            display: flex;
            flex-direction: column;
            gap: 6px;
            min-width: 160px;
        }

        .filter-group label {
            font-size: 0.75rem;
            font-weight: 600;
            text-transform: uppercase;
            color: #475569;
        }

        select, input {
            padding: 8px 12px;
            border-radius: 16px;
            border: 1px solid #cbd5e1;
            background: white;
            font-size: 0.85rem;
        }

        .btn-reset {
            background: #f1f5f9;
            border: 1px solid #cbd5e1;
            padding: 8px 20px;
            border-radius: 30px;
            cursor: pointer;
            font-weight: 500;
            transition: 0.2s;
        }

        .btn-reset-danger {
            background: #fee2e2;
            border-color: #fecaca;
            color: #b91c1c;
        }

        .btn-reset:hover {
            background: #e2e8f0;
        }

        /* Rekap stok akhir bulan */
        .rekap-section {
            background: white;
            border-radius: 28px;
            padding: 20px 24px;
            margin-top: 20px;
            border: 1px solid #e2e8f0;
            box-shadow: 0 6px 14px rgba(0,0,0,0.03);
        }

        .rekap-header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            flex-wrap: wrap;
            margin-bottom: 20px;
            gap: 15px;
        }

        .rekap-section h3 {
            font-size: 1.3rem;
            font-weight: 700;
            display: flex;
            align-items: center;
            gap: 12px;
            margin: 0;
        }

        .rekap-actions {
            display: flex;
            gap: 12px;
        }

        .btn-excel, .btn-print {
            background: #f1f5f9;
            border: 1px solid #cbd5e1;
            padding: 8px 18px;
            border-radius: 30px;
            cursor: pointer;
            font-weight: 600;
            font-size: 0.8rem;
            display: inline-flex;
            align-items: center;
            gap: 8px;
            transition: 0.2s;
        }

        .btn-excel {
            background: #e8f5e9;
            color: #2e7d32;
            border-color: #a5d6a7;
        }

        .btn-print {
            background: #e3f2fd;
            color: #1565c0;
            border-color: #90caf9;
        }

        .btn-excel:hover, .btn-print:hover {
            transform: translateY(-1px);
            filter: brightness(0.97);
        }

        .rekap-grid {
            overflow-x: auto;
        }

        .badge {
            background: #eef2ff;
            padding: 4px 10px;
            border-radius: 40px;
            font-size: 0.7rem;
        }

        @media (max-width: 780px) {
            body { padding: 16px; }
            .panel-header h2 { font-size: 1.2rem; }
        }
        .text-center { text-align: center; }
        .small-note { font-size: 0.7rem; color: #5b6e8c; margin-top: 6px; }
        
        /* Print styling */
        @media print {
            body {
                background: white;
                padding: 0;
                margin: 0;
            }
            .filter-bar, .two-panels, .btn-add, .btn-reset, .btn-excel, .btn-print, .rekap-actions, .action-icons, .panel-header button, #resetPermanenBtn, .btn-reset-danger, .filter-group {
                display: none !important;
            }
            .rekap-section {
                box-shadow: none;
                padding: 0;
                margin: 0;
                border: none;
            }
            .rekap-grid table {
                width: 100%;
                border: 1px solid #ddd;
            }
            th, td {
                border: 1px solid #ccc;
            }
            h1, h3 {
                color: black;
            }
            .container {
                padding: 10px;
            }
        }
    </style>
</head>
<body>
<div class="container">
    <div class="dashboard-header">
        <h1><i class="fas fa-boxes"></i> Manajemen Inventory</h1>
        <div class="sub">Data Barang Masuk Supplier & Stok Consumable + Rekap Bulanan | Cetak & Export Excel</div>
    </div>

    <!-- Filter & Dropdowns + Reset Permanen -->
    <div class="filter-bar">
        <div class="filter-group">
            <label><i class="fas fa-filter"></i> Filter Nama Barang</label>
            <input type="text" id="globalSearchBarang" placeholder="Cari nama barang..." style="width:200px;">
        </div>
        <div class="filter-group">
            <label>Dropdown Nama Barang</label>
            <select id="dropdownNamaBarang"></select>
        </div>
        <div class="filter-group">
            <label>Dropdown Supplier</label>
            <select id="dropdownSupplier"></select>
        </div>
        <div class="filter-group">
            <label>Dropdown Satuan</label>
            <select id="dropdownSatuan"></select>
        </div>
        <div class="filter-group">
            <label>&nbsp;</label>
            <button id="resetPermanenBtn" class="btn-reset btn-reset-danger"><i class="fas fa-database"></i> Reset Semua Data</button>
        </div>
    </div>

    <!-- Dua Panel Utama -->
    <div class="two-panels">
        <!-- PANEL 1: Data Barang Masuk Supplier -->
        <div class="panel">
            <div class="panel-header">
                <h2><i class="fas fa-truck"></i> Data Barang Masuk Supplier</h2>
                <button class="btn-add" id="btnAddMasuk"><i class="fas fa-plus"></i> Tambah Masuk</button>
            </div>
            <div class="table-wrapper">
                <table id="tableMasuk">
                    <thead>
                        <tr><th>Tanggal</th><th>Nama Barang</th><th>Supplier</th><th>Jumlah Masuk</th><th>Satuan</th><th>Aksi</th></tr>
                    </thead>
                    <tbody id="tbodyMasuk"></tbody>
                </table>
            </div>
        </div>

        <!-- PANEL 2: Data Stock Barang Consumable -->
        <div class="panel">
            <div class="panel-header">
                <h2><i class="fas fa-clipboard-list"></i> Stock Barang Consumable</h2>
                <button class="btn-add" id="btnAddKeluar"><i class="fas fa-plus"></i> Tambah Keluar</button>
            </div>
            <div class="table-wrapper">
                <table id="tableKeluar">
                    <thead><tr><th>Total Stok Masuk</th><th>Tanggal Keluar</th><th>Barang Keluar</th><th>Jumlah Keluar</th><th>Satuan</th><th>Nama PIC</th><th>Aksi</th></tr></thead>
                    <tbody id="tbodyKeluar"></tbody>
                </table>
            </div>
        </div>
    </div>

    <!-- REKAP STOK AKHIR PER BULAN + TOMBOL PRINT & EXCEL -->
    <div class="rekap-section" id="rekapSection">
        <div class="rekap-header">
            <h3><i class="fas fa-chart-line"></i> Rekap Stok Akhir per Bulan (Mutasi Stok)</h3>
            <div class="rekap-actions">
                <button class="btn-excel" id="exportExcelBtn"><i class="fas fa-file-excel"></i> Export ke Excel</button>
                <button class="btn-print" id="printRekapBtn"><i class="fas fa-print"></i> Cetak Rekap</button>
            </div>
        </div>
        <div class="rekap-grid" id="rekapContainer"></div>
        <div class="small-note">* Perhitungan: Total Barang Masuk (per barang) dikurangi total barang keluar = Stok Akhir. Ditampilkan per barang + rincian tanggal keluar & satuan.</div>
    </div>
</div>

<script>
    // ---------- DATA MODEL ----------
    let dataMasuk = [];     // { id, tanggal, namaBarang, supplier, jumlah, satuan }
    let dataKeluar = [];    // { id, tanggalKeluar, barangKeluar, jumlahKeluar, satuan, namaPic }

    // Load dari localStorage atau data default
    function loadFromStorage() {
        const storedMasuk = localStorage.getItem('inv_masuk');
        const storedKeluar = localStorage.getItem('inv_keluar');
        if(storedMasuk) dataMasuk = JSON.parse(storedMasuk);
        else {
            dataMasuk = [
                { id: 1, tanggal: '2025-03-10', namaBarang: 'Kertas A4', supplier: 'PT Maju Jaya', jumlah: 500, satuan: 'Rim' },
                { id: 2, tanggal: '2025-03-15', namaBarang: 'Tinta Hitam', supplier: 'Sumber Makmur', jumlah: 20, satuan: 'Botol' },
                { id: 3, tanggal: '2025-04-05', namaBarang: 'Kertas A4', supplier: 'PT Maju Jaya', jumlah: 300, satuan: 'Rim' },
                { id: 4, tanggal: '2025-04-12', namaBarang: 'Spidol Boardmarker', supplier: 'Alat Kantor', jumlah: 50, satuan: 'Pcs' }
            ];
        }
        if(storedKeluar) dataKeluar = JSON.parse(storedKeluar);
        else {
            dataKeluar = [
                { id: 101, tanggalKeluar: '2025-03-20', barangKeluar: 'Kertas A4', jumlahKeluar: 200, satuan: 'Rim', namaPic: 'Andi' },
                { id: 102, tanggalKeluar: '2025-03-28', barangKeluar: 'Tinta Hitam', jumlahKeluar: 5, satuan: 'Botol', namaPic: 'Budi' },
                { id: 103, tanggalKeluar: '2025-04-10', barangKeluar: 'Kertas A4', jumlahKeluar: 150, satuan: 'Rim', namaPic: 'Citra' },
                { id: 104, tanggalKeluar: '2025-04-18', barangKeluar: 'Spidol Boardmarker', jumlahKeluar: 10, satuan: 'Pcs', namaPic: 'Dewi' }
            ];
        }
    }

    function saveToStorage() {
        localStorage.setItem('inv_masuk', JSON.stringify(dataMasuk));
        localStorage.setItem('inv_keluar', JSON.stringify(dataKeluar));
    }

    // Helper: mendapatkan total stok masuk per barang
    function getTotalMasukPerBarang(namaBarang) {
        return dataMasuk.filter(m => m.namaBarang === namaBarang).reduce((sum, m) => sum + m.jumlah, 0);
    }

    // total keluar per barang
    function getTotalKeluarPerBarang(namaBarang) {
        return dataKeluar.filter(k => k.barangKeluar === namaBarang).reduce((sum, k) => sum + k.jumlahKeluar, 0);
    }

    // Ambil data rekap dalam bentuk array objek untuk export excel
    function getRekapDataArray() {
        const uniqueBarangSet = new Set();
        dataMasuk.forEach(m => uniqueBarangSet.add(m.namaBarang));
        dataKeluar.forEach(k => uniqueBarangSet.add(k.barangKeluar));
        const uniqueBarang = Array.from(uniqueBarangSet);
        
        const rekapData = [];
        for(let barang of uniqueBarang) {
            const totalMasuk = getTotalMasukPerBarang(barang);
            const totalKeluar = getTotalKeluarPerBarang(barang);
            const stokAkhir = totalMasuk - totalKeluar;
            const riwayatKeluar = dataKeluar.filter(k => k.barangKeluar === barang);
            let rincianText = riwayatKeluar.map(k => `${k.tanggalKeluar} : ${k.jumlahKeluar} ${k.satuan} (PIC: ${k.namaPic})`).join("; ");
            if(rincianText === "") rincianText = "Tidak ada pengambilan";
            
            rekapData.push({
                "Nama Barang": barang,
                "Total Masuk": totalMasuk,
                "Total Keluar": totalKeluar,
                "Stok Akhir": stokAkhir,
                "Rincian Pengambilan (Tgl, Jml, Satuan, PIC)": rincianText
            });
        }
        return rekapData;
    }

    // Export ke Excel menggunakan SheetJS
    function exportToExcel() {
        const rekapArray = getRekapDataArray();
        if(rekapArray.length === 0) {
            alert("Tidak ada data rekap untuk diexport.");
            return;
        }
        // Tambahkan metadata sheet
        const ws = XLSX.utils.json_to_sheet(rekapArray);
        // Atur lebar kolom (optional)
        ws['!cols'] = [{wch:25},{wch:12},{wch:12},{wch:12},{wch:50}];
        const wb = XLSX.utils.book_new();
        XLSX.utils.book_append_sheet(wb, ws, "Rekap_Stok_Akhir");
        // Generate file
        XLSX.writeFile(wb, `Rekap_Stok_${new Date().toISOString().slice(0,19).replace(/:/g, '-')}.xlsx`);
    }

    // Cetak rekap stok (hanya bagian rekap)
    function printRekapOnly() {
        const rekapContainer = document.getElementById('rekapContainer');
        const originalTitle = document.title;
        // Ambil copy dari konten rekap yang sudah di-render
        const rekapContent = rekapContainer.cloneNode(true);
        // buat elemen sementara untuk print
        const printWindow = window.open('', '_blank');
        printWindow.document.write(`
            <html>
            <head>
                <title>Laporan Rekap Stok Akhir Bulanan</title>
                <style>
                    body { font-family: 'Inter', Arial, sans-serif; padding: 20px; margin: 0; }
                    table { width: 100%; border-collapse: collapse; margin-top: 16px; }
                    th, td { border: 1px solid #aaa; padding: 8px 10px; text-align: left; }
                    th { background-color: #f2f2f2; }
                    h2 { color: #1e3c72; }
                    .header-print { margin-bottom: 20px; border-bottom: 2px solid #333; }
                    .small-note { margin-top: 20px; font-size: 11px; color: gray; }
                </style>
            </head>
            <body>
                <div class="header-print">
                    <h2>Laporan Rekap Stok Akhir per Bulan</h2>
                    <p>Tanggal cetak: ${new Date().toLocaleString()}</p>
                </div>
                ${rekapContent.outerHTML}
                <div class="small-note">* Stok Akhir = Total Barang Masuk - Total Barang Keluar. Rincian pengambilan disertakan.</div>
            </body>
            </html>
        `);
        printWindow.document.close();
        printWindow.print();
        printWindow.onafterprint = function() { printWindow.close(); };
    }

    // Build rekap HTML untuk ditampilkan (digunakan juga oleh print)
    function buildRekapStok() {
        const uniqueBarangSet = new Set();
        dataMasuk.forEach(m => uniqueBarangSet.add(m.namaBarang));
        dataKeluar.forEach(k => uniqueBarangSet.add(k.barangKeluar));
        const uniqueBarang = Array.from(uniqueBarangSet);
        
        let rekapHtml = '<table style="width:100%; border-collapse:collapse;">';
        rekapHtml += `<thead><tr style="background:#f1f5f9;"><th>Nama Barang</th><th>Total Masuk</th><th>Total Keluar</th><th>Stok Akhir</th><th>Rincian Pengambilan (Tgl Keluar, Satuan, Jml, PIC)</th></tr></thead><tbody>`;
        
        for(let barang of uniqueBarang) {
            const totalMasuk = getTotalMasukPerBarang(barang);
            const totalKeluar = getTotalKeluarPerBarang(barang);
            const stokAkhir = totalMasuk - totalKeluar;
            const riwayatKeluar = dataKeluar.filter(k => k.barangKeluar === barang);
            let rincianText = '';
            if(riwayatKeluar.length === 0) rincianText = '<span class="badge">Tidak ada pengambilan</span>';
            else {
                rincianText = riwayatKeluar.map(k => `${k.tanggalKeluar} : ${k.jumlahKeluar} ${k.satuan} (PIC: ${escapeHtml(k.namaPic)})`).join('<br/>');
            }
            rekapHtml += `<tr style="border-bottom:1px solid #e2e8f0;">
                            <td style="padding:10px 6px; font-weight:600;">${escapeHtml(barang)}</td>
                            <td style="padding:10px 6px;">${totalMasuk}</td>
                            <td style="padding:10px 6px;">${totalKeluar}</td>
                            <td style="background:#fef9e3; font-weight:bold; padding:10px 6px;">${stokAkhir}</td>
                            <td style="font-size:0.75rem; padding:10px 6px;">${rincianText}</td>
                           </tr>`;
        }
        rekapHtml += `</tbody></table>`;
        if(uniqueBarang.length===0) rekapHtml = '<p class="small-note">Belum ada data barang. Silakan tambahkan barang masuk terlebih dahulu.</p>';
        document.getElementById('rekapContainer').innerHTML = rekapHtml;
    }

    // Render Tabel Masuk (dengan filter nama barang)
    function renderTabelMasuk() {
        const searchTerm = document.getElementById('globalSearchBarang').value.toLowerCase();
        let filtered = [...dataMasuk];
        if(searchTerm) {
            filtered = filtered.filter(m => m.namaBarang.toLowerCase().includes(searchTerm));
        }
        const tbody = document.getElementById('tbodyMasuk');
        if(filtered.length === 0) {
            tbody.innerHTML = '<tr><td colspan="6" class="text-center">Tidak ada data</td></tr>';
            return;
        }
        tbody.innerHTML = filtered.map(m => `
            <tr>
                <td>${escapeHtml(m.tanggal)}</td>
                <td>${escapeHtml(m.namaBarang)}</td>
                <td>${escapeHtml(m.supplier)}</td>
                <td>${m.jumlah}</td>
                <td>${escapeHtml(m.satuan)}</td>
                <td class="action-icons">
                    <i class="fas fa-edit" onclick="editMasuk(${m.id})"></i>
                    <i class="fas fa-trash-alt" onclick="hapusMasuk(${m.id})"></i>
                </td>
            </tr>
        `).join('');
    }

    function renderTabelKeluar() {
        const searchTerm = document.getElementById('globalSearchBarang').value.toLowerCase();
        let filtered = [...dataKeluar];
        if(searchTerm) {
            filtered = filtered.filter(k => k.barangKeluar.toLowerCase().includes(searchTerm));
        }
        const tbody = document.getElementById('tbodyKeluar');
        if(filtered.length===0) {
            tbody.innerHTML = '<tr><td colspan="7" class="text-center">Tidak ada data keluar</td></tr>';
            return;
        }
        tbody.innerHTML = filtered.map(k => {
            const totalStokBarang = getTotalMasukPerBarang(k.barangKeluar);
            return `
            <tr>
                <td>${totalStokBarang}</td>
                <td>${escapeHtml(k.tanggalKeluar)}</td>
                <td>${escapeHtml(k.barangKeluar)}</td>
                <td>${k.jumlahKeluar}</td>
                <td>${escapeHtml(k.satuan)}</td>
                <td>${escapeHtml(k.namaPic)}</td>
                <td class="action-icons">
                    <i class="fas fa-edit" onclick="editKeluar(${k.id})"></i>
                    <i class="fas fa-trash-alt" onclick="hapusKeluar(${k.id})"></i>
                </td>
            </table>
        `}).join('');
    }

    // Update seluruh UI + dropdown dinamis
    function refreshAll() {
        renderTabelMasuk();
        renderTabelKeluar();
        buildRekapStok();
        updateDropdowns();
        saveToStorage();
    }

    // update dropdown dari data unik
    function updateDropdowns() {
        const uniqueBarang = [...new Set([...dataMasuk.map(m=>m.namaBarang), ...dataKeluar.map(k=>k.barangKeluar)])];
        const uniqueSupplier = [...new Set(dataMasuk.map(m=>m.supplier))];
        const uniqueSatuan = [...new Set([...dataMasuk.map(m=>m.satuan), ...dataKeluar.map(k=>k.satuan)])];
        
        const dropdownBarang = document.getElementById('dropdownNamaBarang');
        dropdownBarang.innerHTML = '<option value="">-- Semua --</option>' + uniqueBarang.map(b=>`<option value="${escapeHtml(b)}">${escapeHtml(b)}</option>`).join('');
        dropdownBarang.onchange = (e) => {
            const val = e.target.value;
            if(val) document.getElementById('globalSearchBarang').value = val;
            else document.getElementById('globalSearchBarang').value = '';
            refreshAll();
        };
        
        const dropdownSupplier = document.getElementById('dropdownSupplier');
        dropdownSupplier.innerHTML = '<option value="">-- Pilih Supplier (referensi) --</option>' + uniqueSupplier.map(s=>`<option value="${escapeHtml(s)}">${escapeHtml(s)}</option>`).join('');
        dropdownSupplier.onchange = () => {};
        
        const dropdownSatuan = document.getElementById('dropdownSatuan');
        dropdownSatuan.innerHTML = '<option value="">-- Pilih Satuan --</option>' + uniqueSatuan.map(s=>`<option value="${escapeHtml(s)}">${escapeHtml(s)}</option>`).join('');
    }

    // CRUD Masuk
    function editMasuk(id) {
        const item = dataMasuk.find(m => m.id === id);
        if(!item) return;
        const newTanggal = prompt("Edit Tanggal (YYYY-MM-DD):", item.tanggal);
        if(newTanggal !== null) item.tanggal = newTanggal;
        const newNama = prompt("Nama Barang:", item.namaBarang);
        if(newNama !== null) item.namaBarang = newNama;
        const newSupplier = prompt("Supplier:", item.supplier);
        if(newSupplier !== null) item.supplier = newSupplier;
        const newJumlah = prompt("Jumlah Masuk:", item.jumlah);
        if(newJumlah !== null && !isNaN(parseInt(newJumlah))) item.jumlah = parseInt(newJumlah);
        const newSatuan = prompt("Satuan:", item.satuan);
        if(newSatuan !== null) item.satuan = newSatuan;
        refreshAll();
    }
    function hapusMasuk(id) {
        if(confirm("Hapus data barang masuk?")) {
            dataMasuk = dataMasuk.filter(m => m.id !== id);
            refreshAll();
        }
    }
    function tambahMasuk() {
        let newId = dataMasuk.length ? Math.max(...dataMasuk.map(m=>m.id)) + 1 : 1;
        let tanggal = prompt("Tanggal (YYYY-MM-DD):", new Date().toISOString().slice(0,10));
        if(!tanggal) return;
        let namaBarang = prompt("Nama Barang:");
        if(!namaBarang) return;
        let supplier = prompt("Supplier:");
        if(!supplier) return;
        let jumlah = parseInt(prompt("Jumlah Masuk:","0"));
        if(isNaN(jumlah)) jumlah = 0;
        let satuan = prompt("Satuan (contoh: Pcs, Rim, Botol):","Pcs");
        dataMasuk.push({ id: newId, tanggal, namaBarang, supplier, jumlah, satuan });
        refreshAll();
    }

    // CRUD Keluar
    function editKeluar(id) {
        const item = dataKeluar.find(k => k.id === id);
        if(!item) return;
        let newTgl = prompt("Tanggal Keluar (YYYY-MM-DD):", item.tanggalKeluar);
        if(newTgl !== null) item.tanggalKeluar = newTgl;
        let newBarang = prompt("Barang Keluar:", item.barangKeluar);
        if(newBarang !== null) item.barangKeluar = newBarang;
        let newJml = prompt("Jumlah Keluar:", item.jumlahKeluar);
        if(newJml !== null && !isNaN(parseInt(newJml))) item.jumlahKeluar = parseInt(newJml);
        let newSatuan = prompt("Satuan:", item.satuan);
        if(newSatuan !== null) item.satuan = newSatuan;
        let newPic = prompt("Nama PIC:", item.namaPic);
        if(newPic !== null) item.namaPic = newPic;
        refreshAll();
    }
    function hapusKeluar(id) {
        if(confirm("Hapus data barang keluar?")) {
            dataKeluar = dataKeluar.filter(k => k.id !== id);
            refreshAll();
        }
    }
    function tambahKeluar() {
        let newId = dataKeluar.length ? Math.max(...dataKeluar.map(k=>k.id)) + 1 : 201;
        let tglKeluar = prompt("Tanggal Keluar (YYYY-MM-DD):", new Date().toISOString().slice(0,10));
        if(!tglKeluar) return;
        let barangKeluar = prompt("Nama Barang Keluar:");
        if(!barangKeluar) return;
        let jmlKeluar = parseInt(prompt("Jumlah Keluar:","1"));
        if(isNaN(jmlKeluar)) jmlKeluar=0;
        let satuan = prompt("Satuan:", "Pcs");
        let pic = prompt("Nama PIC:", "");
        if(pic === null) pic = "";
        dataKeluar.push({ id: newId, tanggalKeluar: tglKeluar, barangKeluar, jumlahKeluar: jmlKeluar, satuan, namaPic: pic });
        refreshAll();
    }
    
    // RESET PERMANEN
    function resetDataPermanen() {
        if(confirm("⚠️ PERINGATAN: Ini akan menghapus SEMUA data barang masuk dan barang keluar secara permanen! Lanjutkan?")) {
            dataMasuk = [];
            dataKeluar = [];
            refreshAll();
            alert("Semua data telah direset permanen.");
        }
    }

    function escapeHtml(str) {
        if(!str) return '';
        return str.replace(/[&<>]/g, function(m) {
            if(m === '&') return '&amp;';
            if(m === '<') return '&lt;';
            if(m === '>') return '&gt;';
            return m;
        });
    }

    // Event binding & init
    window.onload = () => {
        loadFromStorage();
        refreshAll();
        document.getElementById('btnAddMasuk').onclick = tambahMasuk;
        document.getElementById('btnAddKeluar').onclick = tambahKeluar;
        document.getElementById('resetPermanenBtn').onclick = resetDataPermanen;
        document.getElementById('exportExcelBtn').onclick = exportToExcel;
        document.getElementById('printRekapBtn').onclick = printRekapOnly;
        
        const searchInput = document.getElementById('globalSearchBarang');
        searchInput.addEventListener('input', () => refreshAll());
        
        // Tombol reset filter tambahan
        const filterBar = document.querySelector('.filter-bar');
        const resetFilterQuick = document.createElement('button');
        resetFilterQuick.innerText = 'Reset Pencarian';
        resetFilterQuick.className = 'btn-reset';
        resetFilterQuick.style.marginLeft = 'auto';
        resetFilterQuick.onclick = () => {
            document.getElementById('globalSearchBarang').value = '';
            document.getElementById('dropdownNamaBarang').value = '';
            refreshAll();
        };
        filterBar.appendChild(resetFilterQuick);
    };
    
    // expose global functions for inline onclick
    window.editMasuk = editMasuk;
    window.hapusMasuk = hapusMasuk;
    window.editKeluar = editKeluar;
    window.hapusKeluar = hapusKeluar;
</script>
</body>
</html>
