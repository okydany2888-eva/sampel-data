<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, user-scalable=yes, viewport-fit=cover">
    <title>Barang Masuk | Supplier Management</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            -webkit-tap-highlight-color: transparent;
        }

        body {
            background: linear-gradient(145deg, #e0eafc 0%, #cfdef3 100%);
            min-height: 100vh;
            padding: 12px;
            display: flex;
            justify-content: center;
            align-items: center;
            font-family: system-ui, -apple-system, 'Segoe UI', 'Roboto', 'Helvetica Neue', sans-serif;
        }

        /* CARD UTAMA */
        .app-card {
            max-width: 1300px;
            width: 100%;
            margin: 0 auto;
            background: rgba(255, 255, 255, 0.97);
            border-radius: 32px;
            box-shadow: 0 20px 40px -12px rgba(0, 0, 0, 0.3);
            overflow: hidden;
            transition: all 0.2s ease;
        }

        /* HEADER */
        .header {
            background: #0f2b3d;
            background: linear-gradient(135deg, #0f2b3d 0%, #1e3a5f 100%);
            padding: 18px 20px;
            color: white;
        }
        .header h1 {
            font-size: 1.6rem;
            font-weight: 600;
            display: flex;
            align-items: center;
            gap: 10px;
            flex-wrap: wrap;
        }
        .header h1::before {
            content: "🏭";
            font-size: 1.7rem;
        }
        .header p {
            font-size: 0.8rem;
            opacity: 0.85;
            margin-top: 5px;
        }

        /* FORM */
        .form-section {
            padding: 20px 20px 12px;
        }
        .form-grid {
            display: flex;
            flex-wrap: wrap;
            gap: 14px;
            margin-bottom: 16px;
        }
        .input-group {
            flex: 1;
            min-width: 140px;
            display: flex;
            flex-direction: column;
        }
        .input-group label {
            font-size: 0.75rem;
            font-weight: 700;
            color: #1e2f5e;
            margin-bottom: 5px;
            letter-spacing: 0.3px;
            display: flex;
            align-items: center;
            gap: 5px;
        }
        .input-group input, .input-group select {
            padding: 10px 12px;
            border: 1.5px solid #e2e8f0;
            border-radius: 18px;
            font-size: 0.85rem;
            background: white;
            transition: 0.2s;
            outline: none;
        }
        .input-group input:focus, .input-group select:focus {
            border-color: #1e3a5f;
            box-shadow: 0 0 0 3px rgba(30, 58, 95, 0.2);
        }
        .btn-group {
            display: flex;
            gap: 12px;
            flex-wrap: wrap;
            margin: 8px 0 4px;
        }
        .btn {
            flex: 1;
            padding: 10px 16px;
            border-radius: 40px;
            font-weight: 600;
            font-size: 0.85rem;
            border: none;
            cursor: pointer;
            transition: 0.2s;
            background: #f1f5f9;
            color: #1e2f5e;
            border: 1px solid #cbd5e1;
        }
        .btn-primary {
            background: #1e3a5f;
            color: white;
            border: none;
            box-shadow: 0 4px 8px rgba(0,0,0,0.05);
        }
        .btn-primary:active { transform: scale(0.97); background: #0f2b44; }
        .btn-secondary:active { transform: scale(0.97); }

        /* TABEL & RECORD */
        .record-area {
            background: #f9fcff;
            border-top: 2px solid #e2edf2;
            padding: 18px 20px 20px;
        }
        .toolbar-row {
            display: flex;
            justify-content: space-between;
            align-items: center;
            flex-wrap: wrap;
            gap: 12px;
            margin-bottom: 18px;
        }
        .title-badge {
            display: flex;
            align-items: baseline;
            gap: 10px;
            flex-wrap: wrap;
        }
        .title-badge h3 {
            font-size: 1.2rem;
            color: #0a2b3e;
        }
        .badge {
            background: #e2e8f0;
            padding: 4px 12px;
            border-radius: 30px;
            font-size: 0.7rem;
            font-weight: 700;
            color: #1e2f5e;
        }
        .action-icons {
            display: flex;
            gap: 12px;
        }
        .icon-btn {
            background: white;
            border: 1px solid #cbd5e1;
            padding: 6px 16px;
            border-radius: 40px;
            font-size: 0.75rem;
            font-weight: 600;
            cursor: pointer;
            display: inline-flex;
            align-items: center;
            gap: 6px;
            transition: 0.2s;
        }
        .icon-btn:active { transform: scale(0.96); }

        /* SEARCH BAR */
        .search-container {
            background: white;
            border-radius: 28px;
            padding: 12px 16px;
            margin-bottom: 20px;
            display: flex;
            flex-wrap: wrap;
            align-items: flex-end;
            gap: 12px;
            border: 1px solid #e0edf5;
            box-shadow: 0 2px 6px rgba(0,0,0,0.02);
        }
        .search-box {
            flex: 2;
            min-width: 150px;
        }
        .search-box label {
            font-size: 0.65rem;
            font-weight: 600;
            color: #4b5563;
            margin-bottom: 3px;
            display: block;
        }
        .search-box input, .search-box select {
            width: 100%;
            padding: 8px 12px;
            border-radius: 40px;
            border: 1px solid #cbd5e6;
            background: #fefefe;
            font-size: 0.8rem;
        }
        .reset-filter {
            background: none;
            border: 1px solid #cbd5e1;
            padding: 8px 20px;
            border-radius: 40px;
            font-size: 0.75rem;
            font-weight: 500;
            cursor: pointer;
            margin-bottom: 0;
            white-space: nowrap;
        }
        .info-result {
            font-size: 0.7rem;
            background: #eef2ff;
            display: inline-block;
            padding: 4px 12px;
            border-radius: 20px;
            margin-bottom: 12px;
        }

        /* TABLE WRAPPER SCROLL */
        .table-wrap {
            overflow-x: auto;
            border-radius: 24px;
            -webkit-overflow-scrolling: touch;
        }
        table {
            width: 100%;
            border-collapse: collapse;
            background: white;
            border-radius: 24px;
            min-width: 780px;
        }
        th {
            background: #eef2fa;
            padding: 12px 8px;
            font-size: 0.7rem;
            font-weight: 700;
            color: #2c3e66;
            text-transform: uppercase;
        }
        td {
            padding: 10px 8px;
            border-bottom: 1px solid #eef2f5;
            font-size: 0.8rem;
            color: #1f2a3e;
        }
        .delete-cell {
            text-align: center;
        }
        .delete-btn {
            background: none;
            border: none;
            font-size: 1.2rem;
            cursor: pointer;
            color: #b91c1c;
            padding: 4px 8px;
            border-radius: 30px;
        }
        .empty-row td {
            text-align: center;
            padding: 32px;
            color: #6b7280;
        }
        footer {
            font-size: 0.6rem;
            text-align: center;
            padding: 12px 16px;
            background: white;
            color: #6c7a91;
            border-top: 1px solid #e9eff5;
        }

        /* ========= ANDROID PORTRAIT ========= */
        @media (max-width: 600px) {
            body { padding: 8px; }
            .app-card { border-radius: 24px; }
            .header { padding: 14px 16px; }
            .header h1 { font-size: 1.3rem; }
            .header p { font-size: 0.7rem; }
            .form-section { padding: 14px 14px 8px; }
            .form-grid { flex-direction: column; gap: 10px; }
            .input-group { min-width: 100%; }
            .btn-group { flex-direction: column; gap: 8px; }
            .record-area { padding: 14px 12px 16px; }
            .toolbar-row { flex-direction: column; align-items: stretch; }
            .action-icons { justify-content: flex-end; }
            .search-container { flex-direction: column; align-items: stretch; border-radius: 24px; padding: 12px; }
            .reset-filter { align-self: flex-start; margin-top: 4px; }
            th, td { padding: 8px 6px; font-size: 0.7rem; }
        }

        /* ========= ANDROID LANDSCAPE (tinggi terbatas) ========= */
        @media (max-height: 550px) and (orientation: landscape) {
            body { padding: 6px; }
            .header { padding: 8px 16px; }
            .header h1 { font-size: 1.1rem; gap: 6px; }
            .header p { display: none; }
            .form-section { padding: 8px 16px 4px; }
            .form-grid { gap: 8px; margin-bottom: 6px; }
            .input-group input, .input-group select { padding: 6px 10px; font-size: 0.75rem; }
            .input-group label { font-size: 0.65rem; }
            .btn { padding: 5px 12px; font-size: 0.7rem; }
            .record-area { padding: 8px 12px 12px; }
            .toolbar-row { margin-bottom: 8px; }
            .title-badge h3 { font-size: 0.95rem; }
            .badge { font-size: 0.6rem; padding: 2px 8px; }
            .icon-btn { padding: 4px 12px; font-size: 0.65rem; }
            .search-container { padding: 8px 12px; margin-bottom: 10px; gap: 8px; }
            .search-box input, .search-box select { padding: 5px 8px; font-size: 0.7rem; }
            .reset-filter { padding: 5px 14px; font-size: 0.65rem; }
            .table-wrap { max-height: 210px; overflow-y: auto; }
            th, td { padding: 5px 6px; font-size: 0.65rem; }
            footer { padding: 6px 12px; font-size: 0.55rem; }
            .delete-btn { font-size: 1rem; }
        }

        /* Tablet / bentang lebar landscape */
        @media (min-width: 700px) and (max-height: 500px) {
            .table-wrap { max-height: 240px; overflow-y: auto; }
        }

        @media print {
            body { background: white; padding: 0; }
            .app-card { box-shadow: none; border-radius: 0; }
            .form-section, .btn-group, .search-container, .action-icons, .delete-btn, footer, .reset-filter, .header p {
                display: none;
            }
            .record-area { padding: 0; }
            table { min-width: auto; border: 1px solid #ccc; }
            th, td { border: 1px solid #aaa; }
        }
    </style>
</head>
<body>
<div class="app-card">
    <div class="header">
        <h1>Input Barang Masuk</h1>
        <p>Manajemen Supplier · Pencarian Nama, Supplier & Kategori · Mobile Ready</p>
    </div>

    <div class="form-section">
        <form id="itemForm">
            <div class="form-grid">
                <div class="input-group">
                    <label>🏢 Nama Supplier</label>
                    <input type="text" id="supplierName" placeholder="PT. Sumber Jaya" required>
                </div>
                <div class="input-group">
                    <label>📦 Nama Barang</label>
                    <input type="text" id="productName" placeholder="Kertas / Box Display" required>
                </div>
            </div>
            <div class="form-grid">
                <div class="input-group">
                    <label>📂 Kategori</label>
                    <select id="category" required>
                        <option value="" disabled selected>Pilih kategori</option>
                        <option value="Booklet 1 1/4">Booklet 1 1/4</option>
                        <option value="Booklet 1 1/4 tips">Booklet 1 1/4 tips</option>
                        <option value="Booklet Kss">Booklet Kss</option>
                        <option value="Booklet Kss tips">Booklet Kss tips</option>
                        <option value="Display box">Display box</option>
                        <option value="Filter tips 21">Filter tips 21</option>
                        <option value="Filter tips 26">Filter tips 26</option>
                        <option value="Filter tips 30">Filter tips 30</option>
                        <option value="Trapezoid">Trapezoid</option>
                        <option value="Sticker">Sticker</option>
                        <option value="Lainnya">Lainnya</option>
                    </select>
                </div>
                <div class="input-group">
                    <label>🔢 Jumlah</label>
                    <input type="number" id="qty" placeholder="Jumlah" min="1" required>
                </div>
                <div class="input-group">
                    <label>📏 Satuan</label>
                    <input type="text" id="unit" placeholder="pcs, box, kg" required>
                </div>
            </div>
            <div class="form-grid">
                <div class="input-group">
                    <label>📅 Tanggal Masuk</label>
                    <input type="date" id="entryDate" required>
                </div>
                <div class="input-group">
                    <label>📝 Catatan / PO</label>
                    <input type="text" id="notes" placeholder="No. PO / Invoice">
                </div>
            </div>
            <div class="btn-group">
                <button type="submit" class="btn btn-primary">➕ Tambah Barang</button>
                <button type="button" id="resetFormBtn" class="btn btn-secondary">🔄 Reset Form</button>
            </div>
        </form>
    </div>

    <div class="record-area">
        <div class="toolbar-row">
            <div class="title-badge">
                <h3>📋 Log Penerimaan</h3>
                <span class="badge" id="totalCountBadge">0 item</span>
            </div>
            <div class="action-icons">
                <button id="printAction" class="icon-btn">🖨️ Print</button>
                <button id="exportCsv" class="icon-btn">📎 CSV</button>
            </div>
        </div>

        <!-- SEARCH + FILTER -->
        <div class="search-container">
            <div class="search-box">
                <label>🔍 Nama Barang</label>
                <input type="text" id="searchProduct" placeholder="Cari barang...">
            </div>
            <div class="search-box">
                <label>🏭 Supplier</label>
                <input type="text" id="searchSupplierField" placeholder="Cari supplier...">
            </div>
            <div class="search-box">
                <label>📂 Kategori</label>
                <select id="searchCategory">
                    <option value="">Semua Kategori</option>
                    <option value="Booklet 1 1/4">Booklet 1 1/4</option>
                    <option value="Booklet 1 1/4 tips">Booklet 1 1/4 tips</option>
                    <option value="Booklet Kss">Booklet Kss</option>
                    <option value="Booklet Kss tips">Booklet Kss tips</option>
                    <option value="Display box">Display box</option>
                    <option value="Filter tips 21">Filter tips 21</option>
                    <option value="Filter tips 26">Filter tips 26</option>
                    <option value="Filter tips 30">Filter tips 30</option>
                    <option value="Trapezoid">Trapezoid</option>
                    <option value="Sticker">Sticker</option>
                    <option value="Lainnya">Lainnya</option>
                </select>
            </div>
            <button id="clearFiltersBtn" class="reset-filter">✖️ Reset Filter</button>
        </div>
        <div id="filterInfo" class="info-result"></div>

        <div class="table-wrap">
            <table id="dataTable">
                <thead>
                    <tr><th>Supplier</th><th>Nama Barang</th><th>Kategori</th><th>Jumlah</th><th>Unit</th><th>Tgl Masuk</th><th>Catatan</th><th style="width:50px">Aksi</th></tr>
                </thead>
                <tbody id="tableBody">
                    <tr class="empty-row"><td colspan="8">Belum ada data. Silakan tambah barang masuk.</td></tr>
                </tbody>
            </table>
        </div>
    </div>
    <footer>⚡ Data tersimpan di LocalStorage | Cepat | Responsif Portrait & Landscape Android</footer>
</div>

<script>
    // ======================= DATA STORE =======================
    let masterItems = [];      // semua data asli
    let filteredItems = [];    // hasil filter

    // DOM Elements
    const form = document.getElementById('itemForm');
    const supplierInput = document.getElementById('supplierName');
    const productInput = document.getElementById('productName');
    const categorySelect = document.getElementById('category');
    const qtyInput = document.getElementById('qty');
    const unitInput = document.getElementById('unit');
    const dateInput = document.getElementById('entryDate');
    const notesInput = document.getElementById('notes');
    const resetButton = document.getElementById('resetFormBtn');
    const tbody = document.getElementById('tableBody');
    const totalBadge = document.getElementById('totalCountBadge');
    const printBtn = document.getElementById('printAction');
    const exportBtn = document.getElementById('exportCsv');
    // filter fields
    const searchProduct = document.getElementById('searchProduct');
    const searchSupplierField = document.getElementById('searchSupplierField');
    const searchCategory = document.getElementById('searchCategory');
    const clearFilterBtn = document.getElementById('clearFiltersBtn');
    const filterInfoSpan = document.getElementById('filterInfo');

    // helper: set default tanggal
    function setDefaultDate() {
        if (!dateInput.value) {
            const today = new Date();
            const yyyy = today.getFullYear();
            const mm = String(today.getMonth() + 1).padStart(2, '0');
            const dd = String(today.getDate()).padStart(2, '0');
            dateInput.value = `${yyyy}-${mm}-${dd}`;
        }
    }

    // load data from localStorage
    function loadData() {
        const stored = localStorage.getItem('supplier_goods_app');
        if (stored) {
            try {
                masterItems = JSON.parse(stored);
                if (!Array.isArray(masterItems)) masterItems = [];
            } catch(e) { masterItems = []; }
        } else {
            masterItems = [];
        }
        applyFiltersAndRender();
    }

    function saveToLocal() {
        localStorage.setItem('supplier_goods_app', JSON.stringify(masterItems));
    }

    // escape HTML
    function escapeHTML(str) {
        if (!str) return '';
        return str.replace(/[&<>]/g, function(m) {
            if (m === '&') return '&amp;';
            if (m === '<') return '&lt;';
            if (m === '>') return '&gt;';
            return m;
        });
    }

    // format tanggal tampilan
    function formatDateYMD(dateStr) {
        if (!dateStr) return '-';
        const parts = dateStr.split('-');
        if (parts.length !== 3) return dateStr;
        return `${parts[2]}/${parts[1]}/${parts[0]}`;
    }

    // FILTER + RENDER utama
    function applyFiltersAndRender() {
        const keywordProduct = searchProduct.value.trim().toLowerCase();
        const keywordSupplier = searchSupplierField.value.trim().toLowerCase();
        const selectedCat = searchCategory.value;

        filteredItems = masterItems.filter(item => {
            let matchProduct = true;
            let matchSupplier = true;
            let matchCat = true;
            if (keywordProduct) matchProduct = (item.nama || "").toLowerCase().includes(keywordProduct);
            if (keywordSupplier) matchSupplier = (item.supplier || "").toLowerCase().includes(keywordSupplier);
            if (selectedCat && selectedCat !== "") matchCat = (item.kategori || "") === selectedCat;
            return matchProduct && matchSupplier && matchCat;
        });

        // info text
        const totalAll = masterItems.length;
        const showing = filteredItems.length;
        if (keywordProduct || keywordSupplier || (selectedCat && selectedCat !== "")) {
            filterInfoSpan.textContent = `🔎 Menampilkan ${showing} dari ${totalAll} item`;
        } else {
            filterInfoSpan.textContent = `📋 Total semua: ${totalAll} item`;
        }
        totalBadge.textContent = (showing === totalAll) ? `${totalAll} item` : `${showing} / ${totalAll}`;

        renderTable(filteredItems);
    }

    // render tabel
    function renderTable(items) {
        if (!tbody) return;
        if (!items.length) {
            tbody.innerHTML = `<tr class="empty-row"><td colspan="8">📭 Tidak ada data sesuai filter / silakan tambah barang.</td></tr>`;
            return;
        }
        let html = '';
        items.forEach(item => {
            html += `
                <tr data-id="${item.id}">
                    <td>${escapeHTML(item.supplier)}</td>
                    <td>${escapeHTML(item.nama)}</td>
                    <td>${escapeHTML(item.kategori)}</td>
                    <td>${escapeHTML(String(item.jumlah))}</td>
                    <td>${escapeHTML(item.unit)}</td>
                    <td>${escapeHTML(formatDateYMD(item.tanggal))}</td>
                    <td>${escapeHTML(item.catatan || '-')}</td>
                    <td class="delete-cell"><button class="delete-btn" data-id="${item.id}" title="Hapus">🗑️</button></td>
                </tr>
            `;
        });
        tbody.innerHTML = html;
        // attach delete events
        document.querySelectorAll('.delete-btn').forEach(btn => {
            btn.addEventListener('click', (e) => {
                e.stopPropagation();
                const id = btn.getAttribute('data-id');
                if (id) deleteById(id);
            });
        });
    }

    function deleteById(id) {
        const newArr = masterItems.filter(it => String(it.id) !== String(id));
        if (newArr.length !== masterItems.length) {
            masterItems = newArr;
            saveToLocal();
            applyFiltersAndRender();
        }
    }

    // tambah item baru
    function addNewItem(data) {
        const newId = Date.now() + '-' + Math.random().toString(36).substring(2, 10);
        const newEntry = {
            id: newId,
            supplier: data.supplier,
            nama: data.nama,
            kategori: data.kategori,
            jumlah: data.jumlah,
            unit: data.unit,
            tanggal: data.tanggal,
            catatan: data.catatan || ''
        };
        masterItems.unshift(newEntry);
        saveToLocal();
        applyFiltersAndRender();
    }

    // reset form
    function resetFormFields() {
        form.reset();
        setDefaultDate();
        categorySelect.value = "";
        supplierInput.focus();
    }

    // handle submit
    form.addEventListener('submit', (e) => {
        e.preventDefault();
        const supplier = supplierInput.value.trim();
        const nama = productInput.value.trim();
        const kategori = categorySelect.value;
        const jumlahRaw = qtyInput.value.trim();
        const unit = unitInput.value.trim();
        const tanggal = dateInput.value;
        const catatan = notesInput.value.trim();

        if (!supplier) { alert("Supplier harus diisi!"); supplierInput.focus(); return; }
        if (!nama) { alert("Nama barang harus diisi!"); productInput.focus(); return; }
        if (!kategori) { alert("Pilih kategori!"); return; }
        if (!jumlahRaw || parseInt(jumlahRaw) <= 0) { alert("Jumlah harus >0"); qtyInput.focus(); return; }
        if (!unit) { alert("Satuan harus diisi!"); unitInput.focus(); return; }
        if (!tanggal) { alert("Tanggal masuk wajib"); return; }

        addNewItem({
            supplier: supplier,
            nama: nama,
            kategori: kategori,
            jumlah: parseInt(jumlahRaw),
            unit: unit,
            tanggal: tanggal,
            catatan: catatan
        });
        resetFormFields();
    });

    resetButton.addEventListener('click', () => resetFormFields());

    // event filter
    searchProduct.addEventListener('input', () => applyFiltersAndRender());
    searchSupplierField.addEventListener('input', () => applyFiltersAndRender());
    searchCategory.addEventListener('change', () => applyFiltersAndRender());
    clearFilterBtn.addEventListener('click', () => {
        searchProduct.value = '';
        searchSupplierField.value = '';
        searchCategory.value = '';
        applyFiltersAndRender();
    });

    // PRINT (hilangkan kolom aksi)
    printBtn.addEventListener('click', () => {
        const originalTable = document.getElementById('dataTable');
        const cloneTable = originalTable.cloneNode(true);
        // hapus kolom aksi (kolom terakhir) di header & body
        const thead = cloneTable.querySelector('thead tr');
        if (thead && thead.children.length >= 8) {
            thead.children[7].textContent = '';
        }
        const bodyRows = cloneTable.querySelectorAll('tbody tr');
        bodyRows.forEach(row => {
            const lastCell = row.querySelector('td:last-child');
            if (lastCell) lastCell.textContent = '';
        });
        const printWindow = window.open('', '_blank');
        printWindow.document.write(`
            <html><head><title>Laporan Barang Masuk</title>
            <style>
                body { font-family: sans-serif; margin: 24px; }
                table { width: 100%; border-collapse: collapse; }
                th, td { border: 1px solid #aaa; padding: 8px; text-align: left; }
                th { background: #f1f5f9; }
                h2 { color: #1e2f5e; }
            </style>
            </head><body>
            <h2>📦 Laporan Penerimaan Barang dari Supplier</h2>
            ${cloneTable.outerHTML}
            <p><small>Dicetak: ${new Date().toLocaleString()}</small></p>
            </body></html>
        `);
        printWindow.document.close();
        printWindow.print();
    });

    // EXPORT CSV berdasarkan data yang tampil (filter aktif)
    exportBtn.addEventListener('click', () => {
        const dataToExport = filteredItems.length ? filteredItems : masterItems;
        if (!dataToExport.length) {
            alert("Tidak ada data untuk diekspor.");
            return;
        }
        const headers = ['Supplier', 'Nama Barang', 'Kategori', 'Jumlah', 'Satuan', 'Tanggal Masuk', 'Catatan'];
        const csvRows = [];
        csvRows.push(headers.join(','));
        for (const item of dataToExport) {
            const row = [
                `"${(item.supplier || '').replace(/"/g, '""')}"`,
                `"${(item.nama || '').replace(/"/g, '""')}"`,
                `"${(item.kategori || '').replace(/"/g, '""')}"`,
                item.jumlah,
                `"${(item.unit || '').replace(/"/g, '""')}"`,
                `"${item.tanggal || ''}"`,
                `"${(item.catatan || '').replace(/"/g, '""')}"`
            ];
            csvRows.push(row.join(','));
        }
        const blob = new Blob(["\uFEFF" + csvRows.join('\n')], { type: 'text/csv;charset=utf-8;' });
        const link = document.createElement('a');
        const url = URL.createObjectURL(blob);
        link.href = url;
        link.setAttribute('download', `barang_masuk_${new Date().toISOString().slice(0,19)}.csv`);
        document.body.appendChild(link);
        link.click();
        document.body.removeChild(link);
        URL.revokeObjectURL(url);
    });

    // init
    setDefaultDate();
    loadData();
</script>
</body>
</html>
