# Input-Data
<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, user-scalable=yes">
    <title>Form Input Barang Masuk dari Supplier</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            font-family: system-ui, 'Segoe UI', 'Inter', 'Poppins', sans-serif;
        }

        body {
            background: linear-gradient(145deg, #e0eafc 0%, #cfdef3 100%);
            min-height: 100vh;
            padding: 2rem 1.5rem;
            display: flex;
            justify-content: center;
            align-items: center;
        }

        /* Card utama */
        .card {
            max-width: 720px;
            width: 100%;
            margin: 0 auto;
            background: rgba(255, 255, 255, 0.96);
            backdrop-filter: blur(0px);
            border-radius: 36px;
            box-shadow: 0 25px 45px -12px rgba(0, 0, 0, 0.35), 0 4px 12px rgba(0, 0, 0, 0.05);
            overflow: hidden;
            transition: transform 0.2s ease;
        }

        .card:hover {
            transform: translateY(-3px);
        }

        /* Header */
        .header {
            background: #1e2f5e;
            padding: 1.6rem 2rem;
            color: white;
        }

        .header h1 {
            font-size: 1.85rem;
            font-weight: 600;
            letter-spacing: -0.3px;
            display: flex;
            align-items: center;
            gap: 12px;
        }

        .header h1::before {
            content: "📦";
            font-size: 1.9rem;
        }

        .header p {
            font-size: 0.9rem;
            opacity: 0.85;
            margin-top: 6px;
            font-weight: 400;
        }

        /* form area */
        .form-container {
            padding: 2rem 2rem 1.8rem;
        }

        .form-group {
            margin-bottom: 1.4rem;
            display: flex;
            flex-direction: column;
        }

        .form-row {
            display: flex;
            flex-wrap: wrap;
            gap: 1rem;
            margin-bottom: 1.4rem;
        }

        .form-row .form-group {
            flex: 1;
            margin-bottom: 0;
        }

        label {
            font-weight: 600;
            color: #1f2b48;
            margin-bottom: 6px;
            font-size: 0.85rem;
            letter-spacing: 0.3px;
            display: flex;
            align-items: center;
            gap: 6px;
        }

        label i {
            font-style: normal;
            font-weight: 500;
        }

        input, select, textarea {
            width: 100%;
            padding: 12px 14px;
            font-size: 0.95rem;
            border: 1.5px solid #e2e8f0;
            border-radius: 20px;
            background-color: #ffffff;
            transition: all 0.2s;
            outline: none;
            font-weight: 500;
            color: #0a1c2f;
        }

        input:focus, select:focus, textarea:focus {
            border-color: #1e2f5e;
            box-shadow: 0 0 0 3px rgba(30, 47, 94, 0.2);
        }

        textarea {
            resize: vertical;
            min-height: 80px;
        }

        /* tombol aksi */
        .button-group {
            display: flex;
            flex-wrap: wrap;
            gap: 1rem;
            margin-top: 2rem;
            margin-bottom: 1rem;
        }

        .btn {
            flex: 1;
            padding: 12px 16px;
            font-weight: 600;
            font-size: 0.95rem;
            border: none;
            border-radius: 40px;
            cursor: pointer;
            transition: all 0.2s;
            background: #f1f5f9;
            color: #1e2f5e;
            border: 1px solid #cbd5e1;
        }

        .btn-primary {
            background: #1e2f5e;
            color: white;
            border: none;
            box-shadow: 0 4px 8px rgba(0, 0, 0, 0.05);
        }

        .btn-primary:hover {
            background: #0f2147;
            transform: translateY(-2px);
            box-shadow: 0 8px 18px rgba(30, 47, 94, 0.3);
        }

        .btn-secondary {
            background: white;
            border: 1px solid #cbd5e1;
        }

        .btn-secondary:hover {
            background: #f8fafc;
            border-color: #94a3b8;
        }

        /* daftar barang masuk */
        .record-section {
            background: #f8fafd;
            border-top: 2px dashed #cbd5e6;
            padding: 1.5rem 2rem 2rem;
        }

        .record-header {
            display: flex;
            justify-content: space-between;
            align-items: baseline;
            flex-wrap: wrap;
            gap: 10px;
            margin-bottom: 1.2rem;
        }

        .record-header h3 {
            font-weight: 700;
            font-size: 1.3rem;
            color: #0f2b3d;
            display: flex;
            align-items: center;
            gap: 8px;
        }

        .badge-count {
            background: #1e2f5e20;
            background-color: #e9eef3;
            padding: 4px 12px;
            border-radius: 60px;
            font-size: 0.8rem;
            font-weight: 600;
            color: #1e2f5e;
        }

        .table-wrapper {
            overflow-x: auto;
            border-radius: 24px;
        }

        table {
            width: 100%;
            border-collapse: collapse;
            background: white;
            border-radius: 24px;
            overflow: hidden;
            box-shadow: 0 1px 3px rgba(0,0,0,0.05);
        }

        th {
            background-color: #eef2f9;
            padding: 12px 12px;
            text-align: left;
            font-size: 0.75rem;
            font-weight: 700;
            text-transform: uppercase;
            letter-spacing: 0.5px;
            color: #2c3e66;
        }

        td {
            padding: 12px 12px;
            border-bottom: 1px solid #eef2f9;
            font-size: 0.85rem;
            color: #1f2a40;
            vertical-align: top;
        }

        .delete-btn {
            background: none;
            border: none;
            font-size: 1.3rem;
            cursor: pointer;
            color: #b91c1c;
            transition: 0.1s;
            padding: 4px 8px;
            border-radius: 30px;
        }

        .delete-btn:hover {
            background: #fee2e2;
            transform: scale(1.05);
        }

        .empty-row td {
            text-align: center;
            padding: 2rem;
            color: #6c757d;
            font-style: italic;
        }

        footer {
            font-size: 0.7rem;
            text-align: center;
            padding: 1rem 2rem 1.5rem;
            color: #6c7a91;
            background: white;
            border-top: 1px solid #edf2f7;
        }

        @media (max-width: 580px) {
            body {
                padding: 1rem;
            }
            .form-container, .record-section {
                padding-left: 1.2rem;
                padding-right: 1.2rem;
            }
            th, td {
                font-size: 0.75rem;
                padding: 8px 6px;
            }
            .btn {
                font-size: 0.85rem;
            }
        }
    </style>
</head>
<body>

<div class="card">
    <div class="header">
        <h1>Input Barang Masuk</h1>
        <p>From Supplier · Kelola stok dengan rapi</p>
    </div>

    <div class="form-container">
        <form id="barangForm">
            <div class="form-row">
                <div class="form-group">
                    <label>📛 Nama Barang</label>
                    <input type="text" id="namaBarang" placeholder="Contoh: Kabel HDMI 2m" required>
                </div>
                <div class="form-group">
                    <label>🏷️ Kategori</label>
                    <select id="kategori" required>
                        <option value="" disabled selected>-- Pilih kategori --</option>
                        <option value="Booklet">Booklet</option>
                        <option value="Display">Display</option>
                        <option value="Filter tips">Filter tips</option>
                        <option value="Trapezoid">Sembako</option>
                        <option value="Sticker">Sticker</option>
                        <option value="Lainnya">Lainnya</option>
                    </select>
                </div>
            </div>

            <div class="form-row">
                <div class="form-group">
                    <label>🔢 Jumlah</label>
                    <input type="number" id="jumlah" placeholder="0" min="1" required>
                </div>
                <div class="form-group">
                    <label>📦 Satuan / Unit</label>
                    <input type="text" id="unit" placeholder="pcs, box, kg, liter" required>
                </div>
            </div>

            <div class="form-row">
                <div class="form-group">
                    <label>📅 Tanggal Masuk</label>
                    <input type="date" id="tanggalMasuk" required>
                </div>
                <div class="form-group">
                    <label>✏️ Catatan Supplier</label>
                    <input type="text" id="catatanSupplier" placeholder="Contoh: PO-123 / invoice #INV-001">
                </div>
            </div>

            <div class="button-group">
                <button type="submit" class="btn btn-primary">➕ Tambah Barang Masuk</button>
                <button type="button" id="resetFormBtn" class="btn btn-secondary">🗑️ Reset Form</button>
            </div>
        </form>
    </div>

    <div class="record-section">
        <div class="record-header">
            <h3>📋 Log Penerimaan Barang</h3>
            <span class="badge-count" id="totalItemCount">0 item</span>
        </div>
        <div class="table-wrapper">
            <table id="dataTable">
                <thead>
                    <tr>
                        <th>Nama Barang</th>
                        <th>Kategori</th>
                        <th>Jumlah</th>
                        <th>Unit</th>
                        <th>Tgl Masuk</th>
                        <th>Catatan Supplier</th>
                        <th>Aksi</th>
                    </tr>
                </thead>
                <tbody id="tableBody">
                    <tr class="empty-row">
                        <td colspan="7">Belum ada data. Silakan tambah barang masuk dari supplier.</td>
                    </tr>
                </tbody>
            </table>
        </div>
    </div>
    <footer>
        ⚡ Data tersimpan di lokal (LocalStorage) · Refresh halaman tidak akan hilang
    </footer>
</div>

<script>
    // === Data storage ===
    let incomingItems = [];       // array of objects

    // DOM elements
    const form = document.getElementById('barangForm');
    const namaBarangInput = document.getElementById('namaBarang');
    const kategoriSelect = document.getElementById('kategori');
    const jumlahInput = document.getElementById('jumlah');
    const unitInput = document.getElementById('unit');
    const tanggalMasukInput = document.getElementById('tanggalMasuk');
    const catatanSupplierInput = document.getElementById('catatanSupplier');
    const resetFormBtn = document.getElementById('resetFormBtn');
    const tableBody = document.getElementById('tableBody');
    const totalItemCountSpan = document.getElementById('totalItemCount');

    // Helper: set default tanggal hari ini (YYYY-MM-DD)
    function setDefaultDate() {
        if (!tanggalMasukInput.value) {
            const today = new Date();
            const yyyy = today.getFullYear();
            const mm = String(today.getMonth() + 1).padStart(2, '0');
            const dd = String(today.getDate()).padStart(2, '0');
            tanggalMasukInput.value = `${yyyy}-${mm}-${dd}`;
        }
    }

    // Load data from localStorage
    function loadFromStorage() {
        const stored = localStorage.getItem('incoming_supplier_items');
        if (stored) {
            try {
                incomingItems = JSON.parse(stored);
                // validasi sedikit: pastikan array
                if (!Array.isArray(incomingItems)) incomingItems = [];
            } catch(e) { incomingItems = []; }
        } else {
            // Data contoh (optional, supaya tidak kosong sama sekali)
            incomingItems = [];
            // contoh jika ingin demo isi 1 sample? lebih baik kosong dulu, tapi boleh demo
            // Tapi biar bersih: kosong, user bisa nambah sendiri
        }
        renderTable();
        updateTotalCount();
    }

    // Save to localStorage
    function saveToStorage() {
        localStorage.setItem('incoming_supplier_items', JSON.stringify(incomingItems));
    }

    // Render table berdasarkan incomingItems
    function renderTable() {
        if (!tableBody) return;
        
        if (incomingItems.length === 0) {
            tableBody.innerHTML = `<tr class="empty-row"><td colspan="7">📭 Belum ada data. Silakan tambah barang masuk dari supplier.</td></tr>`;
            return;
        }

        let htmlRows = '';
        incomingItems.forEach((item, index) => {
            // format tanggal yang lebih rapi (opsional)
            let tglTampil = item.tanggalMasuk;
            if (tglTampil) {
                // jika ingin format dd/mm/yyyy, biar rapi? tapi tetap pakai yyyy-mm-dd juga oke
                // kita tampilkan asli simpanan
            }
            htmlRows += `
                <tr>
                    <td><strong>${escapeHtml(item.namaBarang)}</strong></td>
                    <td>${escapeHtml(item.kategori)}</td>
                    <td>${item.jumlah}</td>
                    <td>${escapeHtml(item.unit)}</td>
                    <td>${escapeHtml(item.tanggalMasuk)}</td>
                    <td>${escapeHtml(item.catatanSupplier || '-')}</td>
                    <td style="text-align: center;"><button class="delete-btn" data-index="${index}" title="Hapus barang">🗑️</button></td>
                </tr>
            `;
        });
        tableBody.innerHTML = htmlRows;

        // attach event listener ke semua tombol hapus
        const deleteButtons = document.querySelectorAll('.delete-btn');
        deleteButtons.forEach(btn => {
            btn.addEventListener('click', (e) => {
                const idx = parseInt(btn.getAttribute('data-index'), 10);
                if (!isNaN(idx)) {
                    deleteItem(idx);
                }
                e.stopPropagation();
            });
        });
    }

    // Hapus item berdasarkan index
    function deleteItem(index) {
        if (index >= 0 && index < incomingItems.length) {
            incomingItems.splice(index, 1);
            saveToStorage();
            renderTable();
            updateTotalCount();
            // optional: feedback halus (tidak pakai alert biar smooth)
            showTemporaryMessage("✅ Item berhasil dihapus");
        }
    }

    // Fungsi untuk menambah item baru
    function addNewItem(event) {
        event.preventDefault();

        // ambil value dan trim
        const namaBarang = namaBarangInput.value.trim();
        const kategori = kategoriSelect.value;
        const jumlahRaw = jumlahInput.value.trim();
        const unit = unitInput.value.trim();
        const tanggalMasuk = tanggalMasukInput.value;
        const catatanSupplier = catatanSupplierInput.value.trim();

        // validasi mandatory
        if (!namaBarang) {
            alert("❌ Nama barang harus diisi!");
            namaBarangInput.focus();
            return;
        }
        if (!kategori) {
            alert("❌ Silakan pilih kategori!");
            kategoriSelect.focus();
            return;
        }
        if (!jumlahRaw || parseInt(jumlahRaw) <= 0) {
            alert("❌ Jumlah harus diisi dengan angka positif!");
            jumlahInput.focus();
            return;
        }
        if (!unit) {
            alert("❌ Unit/satuan wajib diisi (contoh: pcs, box, kg)!");
            unitInput.focus();
            return;
        }
        if (!tanggalMasuk) {
            alert("❌ Tanggal masuk harus dipilih!");
            tanggalMasukInput.focus();
            return;
        }

        const jumlah = parseInt(jumlahRaw, 10);
        
        // cek duplikat ringan? optional tidak perlu strict, bebas

        // siapkan object item
        const newItem = {
            id: Date.now() + Math.random() * 1000, // id sederhana untuk key (opsional)
            namaBarang: namaBarang,
            kategori: kategori,
            jumlah: jumlah,
            unit: unit,
            tanggalMasuk: tanggalMasuk,
            catatanSupplier: catatanSupplier || ""
        };

        incomingItems.push(newItem);
        saveToStorage();
        renderTable();
        updateTotalCount();

        // reset form setelah sukses (kecuali tanggal bisa tetap hari ini tapi biarkan kosong & isi default lagi)
        resetFormFields();
        setDefaultDate();   // set default tanggal lagi ke hari ini
        // fokus ke nama barang untuk kemudahan input cepat
        namaBarangInput.focus();

        // feedback mini (tidak mengganggu)
        showTemporaryMessage("✨ Barang berhasil ditambahkan!");
    }

    // reset form fields (tapi tidak menghapus data localStorage, hanya bersihkan form)
    function resetFormFields() {
        namaBarangInput.value = '';
        kategoriSelect.value = '';
        jumlahInput.value = '';
        unitInput.value = '';
        catatanSupplierInput.value = '';
        // tanggal akan diisi default di setDefaultDate, tapi sementara kosong lalu set Default
        tanggalMasukInput.value = '';
        setDefaultDate();   // set tanggal hari ini
    }

    // update total item di badge
    function updateTotalCount() {
        const total = incomingItems.length;
        totalItemCountSpan.innerText = `${total} ${total === 1 ? 'item' : 'item'}`;
    }

    // pesan sementara (tanpa alert)
    let messageTimeout = null;
    function showTemporaryMessage(msg) {
        // cek apakah ada elemen floating? kita buat dynamic toast sederhana
        let toastEl = document.getElementById('dynamicToast');
        if (!toastEl) {
            toastEl = document.createElement('div');
            toastEl.id = 'dynamicToast';
            toastEl.style.position = 'fixed';
            toastEl.style.bottom = '25px';
            toastEl.style.left = '50%';
            toastEl.style.transform = 'translateX(-50%)';
            toastEl.style.backgroundColor = '#1e2f5e';
            toastEl.style.color = 'white';
            toastEl.style.padding = '10px 20px';
            toastEl.style.borderRadius = '60px';
            toastEl.style.fontSize = '0.85rem';
            toastEl.style.fontWeight = '500';
            toastEl.style.zIndex = '999';
            toastEl.style.boxShadow = '0 8px 20px rgba(0,0,0,0.2)';
            toastEl.style.backdropFilter = 'blur(4px)';
            toastEl.style.background = '#0f2a3ecc';
            document.body.appendChild(toastEl);
        }
        toastEl.textContent = msg;
        toastEl.style.opacity = '1';
        toastEl.style.transition = 'opacity 0.2s';
        if (messageTimeout) clearTimeout(messageTimeout);
        messageTimeout = setTimeout(() => {
            if (toastEl) toastEl.style.opacity = '0';
        }, 2000);
    }

    // Simple escape HTML untuk keamanan XSS
    function escapeHtml(str) {
        if (!str) return '';
        return str.replace(/[&<>]/g, function(m) {
            if (m === '&') return '&amp;';
            if (m === '<') return '&lt;';
            if (m === '>') return '&gt;';
            return m;
        }).replace(/[\uD800-\uDBFF][\uDC00-\uDFFF]/g, function(c) {
            return c;
        });
    }

    // Event listener form submit
    form.addEventListener('submit', addNewItem);
    resetFormBtn.addEventListener('click', () => {
        resetFormFields();
        showTemporaryMessage("🧹 Form sudah dibersihkan");
    });

    // inisialisasi awal: load data, set default date, render table
    function init() {
        loadFromStorage();
        setDefaultDate();
        // tambahan event untuk mencegah submit double
    }

    init();
</script>
</body>
</html>
