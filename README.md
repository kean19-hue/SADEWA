<!DOCTYPE html>
<html lang="id" class="h-full bg-slate-100">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>SADEWA - Sistem Aplikasi Daur-Ulang Kab. Bogor</title>
    <!-- Tailwind CSS CDN -->
    <script src="https://cdn.tailwindcss.com"></script>
    <!-- Google Fonts: Plus Jakarta Sans -->
    <link href="https://fonts.googleapis.com/css2?family=Plus+Jakarta+Sans:wght@300;400;500;600;700;800&display=swap" rel="stylesheet">
    <!-- Lucide Icons -->
    <script src="https://unpkg.com/lucide@latest"></script>
    <script>
        tailwind.config = {
            theme: {
                extend: {
                    colors: {
                        emerald: {
                            50: '#ecfdf5',
                            100: '#d1fae5',
                            500: '#10b981',
                            600: '#059669',
                            700: '#047857',
                            800: '#065f46',
                            900: '#064e3b',
                            950: '#022c22',
                        },
                        teal: {
                            500: '#14b8a6',
                            600: '#0d9488',
                            700: '#0f766e',
                        }
                    },
                    fontFamily: {
                        sans: ['Plus Jakarta Sans', 'sans-serif'],
                    }
                }
            }
        }
    </script>
    <style>
        body { font-family: 'Plus Jakarta Sans', sans-serif; }
        
        .page-view {
            display: none;
            opacity: 0;
            transform: translateY(8px);
            transition: opacity 0.3s ease, transform 0.3s ease;
        }

        .page-view.active-page {
            display: block;
            opacity: 1;
            transform: translateY(0);
        }

        ::-webkit-scrollbar {
            width: 6px;
            height: 6px;
        }
        ::-webkit-scrollbar-track {
            background: #f1f5f9;
        }
        ::-webkit-scrollbar-thumb {
            background: #cbd5e1;
            border-radius: 9999px;
        }
        ::-webkit-scrollbar-thumb:hover {
            background: #94a3b8;
        }
    </style>
