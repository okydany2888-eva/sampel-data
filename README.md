<!DOCTYPE html>
<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, user-scalable=yes">
    <title>Inventory Dashboard</title>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0-beta3/css/all.min.css">
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700&display=swap" rel="stylesheet">
    <style>
        * { margin: 0; padding: 0; box-sizing: border-box; font-family: 'Inter', sans-serif; }
        body { background: #f1f5f9; overflow-x: hidden; }
        
        /* Sidebar */
        .sidebar {
            position: fixed; left: 0; top: 0; width: 280px; height: 100%;
            background: linear-gradient(180deg, #0f172a 0%, #1e1b4b 100%);
            color: #e2e8f0; z-index: 1000; transform: translateX(-100%);
            transition: transform 0.3s ease; overflow-y: auto;
        }
        .sidebar.open { transform: translateX(0); }
        .sidebar-header { padding: 24px 20px; border-bottom: 1px solid rgba(255,255,255,0.08); margin-bottom: 20px; }
        .sidebar-header h2 { display: flex; align-items: center; gap: 12px; font-size: 20px; }
        .nav-menu { padding: 0 12px; }
        .menu-section { margin-bottom: 20px; }
        .menu-section-title { font-size: 11px; text-transform: uppercase; color: #94a3b8; padding: 8px 16px; }
        .menu-item {
            display: flex; align-items: center; gap: 14px; padding: 12px 16px; margin-bottom: 6px;
            border-radius: 14px; cursor: pointer; transition: all 0.2s; color: #cbd5e1;
        }
        .menu-item:hover { background: rgba(255,255,255,0.08); color: white; transform: translateX(4px); }
        .menu-item.active { background: linear-gradient(135deg, #3b82f6 0%, #8b5cf6 100%); color: white; }
        .menu-icon { font-size: 20px; width: 32px; text-align: center; }
        
        .add-menu-section { padding: 16px; margin-top: 20px; border-top: 1px solid rgba(255,255,255,0.08); }
        .add-menu-section input { width: 100%; padding: 10px 14px; margin-bottom: 10px; background: rgba(255,255,255,0.08); border: 1px solid rgba(255,255,255,0.1); border-radius: 12px; color: white; }
        .add-menu-section button { width: 100%; padding: 10px; background: linear-gradient(135deg, #3b82f6 0%, #8b5cf6 100%); border: none; border-radius: 12px; color: white; cursor: pointer; display: flex; align-items: center; justify-content: center; gap: 8px; }
        
        .hamburger { position: fixed; top: 16px; left: 16px; z-index: 1001; background: #0f172a; border: none; color: white; width: 44px; height: 44px; border-radius: 12px; font-size: 22px; cursor: pointer; }
        .overlay { display: none; position: fixed; top: 0; left: 0; right: 0; bottom: 0; background: rgba(0,0,0,0.5); z-index: 999; }
        .overlay.show { display: block; }
        
        .main-content { margin-left: 0; min-height: 100vh; transition: margin-left 0.3s ease; padding: 12px; }
        @media (min-width: 769px) { .main-content.shift { margin-left: 280px; } }
        .content-area { background: #f1f5f9; border-radius: 0; }
        
        @media print { .sidebar, .hamburger, .overlay { display: none; } .main-content { margin-left: 0 !important; } }
    </style>
</head>
<body>

    <button class="hamburger" id="hamburgerBtn">☰</button>
    
    <div class="sidebar" id="sidebar">
        <div class="sidebar-header"><h2><span>📦</span><span>Inventory</span></h2></div>
        <div class="nav-menu" id="menuContainer"></div>
        <div class="add-menu-section">
            <h4>➕ TAMBAH MENU</h4>
            <input type="text" id="menuName" placeholder="Nama menu">
            <input type="text" id="menuFile" placeholder="File HTML">
            <button onclick="addNewMenu()"><span>➕</span> Tambah Menu</button>
        </div>
    </div>
    
    <div class="overlay" id="overlay" onclick="closeSidebar()"></div>
    
    <div class="main-content" id="mainContent">
        <div class="content-area" id="contentArea">
            <div style="display: flex; justify-content: center; align-items: center; height: 300px;">
                <i class="fas fa-spinner fa-spin" style="font-size: 40px; color: #3b82f6;"></i>
                <span style="margin-left: 12px;">Memuat dashboard...</span>
            </div>
        </div>
    </div>

    <script>
        // ========= KONFIGURASI MENU =========
        let menus = [
            { section: "📋 Transaksi", items: [
                { name: "📦 Barang Masuk", file: "barang-masuk.html", icon: "📦", isDefault: true },
                { name: "📤 Stock Consumable", file: "stock-consumable.html", icon: "📤", isDefault: true },
                { name: "📊 Rekap Stok", file: "rekap-stok.html", icon: "📊", isDefault: true }
            ]},
            { section: "⚙️ Pengaturan", items: [
                { name: "🔧 Pengaturan", file: "pengaturan.html", icon: "🔧", isDefault: true }
            ]}
        ];
        let customMenus = [];

        function loadData() {
            const savedCustom = localStorage.getItem("dashboardCustomMenus");
            if (savedCustom) customMenus = JSON.parse(savedCustom);
        }
        function saveCustomMenus() { localStorage.setItem("dashboardCustomMenus", JSON.stringify(customMenus)); }

        function renderMenu() {
            const container = document.getElementById("menuContainer");
            if (!container) return;
            container.innerHTML = "";
            
            menus.forEach(section => {
                const sectionDiv = document.createElement("div");
                sectionDiv.className = "menu-section";
                const sectionTitle = document.createElement("div");
                sectionTitle.className = "menu-section-title";
                sectionTitle.innerText = section.section;
                sectionDiv.appendChild(sectionTitle);
                section.items.forEach((item, idx) => {
                    sectionDiv.appendChild(createMenuItem(item.name, item.file, item.icon));
                });
                container.appendChild(sectionDiv);
            });

            if (customMenus.length > 0) {
                const divider = document.createElement("div");
                divider.className = "menu-section";
                divider.style.height = "1px";
                divider.style.background = "rgba(255,255,255,0.08)";
                divider.style.margin = "16px";
                container.appendChild(divider);
                
                const customSection = document.createElement("div");
                customSection.className = "menu-section";
                const customTitle = document.createElement("div");
                customTitle.className = "menu-section-title";
                customTitle.innerText = "📌 MENU TAMBAHAN";
                customSection.appendChild(customTitle);
                customMenus.forEach((item) => {
                    customSection.appendChild(createMenuItem(item.name, item.file, item.icon || "📄"));
                });
                container.appendChild(customSection);
            }
        }

        function createMenuItem(name, file, icon) {
            const menuDiv = document.createElement("div");
            menuDiv.className = "menu-item";
            menuDiv.innerHTML = `<div class="menu-icon">${icon}</div><div class="menu-text">${name}</div>`;
            menuDiv.onclick = () => {
                document.querySelectorAll(".menu-item").forEach(item => item.classList.remove("active"));
                menuDiv.classList.add("active");
                loadContent(file, name);
                if (window.innerWidth <= 768) closeSidebar();
            };
            return menuDiv;
        }

        // ========= KONEKSI DATA GLOBAL =========
        // Semua halaman akan membaca/menulis ke localStorage yang SAMA
        
        function loadContent(file, title) {
            const contentArea = document.getElementById("contentArea");
            contentArea.innerHTML = `<div style="display: flex; justify-content: center; align-items: center; height: 200px;"><i class="fas fa-spinner fa-spin" style="font-size: 40px;"></i><span style="margin-left: 12px;">Memuat ${title}...</span></div>`;
            
            fetch(file)
                .then(response => {
                    if (!response.ok) throw new Error(`File ${file} tidak ditemukan`);
                    return response.text();
                })
                .then(html => {
                    // Suntikkan script pengarah ke localStorage yang sama
                    const modifiedHtml = injectLocalStorageBridge(html);
                    contentArea.innerHTML = modifiedHtml;
                    
                    // Eksekusi script yang ada
                    const scripts = contentArea.querySelectorAll("script");
                    scripts.forEach(oldScript => {
                        const newScript = document.createElement("script");
                        if (oldScript.src) newScript.src = oldScript.src;
                        else newScript.textContent = oldScript.textContent;
                        document.body.appendChild(newScript);
                        oldScript.remove();
                    });
                })
                .catch(error => {
                    contentArea.innerHTML = `
                        <div style="background: white; border-radius: 20px; padding: 40px; text-align: center;">
                            <i class="fas fa-exclamation-triangle" style="font-size: 48px; color: #ef4444;"></i>
                            <h3 style="margin-top: 16px;">File tidak ditemukan</h3>
                            <p>File <strong>${file}</strong> belum dibuat.</p>
                            <button onclick="location.reload()" style="margin-top: 20px; padding: 10px 24px; background: #3b82f6; color: white; border: none; border-radius: 12px; cursor: pointer;">
                                <i class="fas fa-sync-alt"></i> Refresh
                            </button>
                        </div>
                    `;
                });
        }
        
        // Fungsi untuk memastikan semua halaman menggunakan localStorage yang sama
        function injectLocalStorageBridge(html) {
            // Hapus script yang mungkin membuat localStorage terpisah
            // Tambahkan script sinkronisasi di awal
            const bridgeScript = `
            <script>
                // Bridge untuk memastikan semua halaman menggunakan data yang sama
                window.SHARED_STORAGE_KEYS = ['inv_masuk_final_v2', 'inv_keluar_final_v2', 'inv_master_data', 'dashboardCustomMenus'];
                
                // Fungsi untuk broadcast perubahan ke halaman induk
                function broadcastDataChange(key, value) {
                    localStorage.setItem(key, value);
                    // Kirim pesan ke iframe parent (jika ada)
                    if (window.parent !== window) {
                        window.parent.postMessage({ type: 'storageUpdate', key: key, value: value }, '*');
                    }
                }
                
                // Override localStorage.setItem untuk broadcast
                const originalSetItem = localStorage.setItem;
                localStorage.setItem = function(key, value) {
                    originalSetItem.call(this, key, value);
                    if (window.SHARED_STORAGE_KEYS.includes(key)) {
                        if (window.parent !== window) {
                            window.parent.postMessage({ type: 'storageUpdate', key: key, value: value }, '*');
                        }
                    }
                };
                
                // Listener untuk menerima update dari parent
                window.addEventListener('message', function(event) {
                    if (event.data && event.data.type === 'storageUpdate') {
                        originalSetItem.call(localStorage, event.data.key, event.data.value);
                        // Trigger event storage manual
                        window.dispatchEvent(new StorageEvent('storage', {
                            key: event.data.key,
                            newValue: event.data.value
                        }));
                    }
                });
            <\/script>
            `;
            return bridgeScript + html;
        }

        function addNewMenu() {
            const name = document.getElementById("menuName").value.trim();
            let file = document.getElementById("menuFile").value.trim();
            if (!name || !file) { alert("Isi nama dan file HTML!"); return; }
            if (!file.endsWith(".html")) file += ".html";
            
            let duplicate = false;
            menus.forEach(s => s.items.forEach(i => { if (i.file === file) duplicate = true; }));
            if (customMenus.some(m => m.file === file)) duplicate = true;
            if (duplicate) { alert("Menu sudah ada!"); return; }
            
            let icon = "📄";
            if (name.includes("Lap")) icon = "📈";
            else if (name.includes("Stat")) icon = "📊";
            customMenus.push({ name: name, file: file, icon: icon });
            saveCustomMenus();
            renderMenu();
            loadContent(file, name);
            document.getElementById("menuName").value = "";
            document.getElementById("menuFile").value = "";
            if (window.innerWidth <= 768) closeSidebar();
            alert(`✅ Menu "${name}" ditambahkan! Buat file ${file}`);
        }

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
                if (window.innerWidth <= 768) overlay.classList.add("show");
                else mainContent.classList.add("shift");
                hamburger.innerHTML = "✕";
            }
        }
        function closeSidebar() {
            document.getElementById("sidebar").classList.remove("open");
            document.getElementById("overlay").classList.remove("show");
            document.getElementById("mainContent").classList.remove("shift");
            document.getElementById("hamburgerBtn").innerHTML = "☰";
        }
        function handleResize() {
            if (window.innerWidth > 768) document.getElementById("overlay").classList.remove("show");
            else document.getElementById("mainContent").classList.remove("shift");
        }
        
        window.onload = () => {
            loadData();
            renderMenu();
            window.addEventListener("resize", handleResize);
            document.getElementById("hamburgerBtn").onclick = toggleSidebar;
            // Load menu pertama
            if (menus[0] && menus[0].items[0]) {
                loadContent(menus[0].items[0].file, menus[0].items[0].name);
                setTimeout(() => {
                    const firstMenu = document.querySelector(".menu-item");
                    if (firstMenu) firstMenu.classList.add("active");
                }, 100);
            }
        };
    </script>
</body>
</html>
