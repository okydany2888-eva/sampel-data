<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, user-scalable=yes, viewport-fit=cover">
    <title>Inventory Management - Dashboard</title>
    <!-- Font Awesome 6 -->
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0-beta3/css/all.min.css">
    <!-- Google Fonts -->
    <link href="https://fonts.googleapis.com/css2?family=Inter:opsz,wght@14..32,400;500;600;700&display=swap" rel="stylesheet">
    <!-- SheetJS (XLSX) -->
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
            overflow-x: hidden;
        }

        /* ========= SIDEBAR ========= */
        .sidebar {
            position: fixed;
            left: 0;
            top: 0;
            width: 280px;
            height: 100%;
            background: linear-gradient(180deg, #0f172a 0%, #1e1b4b 100%);
            color: #e2e8f0;
            z-index: 1000;
            transform: translateX(-100%);
            transition: transform 0.3s cubic-bezier(0.4, 0, 0.2, 1);
            box-shadow: 4px 0 20px rgba(0, 0, 0, 0.2);
            overflow-y: auto;
            overflow-x: hidden;
        }

        .sidebar.open {
            transform: translateX(0);
        }

        .sidebar-header {
            padding: 24px 20px;
            border-bottom: 1px solid rgba(255, 255, 255, 0.08);
            margin-bottom: 20px;
        }

        .sidebar-header h2 {
            display: flex;
            align-items: center;
            gap: 12px;
            font-size: 20px;
            font-weight: 600;
            color: white;
        }

        .nav-menu {
            padding: 0 12px;
        }

        .menu-section {
            margin-bottom: 20px;
        }

        .menu-section-title {
            font-size: 11px;
            text-transform: uppercase;
            letter-spacing: 1px;
            color: #94a3b8;
            padding: 8px 16px;
            margin-top: 8px;
        }

        .menu-item {
            display: flex;
            align-items: center;
            gap: 14px;
            padding: 12px 16px;
            margin-bottom: 6px;
            border-radius: 14px;
            cursor: pointer;
            transition: all 0.2s ease;
            color: #cbd5e1;
        }

        .menu-item:hover {
            background: rgba(255, 255, 255, 0.08);
            color: white;
            transform: translateX(4px);
        }

        .menu-item.active {
            background: linear-gradient(135deg, #3b82f6 0%, #8b5cf6 100%);
            color: white;
            box-shadow: 0 4px 12px rgba(59, 130, 246, 0.3);
        }

        .menu-icon {
            font-size: 20px;
            width: 32px;
            text-align: center;
        }

        .menu-text {
            font-size: 14px;
            font-weight: 500;
        }

        .divider {
            height: 1px;
            background: rgba(255, 255, 255, 0.08);
            margin: 16px;
        }

        /* Form Tambah Menu */
        .add-menu-section {
            padding: 16px;
            margin-top: 20px;
            border-top: 1px solid rgba(255, 255, 255, 0.08);
        }

        .add-menu-section h4 {
            font-size: 12px;
            color: #94a3b8;
            margin-bottom: 12px;
            letter-spacing: 1px;
        }

        .add-menu-section input {
            width: 100%;
            padding: 10px 14px;
            margin-bottom: 10px;
            background: rgba(255, 255, 255, 0.08);
            border: 1px solid rgba(255, 255, 255, 0.1);
            border-radius: 12px;
            color: white;
            font-size: 13px;
            outline: none;
        }

        .add-menu-section input:focus {
            border-color: #3b82f6;
            background: rgba(255, 255, 255, 0.12);
        }

        .add-menu-section input::placeholder {
            color: #64748b;
        }

        .add-menu-section button {
            width: 100%;
            padding: 10px;
            background: linear-gradient(135deg, #3b82f6 0%, #8b5cf6 100%);
            border: none;
            border-radius: 12px;
            color: white;
            font-weight: 600;
            cursor: pointer;
            display: flex;
            align-items: center;
            justify-content: center;
            gap: 8px;
        }

        /* Tombol Hamburger */
        .hamburger {
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
            font-size: 22px;
            cursor: pointer;
            display: flex;
            align-items: center;
            justify-content: center;
            box-shadow: 0 4px 12px rgba(0, 0, 0, 0.15);
        }

        /* Overlay untuk HP */
        .overlay {
            display: none;
            position: fixed;
            top: 0;
            left: 0;
            right: 0;
            bottom: 0;
            background: rgba(0, 0, 0, 0.5);
            z-index: 999;
            backdrop-filter: blur(2px);
        }

        .overlay.show {
            display: block;
        }

        /* Konten Utama */
        .main-content {
            margin-left: 0;
            min-height: 100vh;
            transition: margin-left 0.3s ease;
            padding: 12px;
        }

        @media (min-width: 769px) {
            .main-content.shift {
                margin-left: 280px;
            }
        }

        /* Area Konten Dinamis */
        .content-area {
            background: #f1f5f9;
            border-radius: 0;
        }

        @media print {
            .sidebar, .hamburger, .overlay, .add-menu-section {
                display: none !important;
            }
            .main-content {
                margin-left: 0 !important;
                padding: 0 !important;
            }
        }
    </style>
</head>
<body>

    <!-- Tombol Hamburger -->
    <button class="hamburger" id="hamburgerBtn">☰</button>

    <!-- Sidebar -->
    <div class="sidebar" id="sidebar">
        <div class="sidebar-header">
            <h2>
                <span>📦</span>
                <span>Inventory</span>
            </h2>
        </div>

        <div class="nav-menu" id="menuContainer">
            <!-- Menu akan diisi JavaScript -->
        </div>

        <div class="add-menu-section">
            <h4>➕ TAMBAH MENU BARU</h4>
            <input type="text" id="menuName" placeholder="Nama menu (contoh: Laporan)">
            <input type="text" id="menuFile" placeholder="File HTML (contoh: laporan.html)">
            <button onclick="addNewMenu()">
                <span>➕</span> Tambah Menu
            </button>
        </div>
    </div>

    <!-- Overlay -->
    <div class="overlay" id="overlay" onclick="closeSidebar()"></div>

    <!-- Konten Utama -->
    <div class="main-content" id="mainContent">
        <div class="content-area" id="contentArea">
            <!-- Konten akan diisi oleh JavaScript -->
        </div>
    </div>

    <script>
        // ========= DATA MENU DEFAULT (Per Bagian) =========
        let menus = [
            { 
                section: "📋 Transaksi",
                items: [
                    { name: "📦 Barang Masuk", file: "barang-masuk.html", icon: "📦", isDefault: true },
                    { name: "📤 Stock Consumable", file: "stock-consumable.html", icon: "📤", isDefault: true },
                    { name: "📊 Rekap Stok", file: "rekap-stok.html", icon: "📊", isDefault: true }
                ]
            },
            { 
                section: "⚙️ Pengaturan",
                items: [
                    { name: "🔧 Pengaturan", file: "pengaturan.html", icon: "🔧", isDefault: true }
                ]
            }
        ];

        // Menu custom (tambahan dari user)
        let customMenus = [];

        // ========= LOAD & SAVE KE LOCALSTORAGE =========
        function loadData() {
            const savedCustom = localStorage.getItem("dashboardCustomMenus");
            if (savedCustom) {
                customMenus = JSON.parse(savedCustom);
            }
        }

        function saveCustomMenus() {
            localStorage.setItem("dashboardCustomMenus", JSON.stringify(customMenus));
        }

        // ========= RENDER MENU =========
        function renderMenu() {
            const container = document.getElementById("menuContainer");
            if (!container) return;
            
            container.innerHTML = "";

            // Render menu default per section
            menus.forEach(section => {
                const sectionDiv = document.createElement("div");
                sectionDiv.className = "menu-section";
                
                const sectionTitle = document.createElement("div");
                sectionTitle.className = "menu-section-title";
                sectionTitle.innerText = section.section;
                sectionDiv.appendChild(sectionTitle);
                
                section.items.forEach((item, idx) => {
                    const menuDiv = createMenuItem(item.name, item.file, item.icon, `${section.section}-${idx}`);
                    sectionDiv.appendChild(menuDiv);
                });
                
                container.appendChild(sectionDiv);
            });

            // Render custom menus jika ada
            if (customMenus.length > 0) {
                const divider = document.createElement("div");
                divider.className = "divider";
                container.appendChild(divider);
                
                const customSection = document.createElement("div");
                customSection.className = "menu-section";
                
                const customTitle = document.createElement("div");
                customTitle.className = "menu-section-title";
                customTitle.innerText = "📌 MENU TAMBAHAN";
                customSection.appendChild(customTitle);
                
                customMenus.forEach((item, idx) => {
                    const menuDiv = createMenuItem(item.name, item.file, item.icon || "📄", `custom-${idx}`);
                    customSection.appendChild(menuDiv);
                });
                
                container.appendChild(customSection);
            }

            // Aktifkan menu pertama
            const firstMenu = document.querySelector(".menu-item");
            if (firstMenu) {
                firstMenu.classList.add("active");
            }
        }

        function createMenuItem(name, file, icon, id) {
            const menuDiv = document.createElement("div");
            menuDiv.className = "menu-item";
            
            menuDiv.innerHTML = `
                <div class="menu-icon">${icon}</div>
                <div class="menu-text">${name}</div>
            `;

            menuDiv.onclick = function() {
                document.querySelectorAll(".menu-item").forEach(item => {
                    item.classList.remove("active");
                });
                menuDiv.classList.add("active");
                loadContent(file, name);
                
                if (window.innerWidth <= 768) {
                    closeSidebar();
                }
            };
            
            return menuDiv;
        }

        // ========= LOAD KONTEN DINAMIS =========
        function loadContent(file, title) {
            const contentArea = document.getElementById("contentArea");
            
            // Tampilkan loading
            contentArea.innerHTML = `
                <div style="display: flex; justify-content: center; align-items: center; height: 200px;">
                    <i class="fas fa-spinner fa-spin" style="font-size: 40px; color: #3b82f6;"></i>
                    <span style="margin-left: 12px;">Memuat ${title}...</span>
                </div>
            `;
            
            // Coba load file
            fetch(file)
                .then(response => {
                    if (!response.ok) {
                        throw new Error(`File ${file} tidak ditemukan (404)`);
                    }
                    return response.text();
                })
                .then(html => {
                    contentArea.innerHTML = html;
                    // Eksekusi script yang ada di dalam konten
                    const scripts = contentArea.querySelectorAll("script");
                    scripts.forEach(oldScript => {
                        const newScript = document.createElement("script");
                        if (oldScript.src) {
                            newScript.src = oldScript.src;
                        } else {
                            newScript.textContent = oldScript.textContent;
                        }
                        document.body.appendChild(newScript);
                        oldScript.remove();
                    });
                })
                .catch(error => {
                    contentArea.innerHTML = `
                        <div style="background: white; border-radius: 20px; padding: 40px; text-align: center;">
                            <i class="fas fa-exclamation-triangle" style="font-size: 48px; color: #ef4444;"></i>
                            <h3 style="margin-top: 16px; color: #0f172a;">File tidak ditemukan</h3>
                            <p style="color: #475569; margin-top: 8px;">File <strong>${file}</strong> belum dibuat.</p>
                            <p style="color: #475569;">Silakan buat file <code>${file}</code> di repository yang sama.</p>
                            <button onclick="location.reload()" style="margin-top: 20px; padding: 10px 24px; background: #3b82f6; color: white; border: none; border-radius: 12px; cursor: pointer;">
                                <i class="fas fa-sync-alt"></i> Refresh
                            </button>
                        </div>
                    `;
                });
        }

        // ========= TAMBAH MENU BARU =========
        function addNewMenu() {
            const nameInput = document.getElementById("menuName");
            const fileInput = document.getElementById("menuFile");

            let name = nameInput.value.trim();
            let file = fileInput.value.trim();

            if (name === "" || file === "") {
                alert("⚠️ Harap isi nama menu dan file HTML!");
                return;
            }

            if (!file.endsWith(".html")) {
                file = file + ".html";
            }

            // Cek duplikat di default menu
            let isDuplicate = false;
            menus.forEach(section => {
                section.items.forEach(item => {
                    if (item.file === file) isDuplicate = true;
                });
            });
            if (customMenus.some(m => m.file === file)) isDuplicate = true;

            if (isDuplicate) {
                alert("❌ Menu dengan file ini sudah ada!");
                return;
            }

            // Ambil ikon
            let icon = "📄";
            if (name.includes("Lap") || name.includes("lapor")) icon = "📈";
            else if (name.includes("Stat") || name.includes("stat")) icon = "📊";
            else if (name.includes("Setting") || name.includes("set")) icon = "⚙️";
            
            customMenus.push({ name: name, file: file, icon: icon });
            saveCustomMenus();
            renderMenu();
            loadContent(file, name);

            // Reset input
            nameInput.value = "";
            fileInput.value = "";

            // Tutup sidebar di HP
            if (window.innerWidth <= 768) {
                closeSidebar();
            }

            alert(`✅ Menu "${name}" berhasil ditambahkan!\n📁 Buat file ${file} di folder yang sama.`);
        }

        // ========= SIDEBAR FUNCTIONS =========
        function toggleSidebar() {
            const sidebar = document.getElementById("sidebar");
            const overlay = document.getElementById("overlay");
            const mainContent = document.getElementById("mainContent");
            const hamburger = document.getElementById("hamburgerBtn");

            if (sidebar.classList.contains("open")) {
                sidebar.classList.remove("open");
                overlay.classList.remove("show");
                mainContent.classList.remove("shift");
                hamburger.innerHTML = "☰";
            } else {
                sidebar.classList.add("open");
                if (window.innerWidth <= 768) {
                    overlay.classList.add("show");
                } else {
                    mainContent.classList.add("shift");
                }
                hamburger.innerHTML = "✕";
            }
        }

        function closeSidebar() {
            const sidebar = document.getElementById("sidebar");
            const overlay = document.getElementById("overlay");
            const mainContent = document.getElementById("mainContent");
            const hamburger = document.getElementById("hamburgerBtn");

            sidebar.classList.remove("open");
            overlay.classList.remove("show");
            mainContent.classList.remove("shift");
            hamburger.innerHTML = "☰";
        }

        function handleResize() {
            const sidebar = document.getElementById("sidebar");
            const overlay = document.getElementById("overlay");
            const mainContent = document.getElementById("mainContent");

            if (window.innerWidth > 768) {
                overlay.classList.remove("show");
                if (sidebar.classList.contains("open")) {
                    mainContent.classList.add("shift");
                } else {
                    mainContent.classList.remove("shift");
                }
            } else {
                mainContent.classList.remove("shift");
            }
        }

        // ========= INITIALISASI =========
        function init() {
            loadData();
            renderMenu();
            
            // Load konten pertama (Barang Masuk)
            if (menus[0] && menus[0].items[0]) {
                loadContent(menus[0].items[0].file, menus[0].items[0].name);
            }
            
            window.addEventListener("resize", handleResize);
            
            // Event listener untuk hamburger
            document.getElementById("hamburgerBtn").onclick = toggleSidebar;
            
            // Sidebar default tertutup
            document.getElementById("sidebar").classList.remove("open");
        }

        init();
    </script>
</body>
</html>
