<!DOCTYPE html>
<html lang="id" class="scroll-smooth">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>SADEWA Bogor - Sistem Aplikasi Daur-ulang & Eko-pelayanan Wilayah Antar-desa</title>

    <!-- Tailwind CSS CDN -->
    <script src="https://cdn.tailwindcss.com"></script>
    <script>
        tailwind.config = {
            theme: {
                extend: {
                    colors: {
                        eco: {
                            50: '#f0fdf4',
                            100: '#dcfce7',
                            200: '#bbf7d0',
                            500: '#22c55e',
                            600: '#16a34a',
                            700: '#15803d',
                            800: '#166534',
                            900: '#14532d',
                        }
                    }
                }
            }
        }
    </script>

    <!-- Leaflet.js CSS & JS for Interactive Map -->
    <link rel="stylesheet" href="https://unpkg.com/leaflet@1.9.4/dist/leaflet.css" />
    <script src="https://unpkg.com/leaflet@1.9.4/dist/leaflet.js"></script>

    <!-- Chart.js CDN -->
    <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>

    <!-- FontAwesome CDN -->
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">

    <!-- Google Fonts -->
    <link rel="preconnect" href="https://fonts.googleapis.com">
    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
    <link href="https://fonts.googleapis.com/css2?family=Plus+Jakarta+Sans:wght@400;500;600;700;800&display=swap" rel="stylesheet">

    <style>
        body { font-family: 'Plus Jakarta Sans', sans-serif; }
        .glass-card {
            background: rgba(255, 255, 255, 0.85);
            backdrop-filter: blur(12px);
            border: 1px solid rgba(255, 255, 255, 0.3);
        }
        #map { z-index: 1; }
    </style>
