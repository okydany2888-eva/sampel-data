<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, user-scalable=yes, viewport-fit=cover">
    <title>Inventory Management - Multi Menu Dashboard</title>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0-beta3/css/all.min.css">
    <link href="https://fonts.googleapis.com/css2?family=Inter:opsz,wght@14..32,400;500;600;700&display=swap" rel="stylesheet">
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
            color: #0f172a;
        }

        /* ========= SIDEBAR MENU ========= */
        .app-container {
            display: flex;
            min-height: 100vh;
        }

        /* Sidebar */
        .sidebar-menu {
            width: 280px;
            background: linear-gradient(180deg, #0f172a 0%, #1e1b4b 100%);
            color: #e2e8f0;
            position: fixed;
            left: 0;
            top: 0;
            height: 100vh;
            overflow-y: auto;
            transition: transform 0.3s ease;
            z-index: 1000;
        }

        .sidebar-header {
            padding: 24px 20px;
            border-bottom: 1px solid rgba(255,255,255,0.1);
            margin-bottom: 20px;
        }

        .sidebar-header h2 {
            display: flex;
            align-items: center;
            gap: 12px;
            font-size: 1.2rem;
            color: white;
        }

        .nav-menu {
            padding: 0 12px;
        }

        .menu-item {
            display: flex;
            align-items: center;
            gap: 14px;
            padding: 12px 16px;
            margin-bottom: 6px;
            border-radius: 14px;
            cursor: pointer;
            transition: all 0.2s;
            color: #cbd5e1;
        }

        .menu-item:hover {
            background: rgba(255,255,255,0.08);
            color: white;
        }

        .menu-item.active {
            background: linear-gradient(135deg, #3b82f6 0%, #8b5cf6 100%);
            color: white;
        }

        .menu-icon {
            width: 32px;
            font-size: 1.2rem;
            text-align: center;
        }

        .menu-text {
            font-size: 0.9rem;
            font-weight: 500;
        }

        /* Tombol Hamburger untuk mobile */
        .hamburger {
            display: none;
            position: fixed;
            top: 16px;
            left: 16px;
            z-index: 1001;
            background: #0f172a;
            border: none;
            color: white;
            width: 44px;
            height: 44px;
            border-radius: 12px;
            font-size: 1.3rem;
            cursor: pointer;
            box-shadow: 0 2px 8px rgba(0,0,0,0.15);
        }

        /* Overlay untuk mobile */
        .overlay {
            display: none;
            position: fixed;
            top: 0;
            left: 0;
            right: 0;
            bottom: 0;
            background: rgba(0,0,0,0.5);
            z-index: 999;
        }

        /* ========= KONTEN UTAMA ========= */
        .main-content {
            flex: 1;
            margin-left: 280px;
            padding: 20px;
            transition: margin-left 0.3s ease;
        }

        .page-title {
            margin-bottom: 24px;
        }

        .page-title h1 {
            font-size: 1.5rem;
            color: #0f172a;
            display: flex;
            align-items: center;
            gap: 10px;
        }

        /* Card Container */
        .card {
            background: white;
            border-radius: 20px;
            border: 1px solid #e2e8f0;
            overflow: hidden;
            margin-bottom: 24px;
        }

        .card-header {
            padding: 16px 20px;
            background: #fafcff;
            border-bottom: 1px solid #eef2ff;
            display: flex;
            justify-content: space-between;
            align-items: center;
            flex-wrap: wrap;
            gap: 12px;
        }

        .card-header h2 {
            font-size: 1rem;
            font-weight: 700;
            color: #0f3b5c;
            display: flex;
            align-items: center;
            gap: 8px;
        }

        .btn-add {
            background: linear-gradient(135deg, #10b981 0%, #059669 100%);
            border: none;
            color: white;
            padding: 8px 16px;
            border-radius: 12px;
            font-weight: 600;
            font-size: 0.75rem;
            cursor: pointer;
            display: flex;
            align-items: center;
            gap: 6px;
        }

        .btn-add-keluar {
            background: linear-gradient(135deg, #f59e0b 0%, #d97706 100%);
        }

        /* Form Input */
        .input-form {
            padding: 20px;
            background: #f8fafc;
            border-bottom: 1px solid #e2e8f0;
            display: none;
        }

        .input-form.show {
            display: block;
        }

        .form-row {
            display: flex;
            flex-wrap: wrap;
            gap: 12px;
            align-items: flex-end;
        }

        .form-group {
            flex: 1;
            min-width: 120px;
        }

        .form-group label {
            font-size: 0.7rem;
            font-weight: 600;
            color: #475569;
            display: block;
            margin-bottom: 4px;
        }

        .form-group input, .form-group select {
            width: 100%;
            padding: 8px 10px;
            border-radius: 12px;
            border: 1px solid #cbd5e1;
            font-size: 0.75rem;
            background: white;
        }

        .btn-submit {
            background: #10b981;
            color: white;
            border: none;
            padding: 8px 20px;
            border-radius: 12px;
            cursor: pointer;
            font-weight: 600;
            font-size: 0.75rem;
        }

        /* Tabel */
        .table-wrapper {
            overflow-x: auto;
            padding: 0 16px 16px 16px;
        }

        table {
            width: 100%;
            border-collapse: collapse;
            font-size: 0.75rem;
            min-width: 600px;
        }

        th, td {
            padding: 10px 8px;
            text-align: left;
            border-bottom: 1px solid #e9eef3;
        }

        th {
            background-color: #f8fafc;
            font-weight: 600;
            color: #1e293b;
        }

        .action-icons i {
            margin: 0 4px;
            cursor: pointer;
            font-size: 0.85rem;
        }

        .fa-edit { color: #3b82f6; }
        .fa-trash-alt { color: #ef4444; }

        /* Rekap Stok dengan Kartu */
        .rekap-container {
            padding: 16px;
        }

        .rekap-card {
            background: #f8fafc;
            border-radius: 16px;
            padding: 14px;
            margin-bottom: 12px;
            border-left: 4px solid #3b82f6;
        }

        .rekap-brand {
            font-weight: 700;
            font-size: 0.9rem;
            margin-bottom: 8px;
        }

        .rekap-stats {
            display: flex;
            flex-wrap: wrap;
            gap: 12px;
            margin-bottom: 10px;
        }

        .badge {
            background: #eef2ff;
            padding: 4px 12px;
            border-radius: 20px;
            font-size: 0.7rem;
            font-weight: 500;
        }

        .rekap-stok-akhir {
            font-weight: 800;
            font-size: 1rem;
            color: #0f3b5c;
        }

        .rekap-detail {
            font-size: 0.7rem;
            color: #475569;
            margin-top: 8px;
        }

        .search-bar {
            display: flex;
            gap: 10px;
            align-items: center;
            background: #f1f5f9;
            padding: 8px 16px;
            border-radius: 40px;
            margin-bottom: 16px;
        }

        .search-bar input {
            flex: 1;
            border: none;
            background: transparent;
            outline: none;
            font-size: 0.8rem;
        }

        /* Responsive */
        @media (max-width: 768px) {
            .sidebar-menu {
                transform: translateX(-100%);
                width: 260px;
            }
            .sidebar-menu.open {
                transform: translateX(0);
            }
            .main-content {
                margin-left: 0;
                padding: 16px;
                padding-top: 60px;
            }
            .hamburger {
                display: flex;
                align-items: center;
                justify-content: center;
            }
            .overlay.show {
                display: block;
            }
        }
    </style>
</head>
<body>
<div class="app-container">
    <!-- Tombol Hamburger -->
    <button class="hamburger" id="hamburgerBtn">☰</button>
    <div class="overlay" id="overlay"></div>

    <!-- Sidebar Menu -->
    <div class="sidebar-menu" id="sidebar">
        <div class="sidebar-header">
            <h2><i class="fas fa-boxes"></i> Inventory Pro</h2>
        </div>
        <div class="nav-menu">
            <div class="menu-item active" data-menu="beranda">
                <div class="menu-icon"><i class="fas fa-home"></i></div>
                <div class="menu-text">🏠 Beranda</div>
            </div>
            <div class="menu-item" data-menu="barang-masuk">
                <div class="menu-icon"><i class="fas fa-arrow-down"></i></div>
                <div class="menu-text">📦 Barang Masuk</div>
            </div>
            <div class="menu-item" data-menu="barang-keluar">
                <div class="menu-icon"><i class="fas fa-arrow-up"></i></div>
                <div class="menu-text">📤 Barang Keluar</div>
            </div>
            <div class="menu-item" data-menu="rekap-stok">
                <div class="menu-icon"><i class="fas fa-chart-line"></i></div>
                <div class="menu-text">📊 Rekap Stok</div>
            </div>
            <div class="menu-item" data-menu="laporan">
                <div class="menu-icon"><i class="fas fa-print"></i></div>
                <div class="menu-text">📄 Laporan</div>
            </div>
        </div>
    </div>

    <!-- Konten Utama -->
    <div class="main-content" id="mainContent">
        <div class="page-title">
            <h1 id="mainPageTitle"><i class="fas fa-home"></i> Beranda</h1>
        </div>

        <!-- ========= MENU BERANDA ========= -->
        <div id="menu-beranda" class="menu-page">
            <div class="card">
                <div class="card-header">
                    <h2><i class="fas fa-chart-simple"></i> Ringkasan Inventory</h2>
                </div>
                <div class="rekap-container" id="ringkasanDashboard">
                    <!-- Ringkasan akan diisi JS -->
                </div>
            </div>
        </div>

        <!-- ========= MENU BARANG MASUK ========= -->
        <div id="menu-barang-masuk" class="menu-page" style="display:none;">
            <div class="card">
                <div class="card-header">
                    <h2><i class="fas fa-truck"></i> Data Barang Masuk</h2>
                    <button class="btn-add" id="toggleFormMasukBtn"><i class="fas fa-plus"></i> Tambah Barang</button>
                </div>
                <div class="input-form" id="formBarangMasuk">
                    <div class="form-row">
                        <div class="form-group"><label>Tanggal</label><input type="date" id="tglMasuk"></div>
                        <div class="form-group"><label>Brand</label><input type="text" id="brandMasuk" placeholder="Brand"></div>
                        <div class="form-group"><label>Nama Barang</label><input type="text" id="namaMasuk" placeholder="Nama barang"></div>
                        <div class="form-group"><label>Supplier</label><input type="text" id="supplierMasuk" placeholder="Supplier"></div>
                        <div class="form-group"><label>Jumlah</label><input type="number" id="jumlahMasuk" value="1"></div>
                        <div class="form-group"><label>Satuan</label><input type="text" id="satuanMasuk" placeholder="pcs/kg"></div>
                        <div class="form-group"><button class="btn-submit" id="simpanMasukBtn"><i class="fas fa-save"></i> Simpan</button></div>
                    </div>
                </div>
                <div class="table-wrapper">
                    <table id="tableMasuk">
                        <thead><tr><th>Tanggal</th><th>Brand</th><th>Nama Barang</th><th>Supplier</th><th>Jumlah</th><th>Satuan</th><th>Aksi</th></tr></thead>
                        <tbody id="tbodyMasuk"></tbody>
                    </table>
                </div>
            </div>
        </div>

        <!-- ========= MENU BARANG KELUAR ========= -->
        <div id="menu-barang-keluar" class="menu-page" style="display:none;">
            <div class="card">
                <div class="card-header">
                    <h2><i class="fas fa-clipboard-list"></i> Data Barang Keluar</h2>
                    <button class="btn-add btn-add-keluar" id="toggleFormKeluarBtn"><i class="fas fa-plus"></i> Tambah Keluar</button>
                </div>
                <div class="input-form" id="formBarangKeluar">
                    <div class="form-row">
                        <div class="form-group"><label>Tanggal</label><input type="date" id="tglKeluar"></div>
                        <div class="form-group"><label>Brand</label><input type="text" id="brandKeluar" placeholder="Brand"></div>
                        <div class="form-group"><label>Nama Barang</label><input type="text" id="namaKeluar" placeholder="Nama barang"></div>
                        <div class="form-group"><label>Jumlah</label><input type="number" id="jumlahKeluar" value="1"></div>
                        <div class="form-group"><label>Satuan</label><input type="text" id="satuanKeluar" placeholder="pcs/kg"></div>
                        <div class="form-group"><label>PIC</label><input type="text" id="picKeluar" placeholder="PIC"></div>
                        <div class="form-group"><button class="btn-submit" id="simpanKeluarBtn"><i class="fas fa-save"></i> Simpan</button></div>
                    </div>
                </div>
                <div class="table-wrapper">
                    <table id="tableKeluar">
                        <thead><tr><th>Tanggal</th><th>Brand</th><th>Nama Barang</th><th>Jumlah</th><th>Satuan</th><th>PIC</th><th>Aksi</th></tr></thead>
                        <tbody id="tbodyKeluar"></tbody>
                    </table>
                </div>
            </div>
        </div>

        <!-- ========= MENU REKAP STOK ========= -->
        <div id="menu-rekap-stok" class="menu-page" style="display:none;">
            <div class="card">
                <div class="card-header">
                    <h2><i class="fas fa-chart-line"></i> Rekap Stok Akhir</h2>
                </div>
                <div class="rekap-container">
                    <div class="search-bar">
                        <i class="fas fa-search"></i>
                        <input type="text" id="searchRekap" placeholder="Cari brand atau nama barang...">
                        <button id="clearSearch" style="background:none;border:none;"><i class="fas fa-times-circle"></i></button>
                    </div>
                    <div id="rekapStokList"></div>
                </div>
            </div>
        </div>

        <!-- ========= MENU LAPORAN ========= -->
        <div id="menu-laporan" class="menu-page" style="display:none;">
            <div class="card">
                <div class="card-header">
                    <h2><i class="fas fa-print"></i> Export & Cetak Laporan</h2>
                </div>
                <div class="rekap-container" style="text-align:center; padding: 40px;">
                    <button id="exportExcelBtn" style="background:#10b981; color:white; border:none; padding:12px 24px; border-radius:12px; margin:10px; cursor:pointer;"><i class="fas fa-file-excel"></i> Export ke Excel</button>
                    <button id="printLaporanBtn" style="background:#3b82f6; color:white; border:none; padding:12px 24px; border-radius:12px; margin:10px; cursor:pointer;"><i class="fas fa-print"></i> Cetak Laporan</button>
                </div>
            </div>
        </div>
    </div>
</div>

<script>
    // ========= DATA STORAGE =========
    let dataMasuk = [];
    let dataKeluar = [];

    function loadData() {
        const savedMasuk = localStorage.getItem('menu_barang_masuk');
        const savedKeluar = localStorage.getItem('menu_barang_keluar');
        if(savedMasuk) dataMasuk = JSON.parse(savedMasuk);
        else dataMasuk = [];
        if(savedKeluar) dataKeluar = JSON.parse(savedKeluar);
        else dataKeluar = [];
    }

    function saveData() {
        localStorage.setItem('menu_barang_masuk', JSON.stringify(dataMasuk));
        localStorage.setItem('menu_barang_keluar', JSON.stringify(dataKeluar));
    }

    // Helper
    function getTotalMasuk(brand, nama) {
        return dataMasuk.filter(m => m.brand === brand && m.namaBarang === nama).reduce((s, m) => s + m.jumlah, 0);
    }
    function getTotalKeluar(brand, nama) {
        return dataKeluar.filter(k => k.brand === brand && k.namaBarang === nama).reduce((s, k) => s + k.jumlah, 0);
    }
    function getAllItems() {
        const map = new Map();
        dataMasuk.forEach(m => map.set(`${m.brand}|${m.namaBarang}`, { brand: m.brand, namaBarang: m.namaBarang }));
        dataKeluar.forEach(k => map.set(`${k.brand}|${k.namaBarang}`, { brand: k.brand, namaBarang: k.namaBarang }));
        return Array.from(map.values());
    }

    // Render Tabel
    function renderTabelMasuk() {
        const tbody = document.getElementById('tbodyMasuk');
        if(!dataMasuk.length) { tbody.innerHTML = '<tr><td colspan="7">Belum ada data</td></tr>'; return; }
        tbody.innerHTML = dataMasuk.map(m => `
            <tr>
                <td>${escapeHtml(m.tanggal)}</td>
                <td><strong>${escapeHtml(m.brand)}</strong></td>
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
        const tbody = document.getElementById('tbodyKeluar');
        if(!dataKeluar.length) { tbody.innerHTML = '<tr><td colspan="7">Belum ada data</td></tr>'; return; }
        tbody.innerHTML = dataKeluar.map(k => `
            <tr>
                <td>${escapeHtml(k.tanggal)}</td>
                <td><strong>${escapeHtml(k.brand)}</strong></td>
                <td>${escapeHtml(k.namaBarang)}</td>
                <td>${k.jumlah}</td>
                <td>${escapeHtml(k.satuan)}</td>
                <td>${escapeHtml(k.pic)}</td>
                <td class="action-icons">
                    <i class="fas fa-edit" onclick="editKeluar(${k.id})"></i>
                    <i class="fas fa-trash-alt" onclick="hapusKeluar(${k.id})"></i>
                </td>
            </tr>
        `).join('');
    }

    // Rekap Stok
    function renderRekapStok() {
        const searchTerm = document.getElementById('searchRekap')?.value.toLowerCase() || '';
        let items = getAllItems();
        if(searchTerm) items = items.filter(i => i.brand.toLowerCase().includes(searchTerm) || i.namaBarang.toLowerCase().includes(searchTerm));
        const container = document.getElementById('rekapStokList');
        if(!items.length) { container.innerHTML = '<p class="badge">Tidak ada data</p>'; return; }
        let html = '';
        items.forEach(item => {
            const totalMasuk = getTotalMasuk(item.brand, item.namaBarang);
            const totalKeluar = getTotalKeluar(item.brand, item.namaBarang);
            const stokAkhir = totalMasuk - totalKeluar;
            const riwayatKeluar = dataKeluar.filter(k => k.brand === item.brand && k.namaBarang === item.namaBarang);
            html += `
                <div class="rekap-card">
                    <div class="rekap-brand"><i class="fas fa-box"></i> ${escapeHtml(item.brand)} - ${escapeHtml(item.namaBarang)}</div>
                    <div class="rekap-stats">
                        <span class="badge">📦 Masuk: ${totalMasuk}</span>
                        <span class="badge">📤 Keluar: ${totalKeluar}</span>
                        <span class="rekap-stok-akhir">✅ Stok Akhir: ${stokAkhir}</span>
                    </div>
                    <div class="rekap-detail">
                        <i class="fas fa-history"></i> Riwayat Keluar: ${riwayatKeluar.length ? riwayatKeluar.map(k => `${k.tanggal}: ${k.jumlah} ${k.satuan} (${k.pic})`).join('; ') : 'Tidak ada'}
                    </div>
                </div>
            `;
        });
        container.innerHTML = html;
    }

    // Ringkasan Dashboard
    function renderRingkasan() {
        const items = getAllItems();
        let totalStok = 0;
        let totalTransaksiMasuk = dataMasuk.length;
        let totalTransaksiKeluar = dataKeluar.length;
        items.forEach(item => {
            totalStok += (getTotalMasuk(item.brand, item.namaBarang) - getTotalKeluar(item.brand, item.namaBarang));
        });
        document.getElementById('ringkasanDashboard').innerHTML = `
            <div style="display:grid; grid-template-columns:repeat(auto-fit,minmax(200px,1fr)); gap:16px;">
                <div class="rekap-card"><i class="fas fa-database"></i> Total Item: ${items.length}</div>
                <div class="rekap-card"><i class="fas fa-arrow-down"></i> Transaksi Masuk: ${totalTransaksiMasuk}</div>
                <div class="rekap-card"><i class="fas fa-arrow-up"></i> Transaksi Keluar: ${totalTransaksiKeluar}</div>
                <div class="rekap-card"><i class="fas fa-chart-line"></i> Total Stok: ${totalStok}</div>
            </div>
        `;
    }

    // CRUD
    function addBarangMasuk() {
        let tanggal = document.getElementById('tglMasuk').value;
        let brand = document.getElementById('brandMasuk').value.trim();
        let namaBarang = document.getElementById('namaMasuk').value.trim();
        let supplier = document.getElementById('supplierMasuk').value.trim();
        let jumlah = parseInt(document.getElementById('jumlahMasuk').value);
        let satuan = document.getElementById('satuanMasuk').value.trim();

        if(!tanggal || !brand || !namaBarang || !supplier || !jumlah || !satuan) {
            alert('Semua field harus diisi!');
            return;
        }

        const newId = dataMasuk.length ? Math.max(...dataMasuk.map(m=>m.id)) + 1 : 1;
        dataMasuk.push({ id: newId, tanggal, brand, namaBarang, supplier, jumlah, satuan });
        saveData();
        renderTabelMasuk();
        renderRekapStok();
        renderRingkasan();
        alert('Barang masuk ditambahkan');
        document.getElementById('formBarangMasuk').classList.remove('show');
    }

    function addBarangKeluar() {
        let tanggal = document.getElementById('tglKeluar').value;
        let brand = document.getElementById('brandKeluar').value.trim();
        let namaBarang = document.getElementById('namaKeluar').value.trim();
        let jumlah = parseInt(document.getElementById('jumlahKeluar').value);
        let satuan = document.getElementById('satuanKeluar').value.trim();
        let pic = document.getElementById('picKeluar').value.trim();

        if(!tanggal || !brand || !namaBarang || !jumlah || !satuan || !pic) {
            alert('Semua field harus diisi!');
            return;
        }

        const stokTersedia = getTotalMasuk(brand, namaBarang) - getTotalKeluar(brand, namaBarang);
        if(stokTersedia < jumlah && !confirm(`Stok tersisa ${stokTersedia} ${satuan}. Tetap catat?`)) return;

        const newId = dataKeluar.length ? Math.max(...dataKeluar.map(k=>k.id)) + 1 : 1;
        dataKeluar.push({ id: newId, tanggal, brand, namaBarang, jumlah, satuan, pic });
        saveData();
        renderTabelKeluar();
        renderRekapStok();
        renderRingkasan();
        alert('Barang keluar dicatat');
        document.getElementById('formBarangKeluar').classList.remove('show');
    }

    function editMasuk(id) { let item = dataMasuk.find(m=>m.id===id); if(item){ let t=prompt("Tanggal",item.tanggal); if(t) item.tanggal=t; let b=prompt("Brand",item.brand); if(b) item.brand=b; let n=prompt("Nama",item.namaBarang); if(n) item.namaBarang=n; let s=prompt("Supplier",item.supplier); if(s) item.supplier=s; let j=parseInt(prompt("Jumlah",item.jumlah)); if(!isNaN(j)) item.jumlah=j; let sat=prompt("Satuan",item.satuan); if(sat) item.satuan=sat; saveData(); renderTabelMasuk(); renderRekapStok(); renderRingkasan(); } }
    function hapusMasuk(id) { if(confirm("Hapus?")) { dataMasuk = dataMasuk.filter(m=>m.id!==id); saveData(); renderTabelMasuk(); renderRekapStok(); renderRingkasan(); } }
    function editKeluar(id) { let item = dataKeluar.find(k=>k.id===id); if(item){ let t=prompt("Tanggal",item.tanggal); if(t) item.tanggal=t; let b=prompt("Brand",item.brand); if(b) item.brand=b; let n=prompt("Nama",item.namaBarang); if(n) item.namaBarang=n; let j=parseInt(prompt("Jumlah",item.jumlah)); if(!isNaN(j)) item.jumlah=j; let sat=prompt("Satuan",item.satuan); if(sat) item.satuan=sat; let p=prompt("PIC",item.pic); if(p) item.pic=p; saveData(); renderTabelKeluar(); renderRekapStok(); renderRingkasan(); } }
    function hapusKeluar(id) { if(confirm("Hapus?")) { dataKeluar = dataKeluar.filter(k=>k.id!==id); saveData(); renderTabelKeluar(); renderRekapStok(); renderRingkasan(); } }

    function exportToExcel() {
        const items = getAllItems();
        const data = items.map(item => ({
            Brand: item.brand, "Nama Barang": item.namaBarang,
            "Total Masuk": getTotalMasuk(item.brand, item.namaBarang),
            "Total Keluar": getTotalKeluar(item.brand, item.namaBarang),
            "Stok Akhir": getTotalMasuk(item.brand, item.namaBarang) - getTotalKeluar(item.brand, item.namaBarang)
        }));
        if(!data.length) { alert("Tidak ada data"); return; }
        const ws = XLSX.utils.json_to_sheet(data);
        const wb = XLSX.utils.book_new();
        XLSX.utils.book_append_sheet(wb, ws, "Rekap_Stok");
        XLSX.writeFile(wb, `Rekap_Stok_${new Date().toISOString().slice(0,10)}.xlsx`);
    }
    function printLaporan() { window.print(); }

    function escapeHtml(str) { if(!str) return ''; return str.replace(/[&<>]/g, m=> m==='&'?'&amp;': m==='<'?'&lt;':'&gt;'); }

    // Menu Navigation
    function showMenu(menuId, title) {
        document.querySelectorAll('.menu-page').forEach(page => page.style.display = 'none');
        document.getElementById(`menu-${menuId}`).style.display = 'block';
        document.getElementById('mainPageTitle').innerHTML = title;
        document.querySelectorAll('.menu-item').forEach(item => item.classList.remove('active'));
        document.querySelector(`.menu-item[data-menu="${menuId}"]`).classList.add('active');
        if(window.innerWidth <= 768) closeSidebar();
    }

    function toggleSidebar() { document.getElementById('sidebar').classList.toggle('open'); document.getElementById('overlay').classList.toggle('show'); }
    function closeSidebar() { document.getElementById('sidebar').classList.remove('open'); document.getElementById('overlay').classList.remove('show'); }

    // Init
    window.onload = () => {
        loadData();
        renderTabelMasuk();
        renderTabelKeluar();
        renderRekapStok();
        renderRingkasan();

        document.getElementById('tglMasuk').value = new Date().toISOString().slice(0,10);
        document.getElementById('tglKeluar').value = new Date().toISOString().slice(0,10);

        document.querySelectorAll('.menu-item').forEach(item => {
            item.addEventListener('click', () => showMenu(item.dataset.menu, item.querySelector('.menu-text').innerHTML));
        });
        document.getElementById('toggleFormMasukBtn').onclick = () => document.getElementById('formBarangMasuk').classList.toggle('show');
        document.getElementById('toggleFormKeluarBtn').onclick = () => document.getElementById('formBarangKeluar').classList.toggle('show');
        document.getElementById('simpanMasukBtn').onclick = addBarangMasuk;
        document.getElementById('simpanKeluarBtn').onclick = addBarangKeluar;
        document.getElementById('exportExcelBtn').onclick = exportToExcel;
        document.getElementById('printLaporanBtn').onclick = printLaporan;
        document.getElementById('hamburgerBtn').onclick = toggleSidebar;
        document.getElementById('overlay').onclick = closeSidebar;
        document.getElementById('searchRekap').addEventListener('input', renderRekapStok);
        document.getElementById('clearSearch').onclick = () => { document.getElementById('searchRekap').value = ''; renderRekapStok(); };
    };
    window.editMasuk = editMasuk; window.hapusMasuk = hapusMasuk;
    window.editKeluar = editKeluar; window.hapusKeluar = hapusKeluar;
</script>
</body>
</html>