</head>
<body class="h-full flex flex-col md:flex-row overflow-hidden bg-slate-100 text-slate-800 antialiased">

    <!-- TOAST NOTIFICATION SYSTEM -->
    <div id="toast" class="fixed top-5 right-5 z-50 transform -translate-y-24 opacity-0 transition-all duration-300 ease-out bg-slate-900 text-white px-5 py-4 rounded-2xl shadow-2xl flex items-center space-x-3 border border-slate-700 max-w-md pointer-events-none">
        <div id="toast-icon" class="p-2 bg-emerald-500/20 text-emerald-400 rounded-xl">
            <i data-lucide="check-circle-2" class="w-5 h-5"></i>
        </div>
        <div>
            <h4 id="toast-title" class="font-bold text-sm text-white">Notifikasi</h4>
            <p id="toast-msg" class="text-xs text-slate-300">Aksi berhasil dilakukan.</p>
        </div>
    </div>

    <!-- LOGIN / AUTH MODAL OVERLAY -->
    <div id="login-modal" class="fixed inset-0 z-50 flex items-center justify-center p-4 bg-slate-950/80 backdrop-blur-sm transition-opacity duration-300">
        <div class="bg-white w-full max-w-md rounded-3xl shadow-2xl overflow-hidden border border-slate-100 p-6 sm:p-8 space-y-6">
            <div class="text-center space-y-2">
                <div class="mx-auto w-14 h-14 bg-gradient-to-tr from-emerald-600 to-teal-500 rounded-2xl shadow-lg shadow-emerald-600/30 flex items-center justify-center text-white">
                    <i data-lucide="recycle" class="w-8 h-8"></i>
                </div>
                <h2 class="text-2xl font-extrabold text-slate-900 tracking-tight">Masuk SADEWA</h2>
                <p class="text-xs text-slate-500">Sistem Daur-Ulang & Eko-Pelayanan Kab. Bogor</p>
            </div>

            <form id="login-form" onsubmit="handleLogin(event)" class="space-y-4">
                <!-- ERROR MESSAGE ALERT BOX -->
                <div id="login-error-alert" class="hidden p-3 bg-rose-50 border border-rose-200 text-rose-700 text-xs rounded-xl font-medium flex items-center space-x-2">
                    <i data-lucide="alert-circle" class="w-4 h-4 shrink-0 text-rose-600"></i>
                    <span id="login-error-text">Email atau kata sandi salah.</span>
                </div>

                <div>
                    <label class="block text-xs font-bold text-slate-700 uppercase tracking-wider mb-1.5">Email</label>
                    <input type="email" id="login-email" required placeholder="contoh: warga12bogor@gmail.com" class="w-full bg-slate-50 border border-slate-300 rounded-xl p-3 text-sm focus:ring-2 focus:ring-emerald-500 focus:outline-none">
                </div>
                <div>
                    <label class="block text-xs font-bold text-slate-700 uppercase tracking-wider mb-1.5">Kata Sandi</label>
                    <div class="relative">
                        <input type="password" id="login-password" required placeholder="••••••••" class="w-full bg-slate-50 border border-slate-300 rounded-xl p-3 text-sm focus:ring-2 focus:ring-emerald-500 focus:outline-none pr-10">
                        <button type="button" onclick="peekPassword()" class="absolute right-3 top-1/2 -translate-y-1/2 text-slate-400 hover:text-slate-600 focus:outline-none p-1" title="Intip Kata Sandi (1.5 Detik)">
                            <i id="password-toggle-icon" data-lucide="eye" class="w-4 h-4"></i>
                        </button>
                    </div>
                </div>

                <div class="p-3 bg-slate-50 rounded-xl border border-slate-200 text-[11px] text-slate-600 flex items-center space-x-2">
                    <i data-lucide="shield-check" class="w-4 h-4 text-emerald-600 shrink-0"></i>
                    <span>Sistem akan mendeteksi hak akses akun Anda secara otomatis saat masuk.</span>
                </div>

                <button type="submit" class="w-full bg-emerald-600 hover:bg-emerald-700 text-white font-bold py-3.5 rounded-xl shadow-lg shadow-emerald-600/20 transition flex items-center justify-center space-x-2 text-sm">
                    <i data-lucide="log-in" class="w-4 h-4"></i>
                    <span>Masuk ke Aplikasi</span>
                </button>
            </form>
        </div>
    </div>

    <!-- MOBILE TOP BAR -->
    <header class="md:hidden bg-slate-900 text-white p-4 flex items-center justify-between z-30 shadow-md">
        <div class="flex items-center space-x-3">
            <div class="p-2 bg-emerald-600 rounded-xl">
                <i data-lucide="recycle" class="w-5 h-5 text-white"></i>
            </div>
            <div>
                <span class="font-extrabold text-lg tracking-wide text-white">SADEWA</span>
                <span class="block text-[9px] uppercase tracking-wider text-emerald-400 font-bold">Kabupaten Bogor</span>
            </div>
        </div>
        <button id="mobile-menu-toggle" onclick="toggleMobileSidebar()" class="p-2 text-slate-300 hover:text-white focus:outline-none" aria-label="Buka Menu Navigasi">
            <i data-lucide="menu" class="w-6 h-6"></i>
        </button>
    </header>

    <!-- SIDEBAR NAVIGATION -->
    <aside id="sidebar" class="fixed md:static inset-y-0 left-0 z-40 w-72 bg-slate-900 text-slate-300 flex flex-col justify-between transition-transform duration-300 -translate-x-full md:translate-x-0 shadow-2xl md:shadow-none">
        <div>
            <!-- Sidebar Header -->
            <div class="p-6 hidden md:flex items-center space-x-3 border-b border-slate-800/80">
                <div class="p-2.5 bg-gradient-to-tr from-emerald-600 to-teal-500 rounded-2xl shadow-lg shadow-emerald-600/30 text-white">
                    <i data-lucide="recycle" class="w-6 h-6"></i>
                </div>
                <div>
                    <h1 class="font-extrabold text-xl tracking-wide text-white">SADEWA</h1>
                    <p class="text-[10px] font-bold uppercase tracking-wider text-emerald-400">Kabupaten Bogor</p>
                </div>
            </div>

            <!-- Profile Summary Widget -->
            <div class="mx-4 my-4 p-3.5 bg-slate-800/60 rounded-2xl border border-slate-700/50 flex items-center justify-between">
                <div class="flex items-center space-x-3 min-w-0">
                    <div id="user-avatar" class="w-10 h-10 rounded-xl bg-gradient-to-br from-emerald-500 to-teal-600 text-white flex items-center justify-center font-bold shadow-inner shrink-0">
                        <i data-lucide="user" class="w-5 h-5"></i>
                    </div>
                    <div class="min-w-0">
                        <p id="user-display-name" class="text-xs font-bold text-white truncate">Warga Eko-Aktif</p>
                        <p id="user-role-badge" class="text-[10px] text-emerald-400 font-semibold flex items-center space-x-1">
                            <span id="user-points-badge" class="flex items-center"><i data-lucide="coins" class="w-3 h-3 inline mr-1"></i> <span id="sidebar-user-points">0 PTS</span></span>
                        </p>
                    </div>
                </div>
                <button onclick="handleLogout()" title="Keluar Akun" class="p-2 text-slate-400 hover:text-rose-400 hover:bg-rose-500/10 rounded-xl transition">
                    <i data-lucide="log-out" class="w-4 h-4"></i>
                </button>
            </div>

            <!-- Navigation Links -->
            <nav class="px-3 space-y-1 mt-2">
                <p class="px-3 text-[10px] font-bold text-slate-500 uppercase tracking-wider mb-2">Menu Utama</p>
                
                <button onclick="switchTab('beranda')" id="nav-beranda" class="nav-item w-full flex items-center space-x-3 px-4 py-3 rounded-xl font-medium text-sm transition-all duration-200 bg-emerald-600 text-white shadow-lg shadow-emerald-600/20">
                    <i data-lucide="layout-dashboard" class="w-5 h-5"></i>
                    <span>Beranda</span>
                </button>

                <button onclick="switchTab('lokasi')" id="nav-lokasi" class="nav-item w-full flex items-center space-x-3 px-4 py-3 rounded-xl font-medium text-sm transition-all duration-200 text-slate-400 hover:bg-slate-800 hover:text-white">
                    <i data-lucide="map-pin" class="w-5 h-5"></i>
                    <span>Lokasi Bank Sampah</span>
                </button>

                <button onclick="switchTab('penjemputan')" id="nav-penjemputan" class="nav-item w-full flex items-center space-x-3 px-4 py-3 rounded-xl font-medium text-sm transition-all duration-200 text-slate-400 hover:bg-slate-800 hover:text-white">
                    <i data-lucide="truck" class="w-5 h-5"></i>
                    <span>Jadwal Penjemputan</span>
                </button>

                <button onclick="switchTab('kalkulator')" id="nav-kalkulator" class="nav-item w-full flex items-center space-x-3 px-4 py-3 rounded-xl font-medium text-sm transition-all duration-200 text-slate-400 hover:bg-slate-800 hover:text-white">
                    <i data-lucide="calculator" class="w-5 h-5"></i>
                    <span>Kalkulator & Poin</span>
                </button>

                <button onclick="switchTab('pengaduan')" id="nav-pengaduan" class="nav-item w-full flex items-center space-x-3 px-4 py-3 rounded-xl font-medium text-sm transition-all duration-200 text-slate-400 hover:bg-slate-800 hover:text-white">
                    <i data-lucide="alert-triangle" class="w-5 h-5"></i>
                    <span>Pengaduan Wilayah</span>
                </button>

                <!-- Admin Exclusive Tab Button -->
                <div id="nav-admin-wrapper" class="hidden pt-3 border-t border-slate-800">
                    <p class="px-3 text-[10px] font-bold text-amber-500 uppercase tracking-wider mb-2">Akses Khusus Admin</p>
                    <button onclick="switchTab('admin')" id="nav-admin" class="nav-item w-full flex items-center space-x-3 px-4 py-3 rounded-xl font-medium text-sm text-amber-300 hover:bg-amber-500/10 transition-all duration-200">
                        <i data-lucide="shield-alert" class="w-5 h-5"></i>
                        <span>Panel Admin DLH</span>
                    </button>
                </div>
            </nav>
        </div>

        <!-- Sidebar Footer -->
        <div class="p-4 border-t border-slate-800/80">
            <div class="bg-gradient-to-br from-emerald-950/80 to-slate-800/90 p-3.5 rounded-2xl border border-emerald-800/40 text-center">
                <i data-lucide="shield-check" class="w-6 h-6 text-emerald-400 mx-auto mb-1"></i>
                <p class="text-xs font-bold text-slate-200">DLH Kabupaten Bogor</p>
                <p class="text-[10px] text-slate-400 mt-0.5">Program Pengelolaan Sampah Terpadu Antar-Desa</p>
            </div>
        </div>
    </aside>

    <!-- MAIN CONTENT AREA -->
    <main class="flex-1 h-full overflow-y-auto bg-slate-100 relative">
        <div class="max-w-7xl mx-auto p-4 sm:p-6 lg:p-8">

            <!-- PAGE 1: BERANDA -->
            <section id="page-beranda" class="page-view active-page space-y-6">
                <!-- Welcome Banner -->
                <div class="bg-gradient-to-r from-emerald-800 via-emerald-700 to-teal-800 text-white rounded-3xl p-6 sm:p-8 shadow-xl relative overflow-hidden">
                    <div class="absolute -right-10 -bottom-10 opacity-10 text-white pointer-events-none">
                        <i data-lucide="leaf" class="w-72 h-72"></i>
                    </div>
                    <div class="relative z-10 max-w-2xl">
                        <span class="inline-flex items-center space-x-1.5 bg-emerald-500/20 border border-emerald-400/30 text-emerald-200 text-xs font-bold px-3 py-1 rounded-full mb-3">
                            <i data-lucide="sparkles" class="w-3.5 h-3.5"></i>
                            <span>SADEWA Sistem Eko-Pelayanan</span>
                        </span>
                        <h2 class="text-2xl sm:text-3xl font-extrabold tracking-tight leading-snug">
                            Pengelolaan Sampah Antar-Desa Kab. Bogor
                        </h2>
                        <p class="mt-2 text-slate-200 text-sm leading-relaxed">
                            Kurangi penumpukan sampah, kelola limbah terpilah dari rumah Anda, dan raih Eko-Poin yang dapat ditukarkan dengan berbagai hadiah menarik!
                        </p>
                        <div class="mt-6 flex flex-wrap gap-3">
                            <button onclick="switchTab('penjemputan')" class="bg-white hover:bg-emerald-50 text-emerald-800 font-bold px-5 py-2.5 rounded-xl text-sm shadow transition flex items-center space-x-2">
                                <i data-lucide="calendar-plus" class="w-4 h-4"></i>
                                <span>Jadwalkan Penjemputan</span>
                            </button>
                            <button onclick="switchTab('lokasi')" class="bg-emerald-900/60 hover:bg-emerald-900/80 text-white font-medium px-5 py-2.5 rounded-xl text-sm border border-emerald-500/30 transition flex items-center space-x-2">
                                <i data-lucide="map-pin" class="w-4 h-4"></i>
                                <span>Cari Bank Sampah</span>
                            </button>
                        </div>
                    </div>
                </div>

                <!-- Stats Summary Grid -->
                <div class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-4 gap-4">
                    <div class="bg-white p-5 rounded-2xl border border-slate-200/80 shadow-sm flex items-center space-x-4">
                        <div class="p-3 bg-emerald-100 text-emerald-700 rounded-xl">
                            <i data-lucide="scale" class="w-6 h-6"></i>
                        </div>
                        <div>
                            <p class="text-xs font-semibold text-slate-500">Total Sampah Terolah</p>
                            <p id="stat-total-weight" class="text-xl font-extrabold text-slate-800 mt-0.5">0 Kg</p>
                            <p class="text-[10px] text-emerald-600 font-bold mt-0.5">Sistem Baru Aktif</p>
                        </div>
                    </div>

                    <div class="bg-white p-5 rounded-2xl border border-slate-200/80 shadow-sm flex items-center space-x-4">
                        <div class="p-3 bg-teal-100 text-teal-700 rounded-xl">
                            <i data-lucide="map" class="w-6 h-6"></i>
                        </div>
                        <div>
                            <p class="text-xs font-semibold text-slate-500">Desa & TPS3R Terhubung</p>
                            <p class="text-xl font-extrabold text-slate-800 mt-0.5">86 Lokasi</p>
                            <p class="text-[10px] text-slate-400 font-medium mt-0.5">Di 24 Kecamatan</p>
                        </div>
                    </div>

                    <div class="bg-white p-5 rounded-2xl border border-slate-200/80 shadow-sm flex items-center space-x-4">
                        <div class="p-3 bg-amber-100 text-amber-700 rounded-xl">
                            <i data-lucide="coins" class="w-6 h-6"></i>
                        </div>
                        <div>
                            <p class="text-xs font-semibold text-slate-500">Poin Tersalurkan</p>
                            <p id="stat-total-points" class="text-xl font-extrabold text-slate-800 mt-0.5">0 PTS</p>
                            <p id="stat-total-rupiah" class="text-[10px] text-amber-600 font-bold mt-0.5">Setara Rp 0</p>
                        </div>
                    </div>

                    <div class="bg-white p-5 rounded-2xl border border-slate-200/80 shadow-sm flex items-center space-x-4">
                        <div class="p-3 bg-blue-100 text-blue-700 rounded-xl">
                            <i data-lucide="users" class="w-6 h-6"></i>
                        </div>
                        <div>
                            <p class="text-xs font-semibold text-slate-500">Warga Terdaftar</p>
                            <p id="stat-total-users" class="text-xl font-extrabold text-slate-800 mt-0.5">0 KK</p>
                            <p class="text-[10px] text-blue-600 font-bold mt-0.5">Aktif Memilah</p>
                        </div>
                    </div>
                </div>

                <!-- Quick Action Feature Cards -->
                <div class="grid grid-cols-1 md:grid-cols-3 gap-6">
                    <div onclick="switchTab('kalkulator')" class="bg-white p-6 rounded-2xl border border-slate-200/80 shadow-sm hover:shadow-md transition cursor-pointer group">
                        <div class="p-3 bg-emerald-50 text-emerald-600 rounded-2xl w-fit group-hover:bg-emerald-600 group-hover:text-white transition">
                            <i data-lucide="calculator" class="w-6 h-6"></i>
                        </div>
                        <h3 class="text-lg font-bold text-slate-800 mt-4">Kalkulator Eko-Poin</h3>
                        <p class="text-xs text-slate-500 mt-1.5 leading-relaxed">Simulasikan nilai rupiah dan poin dari sampah plastik, kertas, atau logam yang Anda miliki.</p>
                        <span class="inline-flex items-center text-xs font-bold text-emerald-600 mt-4 group-hover:translate-x-1 transition-transform">
                            Hitung Sekarang <i data-lucide="arrow-right" class="w-4 h-4 ml-1"></i>
                        </span>
                    </div>

                    <div onclick="switchTab('penjemputan')" class="bg-white p-6 rounded-2xl border border-slate-200/80 shadow-sm hover:shadow-md transition cursor-pointer group">
                        <div class="p-3 bg-teal-50 text-teal-600 rounded-2xl w-fit group-hover:bg-teal-600 group-hover:text-white transition">
                            <i data-lucide="truck" class="w-6 h-6"></i>
                        </div>
                        <h3 class="text-lg font-bold text-slate-800 mt-4">Jemput Sampah Desa</h3>
                        <p class="text-xs text-slate-500 mt-1.5 leading-relaxed">Punya sampah terpilah dalam jumlah banyak? Ajukan kurir jemput armada SADEWA ke alamat Anda.</p>
                        <span class="inline-flex items-center text-xs font-bold text-teal-600 mt-4 group-hover:translate-x-1 transition-transform">
                            Buat Permintaan <i data-lucide="arrow-right" class="w-4 h-4 ml-1"></i>
                        </span>
                    </div>

                    <div onclick="switchTab('pengaduan')" class="bg-white p-6 rounded-2xl border border-slate-200/80 shadow-sm hover:shadow-md transition cursor-pointer group">
                        <div class="p-3 bg-rose-50 text-rose-600 rounded-2xl w-fit group-hover:bg-rose-600 group-hover:text-white transition">
                            <i data-lucide="alert-circle" class="w-6 h-6"></i>
                        </div>
                        <h3 class="text-lg font-bold text-slate-800 mt-4">Adukan TPS Ilegal</h3>
                        <p class="text-xs text-slate-500 mt-1.5 leading-relaxed">Laporkan penumpukan sampah liar di area pemukiman atau jalan raya untuk ditindak petugas.</p>
                        <span class="inline-flex items-center text-xs font-bold text-rose-600 mt-4 group-hover:translate-x-1 transition-transform">
                            Kirim Laporan <i data-lucide="arrow-right" class="w-4 h-4 ml-1"></i>
                        </span>
                    </div>
                </div>
            </section>

            <!-- PAGE 2: LOKASI BANK SAMPAH & TPS -->
            <section id="page-lokasi" class="page-view space-y-6">
                <div class="flex flex-col sm:flex-row sm:items-center justify-between gap-4 bg-white p-6 rounded-2xl border border-slate-200/80 shadow-sm">
                    <div>
                        <h2 class="text-2xl font-bold text-slate-800">Direktori Bank Sampah & TPS3R</h2>
                        <p class="text-xs text-slate-500 mt-1">Temukan lokasi penampungan sampah terdekat di wilayah Kabupaten Bogor.</p>
                    </div>

                    <!-- Filter Dropdown -->
                    <div class="flex items-center space-x-2">
                        <label for="filter-kecamatan" class="text-xs font-bold text-slate-600 uppercase tracking-wider">Kecamatan:</label>
                        <select id="filter-kecamatan" onchange="filterTPS()" class="bg-slate-50 border border-slate-300 rounded-xl px-3 py-2 text-xs font-semibold text-slate-700 focus:outline-none focus:ring-2 focus:ring-emerald-500">
                            <option value="semua">Semua Kecamatan</option>
                            <option value="Cibinong">Cibinong</option>
                            <option value="Cileungsi">Cileungsi</option>
                            <option value="Dramaga">Dramaga</option>
                            <option value="Parung">Parung</option>
                            <option value="Gunung Putri">Gunung Putri</option>
                        </select>
                    </div>
                </div>

                <div id="tps-container" class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-6">
                    <!-- Dynamic Items Inserted via JS -->
                </div>
            </section>

            <!-- PAGE 3: JADWAL & LAYANAN PENJEMPUTAN -->
            <section id="page-penjemputan" class="page-view space-y-6">
                <div class="bg-white rounded-3xl border border-slate-200/80 shadow-sm p-6 sm:p-8 max-w-4xl mx-auto">
                    <div class="border-b border-slate-100 pb-5 mb-6">
                        <span class="inline-flex items-center space-x-1 text-emerald-600 font-bold text-xs uppercase tracking-wider mb-1">
                            <i data-lucide="truck" class="w-4 h-4"></i>
                            <span>Armada Antar-Desa</span>
                        </span>
                        <h2 class="text-2xl font-extrabold text-slate-800">Formulir Penjemputan Sampah Terpilah</h2>
                        <p class="text-xs text-slate-500 mt-1">Isi formulir di bawah ini untuk mendatangkan petugas penimbang SADEWA ke alamat lokasi Anda.</p>
                    </div>

                    <form id="form-penjemputan" onsubmit="handlePickupSubmit(event)" class="space-y-5">
                        <div class="grid grid-cols-1 sm:grid-cols-2 gap-5">
                            <div>
                                <label class="block text-xs font-bold text-slate-700 uppercase tracking-wider mb-2">Nama Sesuai KTP</label>
                                <input type="text" id="pickup-name" required placeholder="Contoh: Ibu Rina" class="w-full bg-slate-50 border border-slate-300 rounded-xl p-3 text-sm focus:ring-2 focus:ring-emerald-500 focus:outline-none">
                            </div>
                            <div>
                                <label class="block text-xs font-bold text-slate-700 uppercase tracking-wider mb-2">Nomor WhatsApp / HP</label>
                                <input type="tel" id="pickup-phone" required placeholder="081234567890" class="w-full bg-slate-50 border border-slate-300 rounded-xl p-3 text-sm focus:ring-2 focus:ring-emerald-500 focus:outline-none">
                            </div>
                        </div>

                        <div class="grid grid-cols-1 sm:grid-cols-2 gap-5">
                            <div>
                                <label class="block text-xs font-bold text-slate-700 uppercase tracking-wider mb-2">Kecamatan & Desa</label>
                                <select id="pickup-district" class="w-full bg-slate-50 border border-slate-300 rounded-xl p-3 text-sm focus:ring-2 focus:ring-emerald-500 focus:outline-none">
                                    <option value="Cibinong">Kec. Cibinong</option>
                                    <option value="Cileungsi">Kec. Cileungsi</option>
                                    <option value="Dramaga">Kec. Dramaga</option>
                                    <option value="Parung">Kec. Parung</option>
                                    <option value="Gunung Putri">Kec. Gunung Putri</option>
                                    <option value="Lainnya">Kecamatan Lainnya (Kab. Bogor)</option>
                                </select>
                            </div>
                            <div>
                                <label class="block text-xs font-bold text-slate-700 uppercase tracking-wider mb-2">Tanggal Penjemputan</label>
                                <input type="date" id="pickup-date" required class="w-full bg-slate-50 border border-slate-300 rounded-xl p-3 text-sm focus:ring-2 focus:ring-emerald-500 focus:outline-none">
                            </div>
                        </div>

                        <div class="grid grid-cols-1 sm:grid-cols-2 gap-5">
                            <div>
                                <label class="block text-xs font-bold text-slate-700 uppercase tracking-wider mb-2">Jenis Sampah Utama</label>
                                <select id="pickup-type" class="w-full bg-slate-50 border border-slate-300 rounded-xl p-3 text-sm focus:ring-2 focus:ring-emerald-500 focus:outline-none">
                                    <option value="Plastik & Botol PET" data-rate="500">Plastik & Botol PET (500 PTS / Kg)</option>
                                    <option value="Kertas & Dus Bekas" data-rate="700">Kertas & Dus Bekas (700 PTS / Kg)</option>
                                    <option value="Logam, Besi & Kaleng" data-rate="1500">Logam, Besi & Kaleng (1.500 PTS / Kg)</option>
                                    <option value="Organik / Bahan Kompos" data-rate="300">Organik / Bahan Kompos (300 PTS / Kg)</option>
                                    <option value="Campuran Anorganik" data-rate="400">Campuran Anorganik Terpilah (400 PTS / Kg)</option>
                                </select>
                            </div>
                            <div>
                                <label class="block text-xs font-bold text-slate-700 uppercase tracking-wider mb-2">Estimasi Berat (Kg)</label>
                                <input type="number" id="pickup-weight" min="5" required placeholder="Minimal 5 Kg" class="w-full bg-slate-50 border border-slate-300 rounded-xl p-3 text-sm focus:ring-2 focus:ring-emerald-500 focus:outline-none">
                            </div>
                        </div>

                        <div>
                            <label class="block text-xs font-bold text-slate-700 uppercase tracking-wider mb-2">Alamat Penjemputan Detail</label>
                            <textarea id="pickup-address" rows="3" required placeholder="Nama jalan, nomor rumah, RT/RW, dan patokan..." class="w-full bg-slate-50 border border-slate-300 rounded-xl p-3 text-sm focus:ring-2 focus:ring-emerald-500 focus:outline-none"></textarea>
                        </div>

                        <button type="submit" class="w-full bg-emerald-600 hover:bg-emerald-700 text-white font-bold py-4 rounded-2xl shadow-lg shadow-emerald-600/20 transition flex items-center justify-center space-x-2">
                            <i data-lucide="send" class="w-5 h-5"></i>
                            <span>Kirim Pengajuan Penjemputan</span>
                        </button>
                    </form>
                </div>
            </section>

            <!-- PAGE 4: KALKULATOR POIN & KATALOG -->
            <section id="page-kalkulator" class="page-view space-y-8">
                <div class="bg-white rounded-3xl border border-slate-200/80 shadow-sm p-6 sm:p-8">
                    <h2 class="text-2xl font-bold text-slate-800 mb-1">Kalkulator Estimasi Poin</h2>
                    <p class="text-xs text-slate-500 mb-6">Hitung estimasi Eko-Poin SADEWA yang akan Anda dapatkan dari hasil pemilahan sampah.</p>

                    <div class="grid grid-cols-1 md:grid-cols-2 gap-8 items-center">
                        <div class="space-y-5">
                            <div>
                                <label class="block text-xs font-bold text-slate-700 uppercase tracking-wider mb-2">Kategori Sampah</label>
                                <select id="calc-cat" onchange="runCalculator()" class="w-full bg-slate-50 border border-slate-300 rounded-xl p-3 text-sm font-semibold focus:outline-none focus:ring-2 focus:ring-emerald-500">
                                    <option value="500">Botol Plastik PET (500 Pts / Kg)</option>
                                    <option value="700">Kardus & Dus Bekas (700 Pts / Kg)</option>
                                    <option value="1500">Besi, Kaleng & Logam (1.500 Pts / Kg)</option>
                                    <option value="300">Sampah Organik Kompos (300 Pts / Kg)</option>
                                    <option value="2000">Elektronik Bekas (2.000 Pts / Kg)</option>
                                </select>
                            </div>

                            <div>
                                <div class="flex justify-between items-center mb-2">
                                    <label class="text-xs font-bold text-slate-700 uppercase tracking-wider">Berat Sampah (Kg)</label>
                                    <span id="calc-weight-label" class="text-sm font-extrabold text-emerald-600">5 Kg</span>
                                </div>
                                <input type="range" id="calc-range" min="5" max="100" value="5" oninput="runCalculator()" class="w-full h-2 bg-slate-200 rounded-lg appearance-none cursor-pointer accent-emerald-600">
                            </div>
                        </div>

                        <!-- Calculation Output Box -->
                        <div class="bg-gradient-to-br from-slate-900 to-emerald-950 text-white p-6 rounded-2xl shadow-inner space-y-4">
                            <div>
                                <span class="text-xs text-emerald-400 font-semibold uppercase tracking-wider">Hasil Poin SADEWA</span>
                                <div class="text-4xl font-extrabold text-white mt-1" id="calc-points-out">2.500 <span class="text-sm font-normal text-slate-300">PTS</span></div>
                            </div>
                            <div class="pt-3 border-t border-slate-800 flex justify-between items-center text-xs">
                                <span class="text-slate-400">Nilai Kesetaraan Rupiah:</span>
                                <span id="calc-rupiah-out" class="font-bold text-emerald-400 text-sm">Rp 2.500,-</span>
                            </div>
                        </div>
                    </div>
                </div>

                <!-- Reward Catalog -->
                <div class="space-y-4">
                    <div class="flex justify-between items-center">
                        <div>
                            <h3 class="text-xl font-bold text-slate-800">Katalog Penukaran Poin</h3>
                            <p class="text-xs text-slate-500">Tukarkan Eko-Poin SADEWA Anda dengan saldo digital atau bantuan desa.</p>
                        </div>
                    </div>

                    <div class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-3 gap-5">
                        <div class="bg-white rounded-2xl p-5 border border-slate-200/80 shadow-sm flex flex-col justify-between">
                            <div>
                                <div class="flex justify-between items-start mb-3">
                                    <div class="p-2.5 bg-emerald-100 text-emerald-700 rounded-xl">
                                        <i data-lucide="wallet" class="w-5 h-5"></i>
                                    </div>
                                    <span class="bg-amber-100 text-amber-800 font-bold text-[10px] px-2 py-0.5 rounded-full">10.000 PTS</span>
                                </div>
                                <h4 class="font-bold text-slate-800 text-sm">Voucher E-Wallet Rp 10.000</h4>
                                <p class="text-xs text-slate-500 mt-1">Saldo DANA, GoPay, atau OVO instant.</p>
                            </div>
                            <button onclick="redeemReward('Voucher E-Wallet Rp 10.000', 10000)" class="mt-4 w-full bg-slate-900 hover:bg-emerald-600 text-white text-xs font-bold py-2.5 rounded-xl transition">
                                Tukar Sekarang
                            </button>
                        </div>

                        <div class="bg-white rounded-2xl p-5 border border-slate-200/80 shadow-sm flex flex-col justify-between">
                            <div>
                                <div class="flex justify-between items-start mb-3">
                                    <div class="p-2.5 bg-blue-100 text-blue-700 rounded-xl">
                                        <i data-lucide="smartphone" class="w-5 h-5"></i>
                                    </div>
                                    <span class="bg-amber-100 text-amber-800 font-bold text-[10px] px-2 py-0.5 rounded-full">25.000 PTS</span>
                                </div>
                                <h4 class="font-bold text-slate-800 text-sm">Pulsa / Data All Operator Rp 25.000</h4>
                                <p class="text-xs text-slate-500 mt-1">Isi ulang pulsa reguler atau paket kuota internet.</p>
                            </div>
                            <button onclick="redeemReward('Pulsa All Operator Rp 25.000', 25000)" class="mt-4 w-full bg-slate-900 hover:bg-emerald-600 text-white text-xs font-bold py-2.5 rounded-xl transition">
                                Tukar Sekarang
                            </button>
                        </div>

                        <div class="bg-white rounded-2xl p-5 border border-slate-200/80 shadow-sm flex flex-col justify-between">
                            <div>
                                <div class="flex justify-between items-start mb-3">
                                    <div class="p-2.5 bg-teal-100 text-teal-700 rounded-xl">
                                        <i data-lucide="heart-handshake" class="w-5 h-5"></i>
                                    </div>
                                    <span class="bg-amber-100 text-amber-800 font-bold text-[10px] px-2 py-0.5 rounded-full">5.000 PTS</span>
                                </div>
                                <h4 class="font-bold text-slate-800 text-sm">Donasi Bibit Pohon Desa</h4>
                                <p class="text-xs text-slate-500 mt-1">Penanaman 2 bibit pohon di area resapan Kabupaten Bogor.</p>
                            </div>
                            <button onclick="redeemReward('Donasi Bibit Pohon Desa', 5000)" class="mt-4 w-full bg-slate-900 hover:bg-emerald-600 text-white text-xs font-bold py-2.5 rounded-xl transition">
                                Donasikan
                            </button>
                        </div>
                    </div>
                </div>
            </section>

            <!-- PAGE 5: LAPORAN & PENGADUAN -->
            <section id="page-pengaduan" class="page-view space-y-6">
                <div class="bg-white rounded-3xl border border-slate-200/80 shadow-sm p-6 sm:p-8 max-w-4xl mx-auto">
                    <div class="border-b border-slate-100 pb-5 mb-6">
                        <span class="inline-flex items-center space-x-1 text-rose-600 font-bold text-xs uppercase tracking-wider mb-1">
                            <i data-lucide="alert-triangle" class="w-4 h-4"></i>
                            <span>Pengawasan Lingkungan</span>
                        </span>
                        <h2 class="text-2xl font-extrabold text-slate-800">Pengaduan Penumpukan Sampah Liar</h2>
                        <p class="text-xs text-slate-500 mt-1">Laporkan adanya tempat pembuangan sampah (TPS) liar atau penumpukan sampah meluap di wilayah pemukiman Anda.</p>
                    </div>

                    <form id="form-pengaduan" onsubmit="handleComplaintSubmit(event)" class="space-y-5">
                        <div class="grid grid-cols-1 sm:grid-cols-2 gap-5">
                            <div>
                                <label class="block text-xs font-bold text-slate-700 uppercase tracking-wider mb-2">Nama Pelapor</label>
                                <input type="text" id="report-name" required placeholder="Nama Anda" class="w-full bg-slate-50 border border-slate-300 rounded-xl p-3 text-sm focus:ring-2 focus:ring-rose-500 focus:outline-none">
                            </div>
                            <div>
                                <label class="block text-xs font-bold text-slate-700 uppercase tracking-wider mb-2">Kontak / No. Telepon</label>
                                <input type="tel" id="report-phone" required placeholder="0812xxxx" class="w-full bg-slate-50 border border-slate-300 rounded-xl p-3 text-sm focus:ring-2 focus:ring-rose-500 focus:outline-none">
                            </div>
                        </div>

                        <div class="grid grid-cols-1 sm:grid-cols-2 gap-5">
                            <div>
                                <label class="block text-xs font-bold text-slate-700 uppercase tracking-wider mb-2">Lokasi Kecamatan</label>
                                <select id="report-district" class="w-full bg-slate-50 border border-slate-300 rounded-xl p-3 text-sm focus:ring-2 focus:ring-rose-500 focus:outline-none">
                                    <option value="Cibinong">Kec. Cibinong</option>
                                    <option value="Cileungsi">Kec. Cileungsi</option>
                                    <option value="Dramaga">Kec. Dramaga</option>
                                    <option value="Parung">Kec. Parung</option>
                                    <option value="Gunung Putri">Kec. Gunung Putri</option>
                                    <option value="Lainnya">Kecamatan Lainnya</option>
                                </select>
                            </div>
                            <div>
                                <label class="block text-xs font-bold text-slate-700 uppercase tracking-wider mb-2">Tingkat Darurat</label>
                                <select id="report-urgency" class="w-full bg-slate-50 border border-slate-300 rounded-xl p-3 text-sm focus:ring-2 focus:ring-rose-500 focus:outline-none">
                                    <option value="Sedang">Sedang (Penumpukan Biasa)</option>
                                    <option value="Tinggi">Tinggi (Menutup Jalan / Bau Menyengat)</option>
                                    <option value="Kritis">Kritis (TPS Liar Ilegal Besar)</option>
                                </select>
                            </div>
                        </div>

                        <div>
                            <label class="block text-xs font-bold text-slate-700 uppercase tracking-wider mb-2">Detail Laporan & Lokasi Persis</label>
                            <textarea id="report-detail" rows="4" required placeholder="Jelaskan kondisi tumpukan sampah dan petunjuk arah ke lokasi..." class="w-full bg-slate-50 border border-slate-300 rounded-xl p-3 text-sm focus:ring-2 focus:ring-rose-500 focus:outline-none"></textarea>
                        </div>

                        <button type="submit" class="w-full bg-rose-600 hover:bg-rose-700 text-white font-bold py-4 rounded-2xl shadow-lg shadow-rose-600/20 transition flex items-center justify-center space-x-2">
                            <i data-lucide="shield-alert" class="w-5 h-5"></i>
                            <span>Kirim Laporan Pengaduan</span>
                        </button>
                    </form>
                </div>
            </section>

            <!-- PAGE 6: ADMIN PANEL (EXCLUSIVE TO ADMIN ROLE) -->
            <section id="page-admin" class="page-view space-y-6">
                <div class="bg-gradient-to-r from-slate-900 to-amber-950 text-white p-6 rounded-3xl shadow-xl flex flex-col sm:flex-row justify-between items-start sm:items-center gap-4 border border-amber-500/20">
                    <div>
                        <span class="bg-amber-500/20 border border-amber-400/30 text-amber-300 text-[10px] font-extrabold px-3 py-1 rounded-full uppercase tracking-wider">
                            Otoritas Dinas Lingkungan Hidup
                        </span>
                        <h2 class="text-2xl font-extrabold text-white mt-2">Panel Manajemen & Verifikasi SADEWA</h2>
                        <p class="text-xs text-slate-300 mt-1">Kelola dan verifikasi pengajuan penjemputan sampah warga di seluruh Kecamatan Kab. Bogor.</p>
                    </div>
                </div>

                <!-- Admin Management Table -->
                <div class="bg-white rounded-3xl border border-slate-200/80 shadow-sm overflow-hidden">
                    <div class="p-5 border-b border-slate-100 flex justify-between items-center">
                        <h3 class="font-bold text-slate-800 text-base">Daftar Antrean Penjemputan Masuk</h3>
                        <span class="text-xs font-semibold text-slate-500">Total: <span id="admin-pickup-count">0</span> Permintaan</span>
                    </div>

                    <div class="overflow-x-auto">
                        <table class="w-full text-left text-xs">
                            <thead class="bg-slate-50 text-slate-600 uppercase font-bold tracking-wider border-b border-slate-200">
                                <tr>
                                    <th class="p-4">Pemohon</th>
                                    <th class="p-4">Lokasi & Tgl</th>
                                    <th class="p-4">Jenis / Berat</th>
                                    <th class="p-4">Status</th>
                                    <th class="p-4 text-center">Tindakan Admin</th>
                                </tr>
                            </thead>
                            <tbody id="admin-pickup-tbody" class="divide-y divide-slate-100 font-medium">
                                <!-- Dynamic pickup items -->
                            </tbody>
                        </table>
                    </div>
                </div>
            </section>

        </div>
    </main>

    <!-- JAVASCRIPT APP LOGIC -->
    <script>
        // Data Global Application
        let currentUserRole = 'warga'; // 'warga' or 'admin'
        let currentUserEmail = '';

        // Storage Poin terpisah per-email & database pengguna warga
        let userPointsDatabase = {}; 
        let registeredUsersSet = new Set();
        let totalSystemWeight = 0;
        let totalSystemDistributedPoints = 0;

        // Master Data Bank Sampah & TPS
        const tpsData = [
            { id: 1, name: "Bank Sampah Bersih Asri", kec: "Cibinong", address: "Jl. Raya Pemda No. 12, Cibinong", status: "Buka", hours: "08:00 - 15:00 WIB" },
            { id: 2, name: "TPS3R Mandiri Cileungsi", kec: "Cileungsi", address: "Desa Cileungsi Kidul RT 02/04", status: "Buka", hours: "08:00 - 16:00 WIB" },
            { id: 3, name: "Bank Sampah Kampus Dramaga", kec: "Dramaga", address: "Dekat Lingkar IPB Dramaga", status: "Buka", hours: "09:00 - 14:00 WIB" },
            { id: 4, name: "TPS3R Parung Hijau", kec: "Parung", address: "Jl. Pasar Parung Baru No. 45", status: "Buka", hours: "07:30 - 15:00 WIB" },
            { id: 5, name: "Bank Sampah Gunung Putri Eko", kec: "Gunung Putri", address: "Desa Tlajung Udik, Gunung Putri", status: "Tutup Temp", hours: "Buka Besok 08:00" },
            { id: 6, name: "TPS3R Sukahati Cibinong", kec: "Cibinong", address: "Kel. Sukahati, Cibinong", status: "Buka", hours: "08:00 - 16:00 WIB" }
        ];

        // Master Data Penjemputan & Pengaduan
        let pickupRequests = [];
        let complaintReports = [];

        // Helper Toast Notification System
        let toastTimeout = null;
        function showToast(title, msg) {
            const toast = document.getElementById('toast');
            document.getElementById('toast-title').textContent = title;
            document.getElementById('toast-msg').textContent = msg;

            toast.classList.remove('-translate-y-24', 'opacity-0');
            toast.classList.add('translate-y-0', 'opacity-100');

            if (toastTimeout) clearTimeout(toastTimeout);
            toastTimeout = setTimeout(() => {
                toast.classList.remove('translate-y-0', 'opacity-100');
                toast.classList.add('-translate-y-24', 'opacity-0');
            }, 3500);
        }

        // Navigation Tab Switcher SPA
        function switchTab(tabId) {
            document.querySelectorAll('.page-view').forEach(p => p.classList.remove('active-page'));
            
            const targetPage = document.getElementById(`page-${tabId}`);
            if (targetPage) targetPage.classList.add('active-page');

            document.querySelectorAll('.nav-item').forEach(btn => {
                btn.className = 'nav-item w-full flex items-center space-x-3 px-4 py-3 rounded-xl font-medium text-sm transition-all duration-200 text-slate-400 hover:bg-slate-800 hover:text-white';
            });

            const activeBtn = document.getElementById(`nav-${tabId}`);
            if (activeBtn) {
                if (tabId === 'admin') {
                    activeBtn.className = 'nav-item w-full flex items-center space-x-3 px-4 py-3 rounded-xl font-medium text-sm bg-amber-500 text-slate-950 font-bold shadow-lg shadow-amber-500/20';
                } else {
                    activeBtn.className = 'nav-item w-full flex items-center space-x-3 px-4 py-3 rounded-xl font-medium text-sm bg-emerald-600 text-white shadow-lg shadow-emerald-600/20';
                }
            }

            const sidebar = document.getElementById('sidebar');
            if (!sidebar.classList.contains('-translate-x-full')) {
                sidebar.classList.add('-translate-x-full');
            }
        }

        function toggleMobileSidebar() {
            const sidebar = document.getElementById('sidebar');
            sidebar.classList.toggle('-translate-x-full');
        }

        // Handle Universal Login Logic (Admin & Warga)
        function handleLogin(e) {
            e.preventDefault();
            const emailInput = document.getElementById('login-email');
            const passInput = document.getElementById('login-password');
            const errorAlert = document.getElementById('login-error-alert');
            const errorText = document.getElementById('login-error-text');

            const email = emailInput.value.trim().toLowerCase();
            const password = passInput.value.trim();

            function showError(message) {
                errorText.textContent = message;
                errorAlert.classList.remove('hidden');
                emailInput.classList.add('border-rose-500', 'bg-rose-50/50');
                passInput.classList.add('border-rose-500', 'bg-rose-50/50');
                showToast('Login Gagal', message);
            }

            function clearError() {
                errorAlert.classList.add('hidden');
                emailInput.classList.remove('border-rose-500', 'bg-rose-50/50');
                passInput.classList.remove('border-rose-500', 'bg-rose-50/50');
            }

            clearError();

            if (!email || !password) return;

            // Check if login as Admin
            if (email === 'admin@gmail.com') {
                if (password === 'admin12345') {
                    currentUserRole = 'admin';
                    currentUserEmail = email;
                    showToast('Login Admin Berhasil', 'Selamat datang kembali, Petugas DLH Kabupaten Bogor.');
                    applyUserRoleUI();
                    closeLoginModal();
                    switchTab('admin');
                    return;
                } else {
                    showError('Email atau kata sandi salah.');
                    return;
                }
            }

            // Check if login as Warga (Strict @gmail.com)
            if (!email.endsWith('@gmail.com')) {
                showError('Email atau kata sandi salah (Format email wajib @gmail.com).');
                return;
            }

            currentUserRole = 'warga';
            currentUserEmail = email;

            if (userPointsDatabase[email] === undefined) {
                userPointsDatabase[email] = 0;
            }

            registeredUsersSet.add(email);

            updateHomeDashboardStats();
            showToast('Login Berhasil', `Selamat datang, ${email.split('@')[0]}!`);
            applyUserRoleUI();
            closeLoginModal();
            switchTab('beranda');
        }

        function updateHomeDashboardStats() {
            document.getElementById('stat-total-weight').textContent = `${totalSystemWeight.toLocaleString('id-ID')} Kg`;
            document.getElementById('stat-total-points').textContent = `${totalSystemDistributedPoints.toLocaleString('id-ID')} PTS`;
            document.getElementById('stat-total-rupiah').textContent = `Setara Rp ${(totalSystemDistributedPoints).toLocaleString('id-ID')}`;
            document.getElementById('stat-total-users').textContent = `${registeredUsersSet.size} KK`;
        }

        function applyUserRoleUI() {
            const displayName = document.getElementById('user-display-name');
            const pointsBadge = document.getElementById('user-points-badge');
            const userAvatar = document.getElementById('user-avatar');
            const adminNavWrapper = document.getElementById('nav-admin-wrapper');

            if (currentUserRole === 'admin') {
                displayName.textContent = 'Admin DLH Bogor';
                pointsBadge.innerHTML = '<span class="bg-amber-500/20 text-amber-300 border border-amber-400/30 text-[9px] font-bold px-2 py-0.5 rounded-md">PETUGAS REGIONAL</span>';
                userAvatar.className = 'w-10 h-10 rounded-xl bg-gradient-to-br from-amber-500 to-amber-700 text-white flex items-center justify-center font-bold shadow-inner shrink-0';
                adminNavWrapper.classList.remove('hidden');
                renderAdminPanel();
            } else {
                const activePoints = userPointsDatabase[currentUserEmail] || 0;
                displayName.textContent = currentUserEmail ? currentUserEmail.split('@')[0] : 'Warga Eko-Aktif';
                pointsBadge.innerHTML = `<i data-lucide="coins" class="w-3 h-3 inline mr-1"></i> <span id="sidebar-user-points">${activePoints.toLocaleString('id-ID')} PTS</span>`;
                userAvatar.className = 'w-10 h-10 rounded-xl bg-gradient-to-br from-emerald-500 to-teal-600 text-white flex items-center justify-center font-bold shadow-inner shrink-0';
                adminNavWrapper.classList.add('hidden');
            }

            lucide.createIcons();
        }

        function closeLoginModal() {
            document.getElementById('login-modal').classList.add('hidden');
        }

        function handleLogout() {
            currentUserRole = 'warga';
            currentUserEmail = '';
            document.getElementById('login-form').reset();
            document.getElementById('login-modal').classList.remove('hidden');
            showToast('Berhasil Logout', 'Anda telah keluar dari aplikasi SADEWA.');
        }

        function renderTPS(filter = 'semua') {
            const container = document.getElementById('tps-container');
            if (!container) return;

            const filtered = filter === 'semua' ? tpsData : tpsData.filter(item => item.kec.toLowerCase() === filter.toLowerCase());

            if (filtered.length === 0) {
                container.innerHTML = `
                    <div class="col-span-full bg-white p-8 rounded-2xl text-center text-slate-400">
                        <i data-lucide="map-pin-off" class="w-10 h-10 mx-auto mb-2 opacity-50"></i>
                        <p class="font-semibold text-sm">Tidak ada Bank Sampah / TPS3R terdaftar di kecamatan ini.</p>
                    </div>
                `;
                lucide.createIcons();
                return;
            }

            container.innerHTML = filtered.map(item => `
                <div class="bg-white rounded-2xl p-5 border border-slate-200/80 shadow-sm hover:shadow-md transition">
                    <div class="flex justify-between items-start mb-3">
                        <span class="bg-emerald-100 text-emerald-800 font-bold text-[10px] px-2.5 py-1 rounded-full uppercase tracking-wider">
                            Kec. ${item.kec}
                        </span>
                        <span class="${item.status === 'Buka' ? 'bg-emerald-50 text-emerald-600 border border-emerald-200' : 'bg-rose-50 text-rose-600 border border-rose-200'} font-bold text-[10px] px-2 py-0.5 rounded-md">
                            ${item.status}
                        </span>
                    </div>
                    <h3 class="font-bold text-slate-800 text-base">${item.name}</h3>
                    <p class="text-xs text-slate-500 mt-1 flex items-start">
                        <i data-lucide="map-pin" class="w-3.5 h-3.5 text-slate-400 mr-1 mt-0.5 shrink-0"></i>
                        <span>${item.address}</span>
                    </p>
                    <div class="mt-4 pt-3 border-t border-slate-100 flex justify-between items-center text-xs text-slate-500">
                        <span class="flex items-center"><i data-lucide="clock" class="w-3.5 h-3.5 mr-1 text-emerald-600"></i> ${item.hours}</span>
                        <button onclick="switchTab('penjemputan')" class="text-emerald-600 font-bold hover:underline">Antar ke Sini</button>
                    </div>
                </div>
            `).join('');
            lucide.createIcons();
        }

        function filterTPS() {
            const selected = document.getElementById('filter-kecamatan').value;
            renderTPS(selected);
        }

        function runCalculator() {
            const catSelect = document.getElementById('calc-cat');
            const rate = parseFloat(catSelect.value) || 500;
            const weight = parseFloat(document.getElementById('calc-range').value) || 5;

            document.getElementById('calc-weight-label').textContent = `${weight} Kg`;

            const points = weight * rate;
            const rupiahEquivalent = points; // 1 PTS = Rp 1,-

            document.getElementById('calc-points-out').innerHTML = `${points.toLocaleString('id-ID')} <span class="text-sm font-normal text-slate-300">PTS</span>`;
            document.getElementById('calc-rupiah-out').textContent = `Rp ${rupiahEquivalent.toLocaleString('id-ID')},-`;
        }

        function handlePickupSubmit(e) {
            e.preventDefault();
            const name = document.getElementById('pickup-name').value;
            const phone = document.getElementById('pickup-phone').value;
            const district = document.getElementById('pickup-district').value;
            const date = document.getElementById('pickup-date').value;
            
            const typeSelect = document.getElementById('pickup-type');
            const type = typeSelect.value;
            const rate = parseFloat(typeSelect.options[typeSelect.selectedIndex].getAttribute('data-rate')) || 500;
            const weight = parseFloat(document.getElementById('pickup-weight').value);

            if (weight < 5) {
                showToast('Gagal Pengajuan', 'Minimal berat penjemputan adalah 5 Kg.');
                return;
            }

            const earnedPoints = weight * rate;
            
            if (currentUserEmail && userPointsDatabase[currentUserEmail] !== undefined) {
                userPointsDatabase[currentUserEmail] += earnedPoints;
            }

            totalSystemWeight += weight;
            totalSystemDistributedPoints += earnedPoints;

            pickupRequests.unshift({
                id: Date.now(),
                name, phone, district, date, type, weight, earnedPoints, status: "Menunggu"
            });

            updateHomeDashboardStats();
            applyUserRoleUI();
            showToast('Pengajuan Terkirim!', `Jadwal penjemputan ${type} (${weight} Kg) berhasil! Anda mendapatkan +${earnedPoints.toLocaleString('id-ID')} PTS!`);
            document.getElementById('form-penjemputan').reset();
            
            if (currentUserRole === 'admin') renderAdminPanel();
        }

        function handleComplaintSubmit(e) {
            e.preventDefault();
            const name = document.getElementById('report-name').value;
            const district = document.getElementById('report-district').value;
            const urgency = document.getElementById('report-urgency').value;

            complaintReports.unshift({
                id: Date.now(),
                name, district, urgency, date: new Date().toLocaleDateString('id-ID')
            });

            showToast('Laporan Diterima', `Laporan penumpukan sampah di ${district} (${urgency}) telah diteruskan ke Petugas Satgas DLH.`);
            document.getElementById('form-pengaduan').reset();
        }

        function renderAdminPanel() {
            const countSpan = document.getElementById('admin-pickup-count');
            const tbody = document.getElementById('admin-pickup-tbody');
            if (!tbody || !countSpan) return;

            countSpan.textContent = pickupRequests.length;

            if (pickupRequests.length === 0) {
                tbody.innerHTML = `
                    <tr>
                        <td colspan="5" class="p-8 text-center text-slate-400 font-semibold">
                            Belum ada permintaan penjemputan sampah dari warga.
                        </td>
                    </tr>
                `;
                return;
            }

            tbody.innerHTML = pickupRequests.map(item => `
                <tr class="hover:bg-slate-50 transition">
                    <td class="p-4">
                        <p class="font-bold text-slate-800">${item.name}</p>
                        <p class="text-[11px] text-slate-400">${item.phone}</p>
                    </td>
                    <td class="p-4">
                        <p class="font-semibold text-slate-700">Kec. ${item.district}</p>
                        <p class="text-[11px] text-slate-400">${item.date}</p>
                    </td>
                    <td class="p-4">
                        <p class="font-semibold text-slate-800">${item.type}</p>
                        <p class="text-[11px] text-emerald-600 font-bold">${item.weight} Kg (+${item.earnedPoints.toLocaleString('id-ID')} PTS)</p>
                    </td>
                    <td class="p-4">
                        <span class="${item.status === 'Selesai' ? 'bg-emerald-100 text-emerald-800' : 'bg-amber-100 text-amber-800'} font-bold text-[10px] px-2.5 py-1 rounded-full">
                            ${item.status}
                        </span>
                    </td>
                    <td class="p-4 text-center">
                        ${item.status === 'Menunggu' ? `
                            <button onclick="approvePickup(${item.id})" class="bg-emerald-600 hover:bg-emerald-700 text-white px-3 py-1.5 rounded-lg text-xs font-bold transition">
                                Verifikasi & Selesai
                            </button>
                        ` : `
                            <span class="text-[11px] text-slate-400 font-semibold"><i data-lucide="check-check" class="w-4 h-4 inline text-emerald-500 mr-1"></i>Terverifikasi</span>
                        `}
                    </td>
                </tr>
            `).join('');
            lucide.createIcons();
        }

        function approvePickup(id) {
            const req = pickupRequests.find(r => r.id === id);
            if (req) {
                req.status = 'Selesai';
                showToast('Verifikasi Sukses', `Permintaan penjemputan a.n ${req.name} telah disetujui.`);
                renderAdminPanel();
            }
        }

        function redeemReward(itemName, cost) {
            const currentPoints = userPointsDatabase[currentUserEmail] || 0;
            if (currentPoints >= cost) {
                userPointsDatabase[currentUserEmail] -= cost;
                applyUserRoleUI();
                showToast('Penukaran Berhasil!', `Selamat, ${itemName} berhasil ditukarkan. Sisa poin: ${userPointsDatabase[currentUserEmail].toLocaleString('id-ID')} PTS.`);
            } else {
                showToast('Poin Tidak Cukup', `Anda membutuhkan ${cost.toLocaleString('id-ID')} PTS. Saldo Anda: ${currentPoints.toLocaleString('id-ID')} PTS.`);
            }
        }

        let peekTimeout = null;
        function peekPassword() {
            const passInput = document.getElementById('login-password');
            const icon = document.getElementById('password-toggle-icon');
            
            if (peekTimeout) clearTimeout(peekTimeout);

            passInput.type = 'text';
            icon.setAttribute('data-lucide', 'eye-off');
            lucide.createIcons();

            peekTimeout = setTimeout(() => {
                passInput.type = 'password';
                icon.setAttribute('data-lucide', 'eye');
                lucide.createIcons();
            }, 1500);
        }

        // Initialize App
        window.addEventListener('DOMContentLoaded', () => {
            lucide.createIcons();
            renderTPS();
            runCalculator();
            updateHomeDashboardStats();
        });
    </script>
</body>
</html>