</head>
<body class="bg-slate-50 text-slate-800 flex flex-col min-h-screen">

    <!-- Disclaimer Banner -->
    <div class="bg-amber-500 text-slate-900 text-xs py-1.5 px-4 text-center font-semibold tracking-wide">
        ⚠️ PROTOTYPE INTERAKTIF SADEWA — Simulasi Pelayanan Pengelolaan Sampah Desa (Bukan Portal Resmi Pemkab Bogor)
    </div>

    <!-- Header Navigation -->
    <header class="sticky top-0 z-50 bg-white/90 backdrop-blur-md border-b border-slate-100 shadow-sm">
        <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 h-16 flex items-center justify-between">
            <div class="flex items-center space-x-3 cursor-pointer" onclick="scrollToTop()">
                <div class="w-10 h-10 rounded-xl bg-eco-600 text-white flex items-center justify-center font-black text-xl shadow-md shadow-eco-500/20">
                    <i class="fa-solid fa-recycle"></i>
                </div>
                <div>
                    <span class="font-extrabold text-2xl tracking-tight text-slate-900">SADEWA<span class="text-eco-600">.</span></span>
                    <span class="block text-[10px] text-slate-500 font-medium tracking-wider uppercase -mt-1">Eko-Pelayanan Desa Bogor</span>
                </div>
            </div>

            <nav class="hidden md:flex items-center space-x-1 font-medium text-sm text-slate-600">
                <a href="#pilih-wilayah" class="px-3 py-2 rounded-lg hover:text-eco-600 hover:bg-eco-50 transition">Wilayah Desa</a>
                <a href="#jadwal" class="px-3 py-2 rounded-lg hover:text-eco-600 hover:bg-eco-50 transition">Jadwal</a>
                <a href="#fasilitas-peta" class="px-3 py-2 rounded-lg hover:text-eco-600 hover:bg-eco-50 transition">Peta Fasilitas</a>
                <a href="#jemput-sampah" class="px-3 py-2 rounded-lg hover:text-eco-600 hover:bg-eco-50 transition">Jemput Sampah</a>
                <a href="#lapor-masalah" class="px-3 py-2 rounded-lg hover:text-eco-600 hover:bg-eco-50 transition">Lapor</a>
                <a href="#ecopoints" class="px-3 py-2 rounded-lg hover:text-eco-600 hover:bg-eco-50 transition">EcoPoints</a>
                <a href="#admin" class="ml-2 px-4 py-2 rounded-xl bg-slate-900 text-white font-semibold hover:bg-eco-700 transition shadow-sm flex items-center space-x-2">
                    <i class="fa-solid fa-user-shield text-xs"></i>
                    <span>Portal Admin</span>
                </a>
            </nav>

            <button onclick="toggleMobileMenu()" class="md:hidden text-slate-700 p-2 focus:outline-none">
                <i class="fa-solid fa-bars text-xl" id="menu-icon"></i>
            </button>
        </div>

        <!-- Mobile Menu -->
        <div id="mobile-menu" class="hidden md:hidden bg-white border-b border-slate-200 px-4 pt-2 pb-4 space-y-2">
            <a href="#pilih-wilayah" onclick="toggleMobileMenu()" class="block px-3 py-2 rounded-lg text-slate-700 hover:bg-eco-50">Wilayah Desa</a>
            <a href="#jadwal" onclick="toggleMobileMenu()" class="block px-3 py-2 rounded-lg text-slate-700 hover:bg-eco-50">Jadwal Pengangkutan</a>
            <a href="#fasilitas-peta" onclick="toggleMobileMenu()" class="block px-3 py-2 rounded-lg text-slate-700 hover:bg-eco-50">Peta Fasilitas</a>
            <a href="#jemput-sampah" onclick="toggleMobileMenu()" class="block px-3 py-2 rounded-lg text-slate-700 hover:bg-eco-50">Jemput Sampah</a>
            <a href="#lapor-masalah" onclick="toggleMobileMenu()" class="block px-3 py-2 rounded-lg text-slate-700 hover:bg-eco-50">Lapor Masalah</a>
            <a href="#ecopoints" onclick="toggleMobileMenu()" class="block px-3 py-2 rounded-lg text-slate-700 hover:bg-eco-50">EcoPoints</a>
            <a href="#admin" onclick="toggleMobileMenu()" class="block px-3 py-2 rounded-lg bg-slate-900 text-white font-semibold text-center">Portal Admin Desa</a>
        </div>
    </header>

    <!-- Hero Section -->
    <section class="relative bg-gradient-to-b from-eco-50 via-white to-slate-50 pt-12 pb-16 px-4 sm:px-6 lg:px-8 overflow-hidden">
        <div class="max-w-4xl mx-auto text-center relative z-10">
            <span class="inline-flex items-center space-x-2 px-3.5 py-1.5 rounded-full bg-eco-100 text-eco-800 text-xs font-bold tracking-wide uppercase mb-4">
                <i class="fa-solid fa-leaf"></i>
                <span>SADEWA — Sistem Aplikasi Daur-ulang & Eko-pelayanan Wilayah Antar-desa</span>
            </span>
            <h1 class="text-4xl sm:text-5xl font-extrabold text-slate-900 tracking-tight leading-tight">
                Kelola Sampah, Jaga Desa, Bersama.
            </h1>
            <p class="mt-4 text-base sm:text-lg text-slate-600 max-w-2xl mx-auto">
                Platform SADEWA menghadirkan pelayanan kebersihan terpadu tingkat desa di Kabupaten Bogor. Pilih wilayah Anda untuk memantau jadwal, kirim laporan, dan kumpulkan poin apresiasi.
            </p>

            <!-- Selection Component -->
            <div id="pilih-wilayah" class="mt-8 bg-white p-6 sm:p-8 rounded-3xl shadow-xl border border-slate-100 max-w-2xl mx-auto text-left">
                <h2 class="text-sm font-bold text-slate-400 uppercase tracking-wider mb-4 flex items-center">
                    <i class="fa-solid fa-location-dot text-eco-600 mr-2"></i> Pilih Wilayah Anda
                </h2>
                <div class="grid grid-cols-1 sm:grid-cols-2 gap-4">
                    <div>
                        <label class="block text-xs font-bold text-slate-600 mb-1">Pilih Kecamatan</label>
                        <select id="select-kecamatan" onchange="onKecamatanChange()" class="w-full bg-slate-50 border border-slate-200 text-slate-800 rounded-xl px-4 py-3 font-semibold focus:outline-none focus:ring-2 focus:ring-eco-500">
                            <option value="">-- Pilih Kecamatan --</option>
                        </select>
                    </div>
                    <div>
                        <label class="block text-xs font-bold text-slate-600 mb-1">Pilih Desa / Kelurahan</label>
                        <select id="select-desa" onchange="onDesaChange()" disabled class="w-full bg-slate-50 border border-slate-200 text-slate-800 rounded-xl px-4 py-3 font-semibold focus:outline-none focus:ring-2 focus:ring-eco-500 disabled:opacity-50">
                            <option value="">-- Pilih Desa --</option>
                        </select>
                    </div>
                </div>
            </div>
        </div>
    </section>

    <!-- Dynamic Village Header & Status Summary -->
    <section class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 -mt-6 mb-12">
        <div class="bg-white rounded-3xl p-6 sm:p-8 shadow-lg border border-slate-100">
            <div class="flex flex-col md:flex-row md:items-center justify-between pb-6 border-b border-slate-100 gap-4">
                <div>
                    <div class="inline-flex items-center text-xs font-bold text-eco-700 bg-eco-100 px-3 py-1 rounded-full mb-1">
                        📍 Layanan SADEWA Aktif
                    </div>
                    <h2 class="text-2xl sm:text-3xl font-extrabold text-slate-900" id="display-desa">Desa Rawa Panjang</h2>
                    <p class="text-slate-500 font-medium text-sm" id="display-kecamatan">Kecamatan Bojong Gede, Kabupaten Bogor</p>
                </div>
                <div class="bg-slate-50 px-4 py-3 rounded-2xl border border-slate-200 text-xs text-slate-600 flex items-center space-x-3">
                    <div class="w-3 h-3 rounded-full bg-emerald-500 animate-pulse"></div>
                    <div>
                        <span class="block font-bold text-slate-800">Status Operasional Hari Ini</span>
                        <span id="display-status-hari-ini" class="text-emerald-600 font-semibold">🟢 Pengangkutan Berjalan Normal</span>
                    </div>
                </div>
            </div>

            <!-- Metric Cards -->
            <div class="grid grid-cols-2 lg:grid-cols-4 gap-4 mt-6">
                <div class="bg-eco-50/60 p-4 sm:p-5 rounded-2xl border border-eco-100">
                    <span class="text-2xl">🟢</span>
                    <h4 class="text-xs font-bold text-slate-500 uppercase mt-2">Jadwal Hari Ini</h4>
                    <p class="text-lg font-extrabold text-slate-800 mt-1" id="stat-jadwal-today">Organik & Residu</p>
                </div>

                <div class="bg-amber-50 p-4 sm:p-5 rounded-2xl border border-amber-100">
                    <span class="text-2xl">🟡</span>
                    <h4 class="text-xs font-bold text-slate-500 uppercase mt-2">Laporan Ditangani</h4>
                    <p class="text-lg font-extrabold text-slate-800 mt-1" id="stat-laporan-pending">3 Laporan</p>
                </div>

                <div class="bg-blue-50 p-4 sm:p-5 rounded-2xl border border-blue-100">
                    <span class="text-2xl">♻️</span>
                    <h4 class="text-xs font-bold text-slate-500 uppercase mt-2">Terkumpul Minggu Ini</h4>
                    <p class="text-lg font-extrabold text-slate-800 mt-1" id="stat-sampah-minggu">245 kg</p>
                </div>

                <div class="bg-purple-50 p-4 sm:p-5 rounded-2xl border border-purple-100">
                    <span class="text-2xl">👥</span>
                    <h4 class="text-xs font-bold text-slate-500 uppercase mt-2">Warga Berpartisipasi</h4>
                    <p class="text-lg font-extrabold text-slate-800 mt-1" id="stat-warga-aktif">128 Warga</p>
                </div>
            </div>
        </div>
    </section>

    <!-- Main Content Container -->
    <main class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 space-y-16 mb-20 flex-grow">

        <!-- Schedule Section -->
        <section id="jadwal" class="scroll-mt-24">
            <div class="flex flex-col md:flex-row md:items-end justify-between mb-6">
                <div>
                    <h3 class="text-2xl font-bold text-slate-900">Jadwal Pengangkutan Sampah</h3>
                    <p class="text-slate-500 text-sm mt-1">Jadwal armada truk & motor kaisar desa per minggu</p>
                </div>
                <span class="text-xs text-eco-700 bg-eco-100 px-3 py-1 rounded-lg font-semibold mt-2 md:mt-0 w-fit">
                    Desa: <span id="jadwal-desa-tag">Rawa Panjang</span>
                </span>
            </div>

            <div class="bg-white rounded-3xl shadow-sm border border-slate-200 overflow-hidden">
                <div class="overflow-x-auto">
                    <table class="w-full text-left text-sm">
                        <thead class="bg-slate-50 text-slate-600 font-bold uppercase text-xs border-b border-slate-200">
                            <tr>
                                <th class="py-4 px-6">Hari</th>
                                <th class="py-4 px-6">Jenis Sampah</th>
                                <th class="py-4 px-6">Jam Operasional</th>
                                <th class="py-4 px-6">Armada</th>
                                <th class="py-4 px-6">Status Hari Ini</th>
                            </tr>
                        </thead>
                        <tbody id="table-jadwal-body" class="divide-y divide-slate-100 text-slate-700 font-medium">
                            <!-- Dynamic Schedule Rows -->
                        </tbody>
                    </table>
                </div>
            </div>
        </section>

        <!-- Facilities Map -->
        <section id="fasilitas-peta" class="scroll-mt-24">
            <div class="mb-6">
                <h3 class="text-2xl font-bold text-slate-900">Peta Fasilitas Kebersihan Desa</h3>
                <p class="text-slate-500 text-sm mt-1">Lokasi Bank Sampah, TPS3R, dan Pusat Daur Ulang terdekat</p>
            </div>

            <div class="bg-white p-4 sm:p-6 rounded-3xl shadow-sm border border-slate-200 space-y-4">
                <div class="flex flex-wrap gap-2">
                    <button onclick="filterMap('Semua')" class="btn-filter px-4 py-2 rounded-xl text-xs font-bold bg-slate-900 text-white transition">Semua</button>
                    <button onclick="filterMap('Bank Sampah')" class="btn-filter px-4 py-2 rounded-xl text-xs font-bold bg-slate-100 text-slate-600 hover:bg-slate-200 transition">Bank Sampah</button>
                    <button onclick="filterMap('TPS')" class="btn-filter px-4 py-2 rounded-xl text-xs font-bold bg-slate-100 text-slate-600 hover:bg-slate-200 transition">TPS / TPS3R</button>
                    <button onclick="filterMap('Daur Ulang')" class="btn-filter px-4 py-2 rounded-xl text-xs font-bold bg-slate-100 text-slate-600 hover:bg-slate-200 transition">Daur Ulang</button>
                </div>
                
                <div id="map" class="h-96 rounded-2xl border border-slate-200 w-full"></div>
            </div>
        </section>

        <!-- Pickup Form & Citizen Reports Section (2 Columns) -->
        <div class="grid grid-cols-1 lg:grid-cols-2 gap-8">

            <!-- Request Pickup Form -->
            <section id="jemput-sampah" class="scroll-mt-24">
                <div class="bg-white p-6 sm:p-8 rounded-3xl shadow-sm border border-slate-200 h-full flex flex-col justify-between">
                    <div>
                        <div class="flex items-center space-x-3 mb-4">
                            <div class="w-10 h-10 rounded-2xl bg-eco-100 text-eco-700 flex items-center justify-center font-bold text-lg">
                                🚚
                            </div>
                            <div>
                                <h3 class="text-xl font-bold text-slate-900">Fitur Jemput Sampah SADEWA</h3>
                                <p class="text-xs text-slate-500">Layanan penjemputan sampah terpilah langsung dari rumah</p>
                            </div>
                        </div>

                        <form id="form-jemput" onsubmit="handleJemputSubmit(event)" class="space-y-4 text-xs font-medium text-slate-700 mt-6">
                            <div class="grid grid-cols-2 gap-3">
                                <div>
                                    <label class="block font-bold mb-1">Kecamatan</label>
                                    <input type="text" id="jemput-kecamatan" readonly class="w-full bg-slate-100 border border-slate-200 rounded-xl px-3 py-2.5 font-semibold text-slate-600">
                                </div>
                                <div>
                                    <label class="block font-bold mb-1">Desa</label>
                                    <input type="text" id="jemput-desa" readonly class="w-full bg-slate-100 border border-slate-200 rounded-xl px-3 py-2.5 font-semibold text-slate-600">
                                </div>
                            </div>

                            <div>
                                <label class="block font-bold mb-1">Alamat Penjemputan / RT RW</label>
                                <input type="text" id="jemput-alamat" required placeholder="Contoh: Jl. Mawar No. 12 RT 02/04" class="w-full bg-slate-50 border border-slate-200 rounded-xl px-3 py-2.5 focus:ring-2 focus:ring-eco-500 focus:outline-none">
                            </div>

                            <div class="grid grid-cols-2 gap-3">
                                <div>
                                    <label class="block font-bold mb-1">Jenis Sampah</label>
                                    <select id="jemput-jenis" required class="w-full bg-slate-50 border border-slate-200 rounded-xl px-3 py-2.5 focus:ring-2 focus:ring-eco-500 focus:outline-none">
                                        <option value="Organik">Organik</option>
                                        <option value="Plastik">Plastik Daur Ulang</option>
                                        <option value="Kertas">Kertas / Kardus</option>
                                        <option value="Logam">Logam / Kaleng</option>
                                        <option value="Elektronik">Elektronik / B3</option>
                                    </select>
                                </div>
                                <div>
                                    <label class="block font-bold mb-1">Perkiraan Berat (kg)</label>
                                    <input type="number" id="jemput-berat" min="1" max="100" required placeholder="Contoh: 5" class="w-full bg-slate-50 border border-slate-200 rounded-xl px-3 py-2.5 focus:ring-2 focus:ring-eco-500 focus:outline-none">
                                </div>
                            </div>

                            <div>
                                <label class="block font-bold mb-1">Rencana Tanggal Penjemputan</label>
                                <input type="date" id="jemput-tanggal" required class="w-full bg-slate-50 border border-slate-200 rounded-xl px-3 py-2.5 focus:ring-2 focus:ring-eco-500 focus:outline-none">
                            </div>

                            <button type="submit" class="w-full bg-eco-600 hover:bg-eco-700 text-white font-bold py-3 rounded-xl shadow-lg shadow-eco-600/20 transition text-sm">
                                Ajukan Penjemputan Sekarang
                            </button>
                        </form>
                    </div>
                </div>
            </section>

            <!-- Report Waste Form & Feed -->
            <section id="lapor-masalah" class="scroll-mt-24">
                <div class="bg-white p-6 sm:p-8 rounded-3xl shadow-sm border border-slate-200 h-full flex flex-col justify-between">
                    <div>
                        <div class="flex items-center space-x-3 mb-4">
                            <div class="w-10 h-10 rounded-2xl bg-amber-100 text-amber-700 flex items-center justify-center font-bold text-lg">
                                📢
                            </div>
                            <div>
                                <h3 class="text-xl font-bold text-slate-900">Lapor Sampah Liar / Menumpuk</h3>
                                <p class="text-xs text-slate-500">Laporkan titik penumpukan sampah liar agar segera ditindaklanjuti</p>
                            </div>
                        </div>

                        <form id="form-lapor" onsubmit="handleLaporSubmit(event)" class="space-y-4 text-xs font-medium text-slate-700 mt-6">
                            <div>
                                <label class="block font-bold mb-1">Kategori Masalah</label>
                                <select id="lapor-kategori" required class="w-full bg-slate-50 border border-slate-200 rounded-xl px-3 py-2.5 focus:ring-2 focus:ring-eco-500 focus:outline-none">
                                    <option value="Sampah Menumpuk">Sampah Menumpuk</option>
                                    <option value="TPS Liar">TPS Liar Baru</option>
                                    <option value="Sampah di Sungai / Saluran">Sampah di Sungai / Saluran</option>
                                    <option value="Terlambat Diangkut">Terlambat Diangkut</option>
                                </select>
                            </div>

                            <div>
                                <label class="block font-bold mb-1">Detail Lokasi / Patokan</label>
                                <input type="text" id="lapor-lokasi" required placeholder="Contoh: Depan lapangan pertigaan RT 03" class="w-full bg-slate-50 border border-slate-200 rounded-xl px-3 py-2.5 focus:ring-2 focus:ring-eco-500 focus:outline-none">
                            </div>

                            <div>
                                <label class="block font-bold mb-1">Deskripsi Masalah</label>
                                <textarea id="lapor-deskripsi" rows="2" required placeholder="Jelaskan kondisi penumpukan sampah..." class="w-full bg-slate-50 border border-slate-200 rounded-xl px-3 py-2.5 focus:ring-2 focus:ring-eco-500 focus:outline-none"></textarea>
                            </div>

                            <div>
                                <label class="block font-bold mb-1">Lampirkan Foto Simulasi</label>
                                <input type="text" id="lapor-foto" placeholder="URL Foto (opsional, default gambar simulasi)" class="w-full bg-slate-50 border border-slate-200 rounded-xl px-3 py-2.5 focus:ring-2 focus:ring-eco-500 focus:outline-none">
                            </div>

                            <button type="submit" class="w-full bg-slate-900 hover:bg-slate-800 text-white font-bold py-3 rounded-xl transition text-sm">
                                Kirim Laporan Masyarakat
                            </button>
                        </form>
                    </div>
                </div>
            </section>
        </div>

        <!-- Recent Activity Feed (Public Reports) -->
        <section class="bg-white p-6 sm:p-8 rounded-3xl shadow-sm border border-slate-200">
            <h3 class="text-xl font-bold text-slate-900 mb-2">Laporan & Tiket Terbaru SADEWA di <span id="feed-desa-title">Desa Rawa Panjang</span></h3>
            <p class="text-xs text-slate-500 mb-6">Status terkini penanganan laporan dan tiket penjemputan warga</p>

            <div id="public-reports-feed" class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-4">
                <!-- Dynamic Citizen Feed Items -->
            </div>
        </section>

        <!-- EcoPoints & Impact Statistics Section -->
        <div class="grid grid-cols-1 lg:grid-cols-3 gap-8">
            <!-- EcoPoints -->
            <section id="ecopoints" class="lg:col-span-1 scroll-mt-24">
                <div class="bg-gradient-to-br from-eco-800 to-slate-900 text-white p-6 sm:p-8 rounded-3xl shadow-xl h-full flex flex-col justify-between">
                    <div>
                        <div class="flex items-center justify-between mb-4">
                            <span class="text-xs font-bold uppercase tracking-wider bg-white/10 px-3 py-1 rounded-full text-eco-200">
                                EcoPoints SADEWA
                            </span>
                            <i class="fa-solid fa-coins text-amber-400 text-2xl"></i>
                        </div>
                        <h3 class="text-xl font-bold">Poin Apresiasi Warga</h3>
                        <p class="text-xs text-slate-300 mt-1">Kumpulkan poin dengan terus berpartisipasi menjaga kebersihan desa.</p>

                        <div class="mt-6 bg-white/10 backdrop-blur-md p-4 rounded-2xl border border-white/10 text-center">
                            <span class="text-xs text-slate-300 font-medium block">Total EcoPoints Warga <span id="ecopoint-desa-label">Desa Rawa Panjang</span></span>
                            <span class="text-3xl font-extrabold text-amber-400 mt-1 block" id="ecopoint-total">1.250 Poin</span>
                        </div>

                        <div class="mt-6 space-y-2.5 text-xs">
                            <div class="flex justify-between items-center py-1.5 border-b border-white/10">
                                <span>♻️ Setor Sampah Plastik</span>
                                <span class="font-bold text-eco-300">+50 poin</span>
                            </div>
                            <div class="flex justify-between items-center py-1.5 border-b border-white/10">
                                <span>📦 Setor Dus / Kertas</span>
                                <span class="font-bold text-eco-300">+30 poin</span>
                            </div>
                            <div class="flex justify-between items-center py-1.5 border-b border-white/10">
                                <span>🗑️ Lapor Penumpukan</span>
                                <span class="font-bold text-eco-300">+20 poin</span>
                            </div>
                            <div class="flex justify-between items-center py-1.5">
                                <span>🌱 Kerja Bakti Desa</span>
                                <span class="font-bold text-eco-300">+100 poin</span>
                            </div>
                        </div>
                    </div>

                    <div class="mt-6 pt-4 border-t border-white/10 text-center">
                        <p class="text-[11px] text-slate-300">Poin dapat ditukarkan voucher sembako atau pupuk organik di Kantor Desa setempat.</p>
                    </div>
                </div>
            </section>

            <!-- Impact Statistics & Chart -->
            <section class="lg:col-span-2">
                <div class="bg-white p-6 sm:p-8 rounded-3xl shadow-sm border border-slate-200 h-full flex flex-col justify-between space-y-6">
                    <div>
                        <div class="flex items-center justify-between mb-4">
                            <div>
                                <h3 class="text-xl font-bold text-slate-900">Dampak Lingkungan Desa</h3>
                                <p class="text-xs text-slate-500">Statistik realisasi pengelolaan sampah tingkat desa</p>
                            </div>
                            <span class="text-xs font-bold text-slate-500 bg-slate-100 px-3 py-1 rounded-lg" id="chart-month-tag">
                                Bulan Ini
                            </span>
                        </div>

                        <div class="grid grid-cols-2 sm:grid-cols-4 gap-3 mb-6">
                            <div class="p-3 bg-slate-50 rounded-2xl border border-slate-100">
                                <span class="text-xs text-slate-500 block font-semibold">Terkumpul</span>
                                <span class="text-base font-extrabold text-slate-800" id="stat-sampah-total">1.245 kg</span>
                            </div>
                            <div class="p-3 bg-slate-50 rounded-2xl border border-slate-100">
                                <span class="text-xs text-slate-500 block font-semibold">Didaur Ulang</span>
                                <span class="text-base font-extrabold text-eco-600" id="stat-sampah-daurulang">782 kg</span>
                            </div>
                            <div class="p-3 bg-slate-50 rounded-2xl border border-slate-100">
                                <span class="text-xs text-slate-500 block font-semibold">Warga Aktif</span>
                                <span class="text-base font-extrabold text-slate-800" id="stat-warga-total">328</span>
                            </div>
                            <div class="p-3 bg-slate-50 rounded-2xl border border-slate-100">
                                <span class="text-xs text-slate-500 block font-semibold">Selesai Ditangani</span>
                                <span class="text-base font-extrabold text-emerald-600" id="stat-laporan-selesai">94%</span>
                            </div>
                        </div>

                        <!-- Interactive Chart.js Canvas -->
                        <div class="h-56 w-full">
                            <canvas id="villageImpactChart"></canvas>
                        </div>
                    </div>
                </div>
            </section>
        </div>

        <!-- ADMIN PORTAL SECTION -->
        <section id="admin" class="scroll-mt-24 pt-8 border-t border-slate-200">
            <div class="bg-slate-900 text-white rounded-3xl p-6 sm:p-8 shadow-2xl space-y-6">
                
                <div class="flex flex-col md:flex-row md:items-center justify-between pb-6 border-b border-slate-800 gap-4">
                    <div>
                        <div class="inline-flex items-center space-x-2 text-xs font-bold text-amber-400 bg-amber-400/10 px-3 py-1 rounded-full mb-2">
                            <i class="fa-solid fa-user-gear"></i>
                            <span>Portal Kontrol SADEWA Admin Desa</span>
                        </div>
                        <h2 class="text-2xl sm:text-3xl font-extrabold">Dashboard Petugas Kebersihan Desa</h2>
                        <p class="text-slate-400 text-xs sm:text-sm mt-1">Kelola status tiket, permintaan jemput sampah, dan intervensi lokasi</p>
                    </div>

                    <div class="flex items-center space-x-2 bg-slate-800 p-2 rounded-2xl border border-slate-700">
                        <select id="admin-select-kecamatan" onchange="onAdminKecamatanChange()" class="bg-slate-900 border border-slate-700 text-xs text-white rounded-xl px-3 py-2 focus:outline-none">
                            <option value="">-- Pilih Kecamatan Admin --</option>
                        </select>
                        <select id="admin-select-desa" onchange="onAdminDesaChange()" class="bg-slate-900 border border-slate-700 text-xs text-white rounded-xl px-3 py-2 focus:outline-none">
                            <option value="">-- Pilih Desa Admin --</option>
                        </select>
                    </div>
                </div>

                <!-- Admin Metrics -->
                <div class="grid grid-cols-2 md:grid-cols-4 gap-4">
                    <div class="bg-slate-800/80 p-4 rounded-2xl border border-slate-700">
                        <span class="text-xs text-slate-400 uppercase font-bold block">Total Laporan</span>
                        <span class="text-2xl font-black text-white mt-1 block" id="admin-stat-total">18</span>
                    </div>
                    <div class="bg-slate-800/80 p-4 rounded-2xl border border-slate-700">
                        <span class="text-xs text-slate-400 uppercase font-bold block">Laporan Baru</span>
                        <span class="text-2xl font-black text-amber-400 mt-1 block" id="admin-stat-baru">4</span>
                    </div>
                    <div class="bg-slate-800/80 p-4 rounded-2xl border border-slate-700">
                        <span class="text-xs text-slate-400 uppercase font-bold block">Permintaan Jemput</span>
                        <span class="text-2xl font-black text-eco-400 mt-1 block" id="admin-stat-jemput">7</span>
                    </div>
                    <div class="bg-slate-800/80 p-4 rounded-2xl border border-slate-700">
                        <span class="text-xs text-slate-400 uppercase font-bold block">Tingkat Penanganan</span>
                        <span class="text-2xl font-black text-blue-400 mt-1 block" id="admin-stat-efisiensi">92%</span>
                    </div>
                </div>

                <!-- Admin Table Container -->
                <div class="space-y-4">
                    <h3 class="text-lg font-bold text-white flex items-center justify-between">
                        <span>Daftar Tiket & Masukan Masuk di <span id="admin-desa-title" class="text-eco-400">Desa Rawa Panjang</span></span>
                        <span class="text-xs font-normal text-slate-400">Pembaruan Real-time dari Form Warga</span>
                    </h3>

                    <div class="bg-slate-800 rounded-2xl border border-slate-700 overflow-hidden">
                        <div class="overflow-x-auto">
                            <table class="w-full text-left text-xs">
                                <thead class="bg-slate-900/80 text-slate-400 font-bold uppercase border-b border-slate-700">
                                    <tr>
                                        <th class="py-3 px-4">ID Tiket</th>
                                        <th class="py-3 px-4">Tipe</th>
                                        <th class="py-3 px-4">Detail / Lokasi</th>
                                        <th class="py-3 px-4">Waktu</th>
                                        <th class="py-3 px-4">Status Saat Ini</th>
                                        <th class="py-3 px-4">Aksi Admin</th>
                                    </tr>
                                </thead>
                                <tbody id="admin-table-body" class="divide-y divide-slate-700/60 text-slate-300">
                                    <!-- Dynamic Admin Rows -->
                                </tbody>
                            </table>
                        </div>
                    </div>
                </div>

            </div>
        </section>

    </main>

    <!-- Footer -->
    <footer class="bg-slate-900 text-slate-400 py-12 border-t border-slate-800 mt-auto">
        <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 grid grid-cols-1 md:grid-cols-3 gap-8 text-xs">
            <div>
                <div class="flex items-center space-x-2 text-white font-extrabold text-lg mb-3">
                    <i class="fa-solid fa-recycle text-eco-500"></i>
                    <span>SADEWA Bogor</span>
                </div>
                <p class="text-slate-400 leading-relaxed">
                    Sistem Aplikasi Daur-ulang & Eko-pelayanan Wilayah Antar-desa. Prototype tata kelola kebersihan berbasis keikutsertaan warga di Kabupaten Bogor, Jawa Barat.
                </p>
            </div>
            <div>
                <h4 class="font-bold text-white uppercase tracking-wider mb-3">Sistem Desa Terintegrasi</h4>
                <ul class="space-y-2">
                    <li><a href="#pilih-wilayah" class="hover:text-white transition">Status Layanan Desa</a></li>
                    <li><a href="#jadwal" class="hover:text-white transition">Jadwal Pengangkutan Sampah</a></li>
                    <li><a href="#fasilitas-peta" class="hover:text-white transition">Peta Bank Sampah & TPS3R</a></li>
                    <li><a href="#ecopoints" class="hover:text-white transition">Sistem Poin Warga</a></li>
                </ul>
            </div>
            <div>
                <h4 class="font-bold text-white uppercase tracking-wider mb-3">Catatan Pengembang</h4>
                <p class="leading-relaxed text-slate-400">
                    Aplikasi SADEWA disimulasikan menggunakan data dummy lokal (`localStorage`) untuk tujuan demonstrasi interaktif pelayanan masyarakat daerah.
                </p>
                <div class="mt-4 text-slate-500 text-[11px]">
                    &copy; 2026 SADEWA Bogor Prototype. Hak cipta dilindungi.
                </div>
            </div>
        </div>
    </footer>

    <!-- Toast Notification Container -->
    <div id="toast-container" class="fixed bottom-5 right-5 z-50 flex flex-col space-y-2 pointer-events-none"></div>

    <!-- Success Modal -->
    <div id="modal-success" class="fixed inset-0 z-50 bg-slate-900/60 backdrop-blur-sm flex items-center justify-center p-4 hidden">
        <div class="bg-white rounded-3xl max-w-md w-full p-6 text-center shadow-2xl space-y-4">
            <div class="w-16 h-16 bg-eco-100 text-eco-600 rounded-full flex items-center justify-center mx-auto text-3xl font-extrabold">
                ✓
            </div>
            <h3 class="text-2xl font-bold text-slate-900" id="modal-title">Permintaan Berhasil!</h3>
            <div class="bg-slate-50 p-4 rounded-2xl text-xs space-y-2 text-left border border-slate-100" id="modal-body">
                <!-- Dynamic Content -->
            </div>
            <button onclick="closeModal()" class="w-full bg-slate-900 text-white font-bold py-3 rounded-xl hover:bg-slate-800 transition text-sm">
                Tutup & Kembali
            </button>
        </div>
    </div>

    <!-- JavaScript Application Logic -->
    <script>
        // ==========================================
        // Data Structure: Regions (Kabupaten Bogor)
        // ==========================================
        const WILAYAH_BOGOR = {
            "Babakan Madang": ["Babakan Madang", "Bojong Koneng", "Cijayanti", "Cipambuan", "Citaringgul", "Kadumanggu", "Karang Tengah", "Sentul", "Sumur Batu"],
            "Bojong Gede": ["Bojong Baru", "Bojong Gede", "Cimanggis", "Kedung Waringin", "Ragajaya", "Rawa Panjang", "Susukan", "Waringin Jaya"],
            "Ciawi": ["Banjar Sari", "Banjar Wangi", "Banjarwaru", "Bendungan", "Bitungsari", "Bojong Murni", "Ciawi", "Cibedug", "Cileungsi", "Citapen", "Jambuluwuk", "Pandan Sari", "Teluk Pinang"],
            "Cibinong": ["Cirimekar", "Ciriung", "Harapan Jaya", "Karadenan", "Kandang Roda", "Nanggewer", "Nanggewer Mekar", "Pabuaran", "Pabuaran Mekar", "Pondok Rajeg", "Sukahati"],
            "Cisarua": ["Cibeureum", "Cilember", "Cisarua", "Citeko", "Jogjogan", "Kopo", "Leuwimalang", "Tugu Utara", "Tugu Selatan"],
            "Gunung Putri": ["Bojong Nangka", "Ciangsana", "Cikeas Udik", "Gunung Putri", "Karanggan", "Nagrak", "Tlanajaya", "Wanaherang"],
            "Parung": ["Ciseeng", "Iwul", "Jabon Mekar", "Pamagersari", "Parung", "Warujaya", "Pamegarsari"],
            "Dramaga": ["Babakan", "Ciherang", "Cikarwang", "Dramaga", "Neglasari", "Petir", "Purwasari", "Sukawening"],
            "Sukaraja": ["Cadas Ngampar", "Cibanon", "Cikeas", "Cilebut Barat", "Cilebut Timur", "Nagrak", "Sukaraja", "Sukatani"],
            "Jonggol": ["Balekambang", "Bendungan", "Cibitung Tengah", "Jonggol", "Sukamaju", "Sukagalih", "Sirnagalih"]
        };

        // Facility Marker Coordinates Simulation Map
        const FACILITY_MAP_DATA = {
            "Bojong Gede": { lat: -6.4912, lng: 106.7972 },
            "Babakan Madang": { lat: -6.5882, lng: 106.8732 },
            "Ciawi": { lat: -6.6582, lng: 106.8482 },
            "Cibinong": { lat: -6.4812, lng: 106.8532 },
            "Cisarua": { lat: -6.6882, lng: 106.9382 },
            "Gunung Putri": { lat: -6.4282, lng: 106.9082 },
            "Parung": { lat: -6.4212, lng: 106.7282 },
            "Dramaga": { lat: -6.5812, lng: 106.7382 },
            "Sukaraja": { lat: -6.5512, lng: 106.8282 },
            "Jonggol": { lat: -6.4682, lng: 107.0582 }
        };

        // State variables
        let selectedKecamatan = "Bojong Gede";
        let selectedDesa = "Rawa Panjang";
        let leafletMap = null;
        let mapMarkers = [];
        let impactChart = null;

        // Initialize application on load
        document.addEventListener('DOMContentLoaded', () => {
            initWilayahDropdowns();
            initStorageDefaults();
            updateVillageDashboard(selectedKecamatan, selectedDesa);
            initLeafletMap();
            initChart();
            renderPublicFeed();
            renderAdminTable();
        });

        // Toggle Mobile Navigation
        function toggleMobileMenu() {
            const menu = document.getElementById('mobile-menu');
            menu.classList.toggle('hidden');
        }

        function scrollToTop() {
            window.scrollTo({ top: 0, behavior: 'smooth' });
        }

        // Dropdown Initializer
        function initWilayahDropdowns() {
            const kecSelect = document.getElementById('select-kecamatan');
            const adminKecSelect = document.getElementById('admin-select-kecamatan');

            kecSelect.innerHTML = '<option value="">-- Pilih Kecamatan --</option>';
            adminKecSelect.innerHTML = '<option value="">-- Pilih Kecamatan Admin --</option>';

            Object.keys(WILAYAH_BOGOR).forEach(kec => {
                kecSelect.innerHTML += `<option value="${kec}">${kec}</option>`;
                adminKecSelect.innerHTML += `<option value="${kec}">${kec}</option>`;
            });

            // Set Defaults
            kecSelect.value = selectedKecamatan;
            adminKecSelect.value = selectedKecamatan;
            populateDesaDropdown(selectedKecamatan);
            populateAdminDesaDropdown(selectedKecamatan);

            document.getElementById('select-desa').value = selectedDesa;
            document.getElementById('admin-select-desa').value = selectedDesa;
        }

        function populateDesaDropdown(kec) {
            const desaSelect = document.getElementById('select-desa');
            desaSelect.innerHTML = '<option value="">-- Pilih Desa --</option>';

            if (kec && WILAYAH_BOGOR[kec]) {
                WILAYAH_BOGOR[kec].forEach(desa => {
                    desaSelect.innerHTML += `<option value="${desa}">${desa}</option>`;
                });
                desaSelect.disabled = false;
            } else {
                desaSelect.disabled = true;
            }
        }

        function populateAdminDesaDropdown(kec) {
            const adminDesaSelect = document.getElementById('admin-select-desa');
            adminDesaSelect.innerHTML = '<option value="">-- Pilih Desa Admin --</option>';

            if (kec && WILAYAH_BOGOR[kec]) {
                WILAYAH_BOGOR[kec].forEach(desa => {
                    adminDesaSelect.innerHTML += `<option value="${desa}">${desa}</option>`;
                });
                adminDesaSelect.disabled = false;
            } else {
                adminDesaSelect.disabled = true;
            }
        }

        // Change Event Handlers
        function onKecamatanChange() {
            const kec = document.getElementById('select-kecamatan').value;
            populateDesaDropdown(kec);
        }

        function onDesaChange() {
            selectedKecamatan = document.getElementById('select-kecamatan').value;
            selectedDesa = document.getElementById('select-desa').value;
            if (selectedDesa) {
                updateVillageDashboard(selectedKecamatan, selectedDesa);
                showToast(`Wilayah berhasil diubah ke ${selectedDesa}`);
            }
        }

        function onAdminKecamatanChange() {
            const kec = document.getElementById('admin-select-kecamatan').value;
            populateAdminDesaDropdown(kec);
        }

        function onAdminDesaChange() {
            const adminKec = document.getElementById('admin-select-kecamatan').value;
            const adminDesa = document.getElementById('admin-select-desa').value;
            if (adminDesa) {
                selectedKecamatan = adminKec;
                selectedDesa = adminDesa;
                document.getElementById('select-kecamatan').value = adminKec;
                populateDesaDropdown(adminKec);
                document.getElementById('select-desa').value = adminDesa;

                updateVillageDashboard(selectedKecamatan, selectedDesa);
                showToast(`Dashboard Admin beralih ke ${adminDesa}`);
            }
        }

        // Dashboard Data Generator & Updater
        function updateVillageDashboard(kec, desa) {
            // Update Text Displays
            document.getElementById('display-desa').innerText = `Desa ${desa}`;
            document.getElementById('display-kecamatan').innerText = `Kecamatan ${kec}, Kabupaten Bogor`;
            document.getElementById('jadwal-desa-tag').innerText = desa;
            document.getElementById('feed-desa-title').innerText = `Desa ${desa}`;
            document.getElementById('ecopoint-desa-label').innerText = `Desa ${desa}`;
            document.getElementById('admin-desa-title').innerText = `Desa ${desa}`;

            // Populate Form Inputs for Pickup
            document.getElementById('jemput-kecamatan').value = kec;
            document.getElementById('jemput-desa').value = desa;

            // Pseudo Random Dynamic Data based on Desa Name Length
            const seed = desa.length;
            const pendingReports = (seed % 5) + 1;
            const weeklyKg = 180 + (seed * 12);
            const activeCitizens = 80 + (seed * 15);
            const monthlyKg = weeklyKg * 4 + 120;
            const recycledKg = Math.round(monthlyKg * 0.65);

            document.getElementById('stat-laporan-pending').innerText = `${pendingReports} Laporan`;
            document.getElementById('stat-sampah-minggu').innerText = `${weeklyKg} kg`;
            document.getElementById('stat-warga-aktif').innerText = `${activeCitizens} Warga`;

            document.getElementById('stat-sampah-total').innerText = `${monthlyKg.toLocaleString('id-ID')} kg`;
            document.getElementById('stat-sampah-daurulang').innerText = `${recycledKg.toLocaleString('id-ID')} kg`;
            document.getElementById('stat-warga-total').innerText = (activeCitizens + 140).toString();
            document.getElementById('ecopoint-total').innerText = `${(activeCitizens * 10 + 450).toLocaleString('id-ID')} Poin`;

            // Update Schedule Table
            renderScheduleTable(desa);

            // Update Map Center
            updateMapCenter(kec);

            // Update Chart
            updateChartData(seed);

            // Update Feeds
            renderPublicFeed();
            renderAdminTable();
        }

        // Schedule Table Generator
        function renderScheduleTable(desa) {
            const tableBody = document.getElementById('table-jadwal-body');
            const schedules = [
                { hari: 'Senin', jenis: 'Organik (Sisa Makanan, Kebun)', jam: '07:00 - 11:00 WIB', armada: 'Motor Kaisar RT' },
                { hari: 'Selasa', jenis: 'Anorganik (Plastik, Kertas, Botol)', jam: '07:00 - 11:00 WIB', armada: 'Truk DLH Wilayah' },
                { hari: 'Rabu', jenis: 'Organik', jam: '07:00 - 11:00 WIB', armada: 'Motor Kaisar RT' },
                { hari: 'Kamis', jenis: 'Anorganik & Daur Ulang', jam: '07:00 - 11:00 WIB', armada: 'Truk DLH Wilayah' },
                { hari: 'Jumat', jenis: 'Residu Akhir / B3 RT', jam: '07:00 - 10:00 WIB', armada: 'Truk Sampah Desa' },
                { hari: 'Sabtu', jenis: 'Jemput Khusus / Bank Sampah', jam: '08:00 - 12:00 WIB', armada: 'Tim Relawan Desa' }
            ];

            const todayDays = ['Minggu', 'Senin', 'Selasa', 'Rabu', 'Kamis', 'Jumat', 'Sabtu'];
            const currentDayStr = todayDays[new Date().getDay()];

            let html = '';
            schedules.forEach(item => {
                const isToday = item.hari === currentDayStr;
                html += `
                    <tr class="${isToday ? 'bg-eco-50/70 font-bold' : 'hover:bg-slate-50'}">
                        <td class="py-3 px-6 flex items-center space-x-2">
                            ${isToday ? '<span class="w-2 h-2 rounded-full bg-eco-600"></span>' : ''}
                            <span>${item.hari}</span>
                        </td>
                        <td class="py-3 px-6 text-slate-800">${item.jenis}</td>
                        <td class="py-3 px-6">${item.jam}</td>
                        <td class="py-3 px-6 text-slate-500">${item.armada}</td>
                        <td class="py-3 px-6">
                            <span class="px-2.5 py-1 rounded-full text-[10px] font-bold ${isToday ? 'bg-eco-200 text-eco-800' : 'bg-slate-100 text-slate-600'}">
                                ${isToday ? 'Jadwal Hari Ini' : 'Terjadwal'}
                            </span>
                        </td>
                    </tr>
                `;
            });
            tableBody.innerHTML = html;
        }

        // Local Storage System Initialization
        function initStorageDefaults() {
            if (!localStorage.getItem('sadewa_reports')) {
                const defaultReports = [
                    {
                        id: '#SDW-0821',
                        kecamatan: 'Bojong Gede',
                        desa: 'Rawa Panjang',
                        lokasi: 'Jl. Raya Pabuaran Pertigaan RT 02/05',
                        kategori: 'Sampah Menumpuk',
                        deskripsi: 'Tumpukan kantong plastik sisa pasar di pinggir jalan utama.',
                        waktu: '22 Sep 2026, 08:30 WIB',
                        status: 'Sedang ditangani',
                        tipe: 'Laporan'
                    },
                    {
                        id: '#SDW-0819',
                        kecamatan: 'Bojong Gede',
                        desa: 'Rawa Panjang',
                        lokasi: 'RT 04 RW 01 Perumahan Cilebut',
                        kategori: 'Plastik Daur Ulang',
                        deskripsi: 'Permintaan jemput kardus bekas & botol mineral (12kg)',
                        waktu: '21 Sep 2026, 14:15 WIB',
                        status: 'Diverifikasi',
                        tipe: 'Jemput Sampah'
                    },
                    {
                        id: '#SDW-0805',
                        kecamatan: 'Bojong Gede',
                        desa: 'Rawa Panjang',
                        lokasi: 'Samping Jembatan Saluran Irigasi',
                        kategori: 'Sampah di Sungai',
                        deskripsi: 'Sumbatan ranting dan plastik limbah rumah tangga.',
                        waktu: '20 Sep 2026, 09:00 WIB',
                        status: 'Selesai',
                        tipe: 'Laporan'
                    }
                ];
                localStorage.setItem('sadewa_reports', JSON.stringify(defaultReports));
            }
        }

        function getStoredReports() {
            return JSON.parse(localStorage.getItem('sadewa_reports')) || [];
        }

        function saveStoredReports(reports) {
            localStorage.setItem('sadewa_reports', JSON.stringify(reports));
        }

        // Citizen Form Submissions
        function handleJemputSubmit(event) {
            event.preventDefault();
            const kec = document.getElementById('jemput-kecamatan').value;
            const desa = document.getElementById('jemput-desa').value;
            const alamat = document.getElementById('jemput-alamat').value;
            const jenis = document.getElementById('jemput-jenis').value;
            const berat = document.getElementById('jemput-berat').value;
            const tanggal = document.getElementById('jemput-tanggal').value;

            const ticketId = `SDW-2026-${Math.floor(10000 + Math.random() * 90000)}`;

            const newPickup = {
                id: `#${ticketId}`,
                kecamatan: kec,
                desa: desa,
                lokasi: alamat,
                kategori: `Penjemputan ${jenis} (${berat} kg)`,
                deskripsi: `Rencana Jemput: ${tanggal}. Jenis: ${jenis}, Perkiraan: ${berat} kg`,
                waktu: new Date().toLocaleString('id-ID', { dateStyle: 'medium', timeStyle: 'short' }),
                status: 'Diterima',
                tipe: 'Jemput Sampah'
            };

            const reports = getStoredReports();
            reports.unshift(newPickup);
            saveStoredReports(reports);

            // Open Modal
            document.getElementById('modal-title').innerText = "Permintaan Jemput SADEWA Berhasil!";
            document.getElementById('modal-body').innerHTML = `
                <div><span class="font-bold text-slate-500">Nomor Tiket:</span> <span class="font-bold text-eco-600">${newPickup.id}</span></div>
                <div><span class="font-bold text-slate-500">Wilayah:</span> ${desa}, Kecamatan ${kec}</div>
                <div><span class="font-bold text-slate-500">Alamat:</span> ${alamat}</div>
                <div><span class="font-bold text-slate-500">Status Awal:</span> <span class="px-2 py-0.5 rounded bg-amber-100 text-amber-800 font-bold">Menunggu Petugas</span></div>
            `;
            document.getElementById('modal-success').classList.remove('hidden');

            document.getElementById('form-jemput').reset();
            document.getElementById('jemput-kecamatan').value = kec;
            document.getElementById('jemput-desa').value = desa;

            renderPublicFeed();
            renderAdminTable();
        }

        function handleLaporSubmit(event) {
            event.preventDefault();
            const kategori = document.getElementById('lapor-kategori').value;
            const lokasi = document.getElementById('lapor-lokasi').value;
            const deskripsi = document.getElementById('lapor-deskripsi').value;

            const reportId = `SDW-${Math.floor(1000 + Math.random() * 9000)}`;

            const newReport = {
                id: `#${reportId}`,
                kecamatan: selectedKecamatan,
                desa: selectedDesa,
                lokasi: lokasi,
                kategori: kategori,
                deskripsi: deskripsi,
                waktu: new Date().toLocaleString('id-ID', { dateStyle: 'medium', timeStyle: 'short' }),
                status: 'Diterima',
                tipe: 'Laporan'
            };

            const reports = getStoredReports();
            reports.unshift(newReport);
            saveStoredReports(reports);

            // Open Modal
            document.getElementById('modal-title').innerText = "Laporan SADEWA Diterima!";
            document.getElementById('modal-body').innerHTML = `
                <div><span class="font-bold text-slate-500">ID Laporan:</span> <span class="font-bold text-eco-600">${newReport.id}</span></div>
                <div><span class="font-bold text-slate-500">Lokasi:</span> ${lokasi} (${selectedDesa})</div>
                <div><span class="font-bold text-slate-500">Kategori:</span> ${kategori}</div>
                <div><span class="font-bold text-slate-500">Status:</span> <span class="px-2 py-0.5 rounded bg-amber-100 text-amber-800 font-bold">Diterima Admin Desa</span></div>
            `;
            document.getElementById('modal-success').classList.remove('hidden');

            document.getElementById('form-lapor').reset();

            renderPublicFeed();
            renderAdminTable();
        }

        function closeModal() {
            document.getElementById('modal-success').classList.add('hidden');
        }

        // Render Public Citizen Reports Card Feed
        function renderPublicFeed() {
            const container = document.getElementById('public-reports-feed');
            const reports = getStoredReports().filter(r => r.desa === selectedDesa);

            if (reports.length === 0) {
                container.innerHTML = `
                    <div class="col-span-full py-8 text-center text-slate-400 text-xs">
                        Belum ada laporan aktif SADEWA untuk Desa ${selectedDesa}.
                    </div>
                `;
                return;
            }

            let html = '';
            reports.forEach(item => {
                let badgeColor = 'bg-amber-100 text-amber-800';
                if (item.status === 'Sedang ditangani' || item.status === 'Ditangani') badgeColor = 'bg-blue-100 text-blue-800';
                if (item.status === 'Selesai') badgeColor = 'bg-emerald-100 text-emerald-800';

                html += `
                    <div class="bg-slate-50 p-4 rounded-2xl border border-slate-200 flex flex-col justify-between space-y-3">
                        <div>
                            <div class="flex items-center justify-between text-xs mb-2">
                                <span class="font-extrabold text-slate-800">${item.id}</span>
                                <span class="px-2 py-0.5 rounded-full font-bold text-[10px] ${badgeColor}">${item.status}</span>
                            </div>
                            <span class="text-[10px] font-bold text-eco-700 uppercase tracking-wide block mb-1">📍 Desa ${item.desa}</span>
                            <h4 class="font-bold text-slate-900 text-sm">${item.kategori}</h4>
                            <p class="text-xs text-slate-600 mt-1 line-clamp-2">${item.deskripsi}</p>
                            <p class="text-[11px] text-slate-400 mt-2"><i class="fa-solid fa-location-dot mr-1"></i> ${item.lokasi}</p>
                        </div>
                        <div class="pt-2 border-t border-slate-200/60 flex justify-between items-center text-[10px] text-slate-400">
                            <span><i class="fa-regular fa-clock mr-1"></i> ${item.waktu}</span>
                            <span class="font-bold text-slate-500">${item.tipe}</span>
                        </div>
                    </div>
                `;
            });
            container.innerHTML = html;
        }

        // Render Admin Table Dynamic Controls
        function renderAdminTable() {
            const tableBody = document.getElementById('admin-table-body');
            const reports = getStoredReports().filter(r => r.desa === selectedDesa);

            document.getElementById('admin-stat-total').innerText = reports.length.toString();
            document.getElementById('admin-stat-baru').innerText = reports.filter(r => r.status === 'Diterima').length.toString();
            document.getElementById('admin-stat-jemput').innerText = reports.filter(r => r.tipe === 'Jemput Sampah').length.toString();

            if (reports.length === 0) {
                tableBody.innerHTML = `
                    <tr>
                        <td colspan="6" class="py-6 text-center text-slate-500">Tidak ada data tiket SADEWA untuk Desa ${selectedDesa}.</td>
                    </tr>
                `;
                return;
            }

            let html = '';
            reports.forEach((item, index) => {
                html += `
                    <tr class="hover:bg-slate-700/40 transition">
                        <td class="py-3 px-4 font-bold text-eco-400">${item.id}</td>
                        <td class="py-3 px-4">
                            <span class="px-2 py-0.5 rounded text-[10px] font-bold ${item.tipe === 'Jemput Sampah' ? 'bg-purple-900/60 text-purple-200' : 'bg-blue-900/60 text-blue-200'}">
                                ${item.tipe}
                            </span>
                        </td>
                        <td class="py-3 px-4">
                            <div class="font-semibold text-white">${item.kategori}</div>
                            <div class="text-[10px] text-slate-400 truncate max-w-xs">${item.lokasi} - ${item.deskripsi}</div>
                        </td>
                        <td class="py-3 px-4 text-slate-400 text-[10px]">${item.waktu}</td>
                        <td class="py-3 px-4">
                            <span class="px-2 py-0.5 rounded-full text-[10px] font-bold ${getAdminStatusBadgeClass(item.status)}">
                                ${item.status}
                            </span>
                        </td>
                        <td class="py-3 px-4">
                            <select onchange="updateReportStatus('${item.id}', this.value)" class="bg-slate-900 border border-slate-600 text-white text-[11px] rounded-lg px-2 py-1 focus:outline-none">
                                <option value="Diterima" ${item.status === 'Diterima' ? 'selected' : ''}>Diterima</option>
                                <option value="Diverifikasi" ${item.status === 'Diverifikasi' ? 'selected' : ''}>Diverifikasi</option>
                                <option value="Ditangani" ${item.status === 'Ditangani' || item.status === 'Sedang ditangani' ? 'selected' : ''}>Ditangani</option>
                                <option value="Selesai" ${item.status === 'Selesai' ? 'selected' : ''}>Selesai</option>
                            </select>
                        </td>
                    </tr>
                `;
            });
            tableBody.innerHTML = html;
        }

        function getAdminStatusBadgeClass(status) {
            if (status === 'Diterima') return 'bg-amber-900/60 text-amber-300';
            if (status === 'Diverifikasi') return 'bg-purple-900/60 text-purple-300';
            if (status === 'Ditangani' || status === 'Sedang ditangani') return 'bg-blue-900/60 text-blue-300';
            if (status === 'Selesai') return 'bg-emerald-900/60 text-emerald-300';
            return 'bg-slate-700 text-slate-300';
        }

        function updateReportStatus(reportId, newStatus) {
            const reports = getStoredReports();
            const idx = reports.findIndex(r => r.id === reportId);
            if (idx !== -1) {
                reports[idx].status = newStatus;
                saveStoredReports(reports);
                renderPublicFeed();
                renderAdminTable();
                showToast(`Status tiket ${reportId} diperbarui ke ${newStatus}`);
            }
        }

        // Leaflet Interactive Map Logic
        function initLeafletMap() {
            const initialCoords = FACILITY_MAP_DATA["Bojong Gede"];
            leafletMap = L.map('map').setView([initialCoords.lat, initialCoords.lng], 13);

            L.tileLayer('https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png', {
                maxZoom: 18,
                attribution: '&copy; OpenStreetMap contributors | SADEWA Bogor'
            }).addTo(leafletMap);

            renderMapMarkers("Semua");
        }

        function updateMapCenter(kec) {
            if (!leafletMap) return;
            const coords = FACILITY_MAP_DATA[kec] || FACILITY_MAP_DATA["Bojong Gede"];
            leafletMap.setView([coords.lat, coords.lng], 13);
            renderMapMarkers("Semua");
        }

        function renderMapMarkers(category) {
            if (!leafletMap) return;

            // Clear existing markers
            mapMarkers.forEach(m => leafletMap.removeLayer(m));
            mapMarkers = [];

            const coords = FACILITY_MAP_DATA[selectedKecamatan] || FACILITY_MAP_DATA["Bojong Gede"];

            const demoFacilities = [
                { name: `Bank Sampah Berkah SADEWA ${selectedDesa}`, type: 'Bank Sampah', lat: coords.lat + 0.005, lng: coords.lng + 0.004, desc: 'Menerima plastik, kertas, dan jelantah' },
                { name: `TPS3R Mandiri ${selectedDesa}`, type: 'TPS', lat: coords.lat - 0.004, lng: coords.lng - 0.003, desc: 'Fasilitas pemilahan organik & komposting' },
                { name: `Pusat Daur Ulang Kreatif`, type: 'Daur Ulang', lat: coords.lat + 0.002, lng: coords.lng - 0.006, desc: 'Pengolahan sampah plastik menjadi kerajinan' },
                { name: `Pos Pelayanan SADEWA RT 03`, type: 'TPS', lat: coords.lat - 0.006, lng: coords.lng + 0.005, desc: 'Titik penimbangan & edukasi warga' }
            ];

            demoFacilities.forEach(fac => {
                if (category === 'Semua' || fac.type === category) {
                    const marker = L.marker([fac.lat, fac.lng]).addTo(leafletMap);
                    marker.bindPopup(`
                        <div class="p-1">
                            <span class="text-[10px] font-bold uppercase text-eco-600 block">${fac.type}</span>
                            <strong class="text-sm block text-slate-800">${fac.name}</strong>
                            <p class="text-xs text-slate-600 mt-1">${fac.desc}</p>
                            <span class="text-[10px] text-slate-400 mt-2 block">📍 Wilayah ${selectedDesa}</span>
                        </div>
                    `);
                    mapMarkers.push(marker);
                }
            });
        }

        function filterMap(category) {
            // Update button styles
            const buttons = document.querySelectorAll('.btn-filter');
            buttons.forEach(btn => {
                if (btn.innerText.includes(category) || (category === 'Semua' && btn.innerText === 'Semua')) {
                    btn.className = 'btn-filter px-4 py-2 rounded-xl text-xs font-bold bg-slate-900 text-white transition';
                } else {
                    btn.className = 'btn-filter px-4 py-2 rounded-xl text-xs font-bold bg-slate-100 text-slate-600 hover:bg-slate-200 transition';
                }
            });

            renderMapMarkers(category);
        }

        // Chart.js Dynamics
        function initChart() {
            const ctx = document.getElementById('villageImpactChart').getContext('2d');
            impactChart = new Chart(ctx, {
                type: 'bar',
                data: {
                    labels: ['Minggu 1', 'Minggu 2', 'Minggu 3', 'Minggu 4'],
                    datasets: [
                        {
                            label: 'Sampah Terkumpul (kg)',
                            data: [210, 340, 290, 405],
                            backgroundColor: '#22c55e',
                            borderRadius: 8
                        },
                        {
                            label: 'Didaur Ulang (kg)',
                            data: [130, 220, 180, 252],
                            backgroundColor: '#1e293b',
                            borderRadius: 8
                        }
                    ]
                },
                options: {
                    responsive: true,
                    maintainAspectRatio: false,
                    plugins: {
                        legend: { position: 'bottom', labels: { font: { family: 'Plus Jakarta Sans', size: 11 } } }
                    },
                    scales: {
                        y: { grid: { color: '#f1f5f9' }, ticks: { font: { family: 'Plus Jakarta Sans', size: 10 } } },
                        x: { grid: { display: false }, ticks: { font: { family: 'Plus Jakarta Sans', size: 10 } } }
                    }
                }
            });
        }

        function updateChartData(seed) {
            if (!impactChart) return;
            const w1 = 180 + seed * 5;
            const w2 = 220 + seed * 8;
            const w3 = 200 + seed * 6;
            const w4 = 250 + seed * 10;

            impactChart.data.datasets[0].data = [w1, w2, w3, w4];
            impactChart.data.datasets[1].data = [Math.round(w1 * 0.6), Math.round(w2 * 0.65), Math.round(w3 * 0.6), Math.round(w4 * 0.7)];
            impactChart.update();
        }

        // Notification Toast System
        function showToast(message) {
            const container = document.getElementById('toast-container');
            const toast = document.createElement('div');
            toast.className = 'bg-slate-900 text-white text-xs px-4 py-3 rounded-2xl shadow-xl flex items-center space-x-2 border border-slate-700 animate-bounce pointer-events-auto';
            toast.innerHTML = `
                <i class="fa-solid fa-circle-check text-eco-400"></i>
                <span>${message}</span>
            `;
            container.appendChild(toast);

            setTimeout(() => {
                toast.remove();
            }, 3000);
        }
    </script>
</body>
</html>
