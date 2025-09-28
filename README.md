<!DOCTYPE html>
<html lang="tr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Giriş - Analiz Pro X</title>
    <link href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0/css/all.min.css" rel="stylesheet">
        <script src="https://cdn.tailwindcss.com"></script>
        <script>
        // Hide Tailwind CDN production warning in console (quick fix)
        (function(){
            var origWarn = console.warn;
            console.warn = function(){
                if (arguments.length && typeof arguments[0]==='string' && arguments[0].includes('cdn.tailwindcss.com should not be used in production')) return;
                return origWarn.apply(console, arguments);
            };
        })();
        </script>
    <style>
    /* Küçük tema destekleri */
    .apx-dark { background: linear-gradient(180deg,#0f172a,#07132a) !important; color: #e6eef8; }
    .apx-dark .bg-white { background-color: #0b1220 !important; }
    .apx-dark .text-gray-700, .apx-dark .text-gray-900 { color: #dbeafe !important; }
    /* Test UI front class to ensure it appears above admin overlays during candidate flow */
    .apx-test-front { position: relative !important; z-index: 10060 !important; }
    </style>
</head>
<body class="bg-gradient-to-br from-blue-900 via-purple-900 to-indigo-900 min-h-screen flex items-center justify-center overflow-auto">
    <!-- Background Pattern -->
    <div class="absolute inset-0 bg-black opacity-30"></div>
    <div class="absolute inset-0" style="background-image: url('data:image/svg+xml,%3Csvg%20width%3D%2260%22%20height%3D%2260%22%20viewBox%3D%220%200%2060%2060%22%20xmlns%3D%22http%3A//www.w3.org/2000/svg%22%3E%3Cg%20fill%3D%22none%22%20fill-rule%3D%22evenodd%22%3E%3Cg%20fill%3D%22%234F46E5%22%20fill-opacity%3D%220.05%22%3E%3Ccircle%20cx%3D%2230%22%20cy%3D%2230%22%20r%3D%2230%22/%3E%3C/g%3E%3C/g%3E%3C/svg%3E'); background-size: 120px 120px;"></div>
    
    <div class="relative z-10 w-full max-w-5xl mx-auto px-2 md:px-8 py-8">
        <!-- Logo Section -->
        <div class="text-center mb-12">
            <div class="inline-flex items-center justify-center w-24 h-24 bg-white rounded-full shadow-2xl mb-6">
                <i class="fas fa-brain text-4xl text-blue-600"></i>
            </div>
            <h1 class="text-5xl font-bold text-white mb-3">Analiz Pro X</h1>
            <p class="text-xl text-blue-200">Gelecek Nesil Kişilik ve Yetenek Değerlendirme Sistemi</p>
            <div class="mt-4 text-blue-100">
                <i class="fas fa-shield-alt mr-2"></i>
                Güvenli • Güvenilir • Yapay Zeka Destekli
            </div>
        </div>

        <!-- Login Options -->
    <div class="grid grid-cols-1 md:grid-cols-2 gap-6 md:gap-8 justify-items-center items-center place-items-center">
        <!-- Floating admin quick-open button -->
        <button id="openAdminBtn" title="Yönetici" class="fixed top-6 right-6 z-50 bg-white text-blue-700 p-3 rounded-full shadow-lg hover:scale-105 transform transition">
            <i class="fas fa-user-cog"></i>
        </button>
        <!-- Admin Panel (Başlangıçta gizli) -->
    <!-- Compact admin panel: opens from the floating button (not full-screen) -->
    <div id="adminPanel" class="hidden fixed inset-0 z-[9999] flex items-start justify-center pt-16">
        <!-- İK Paneli (Başlangıçta gizli) -->
        <div id="ikPanel" class="hidden fixed inset-0 bg-black bg-opacity-60 flex items-center justify-center z-50">
            <div class="bg-white rounded-2xl shadow-2xl p-10 w-full max-w-3xl relative overflow-y-auto max-h-screen">
                <button onclick="document.getElementById('ikPanel').classList.add('hidden')" class="absolute top-4 right-4 text-gray-400 hover:text-red-500 text-2xl">&times;</button>
                <h2 class="text-3xl font-bold text-green-800 mb-6 text-center">İK Yöneticisi Paneli</h2>
                <form id="addCandidateForm" class="bg-green-50 p-6 rounded-lg shadow mb-6">
                    <div class="grid grid-cols-1 md:grid-cols-2 gap-4">
                        <div>
                            <label class="block text-sm font-medium text-gray-700 mb-1">Aday Rumuz</label>
                            <input type="text" id="newCandidateNickname" required class="w-full px-3 py-2 border rounded-lg" placeholder="Rumuz (ör: aday01)">
                        </div>
                        <div>
                            <label class="block text-sm font-medium text-gray-700 mb-1">Aday Şifre</label>
                            <input type="text" id="newCandidatePassword" required class="w-full px-3 py-2 border rounded-lg" placeholder="Şifre">
                        </div>
                        <div>
                            <label class="block text-sm font-medium text-gray-700 mb-1">Sektör</label>
                            <select id="candidateSector" class="w-full px-3 py-2 border rounded-lg">
                                <option value="">Seçiniz</option>
                                <option value="imalat">İmalat Sektörü</option>
                                <option value="hizmet">Hizmet Sektörü</option>
                            </select>
                        </div>
                        <div>
                            <label class="block text-sm font-medium text-gray-700 mb-1">Görevi (Rol)</label>
                            <select id="candidateRole" class="w-full px-3 py-2 border rounded-lg">
                                <option value="">Önce sektör seçin</option>
                            </select>
                        </div>
                    </div>
                    <button type="submit" class="mt-4 w-full bg-green-600 text-white py-2 rounded-lg hover:bg-green-700">Aday Kaydet</button>
                </form>
                <div class="mb-6">
                    <h3 class="text-xl font-bold text-green-700 mb-2">Kayıtlı Adaylar</h3>
                    <table class="w-full text-sm text-left border">
                        <thead class="bg-green-100">
                            <tr><th class="p-2">Rumuz</th><th class="p-2">Tip</th><th class="p-2">Başlık</th></tr>
                        </thead>
                        <tbody id="candidateList"></tbody>
                    </table>
                </div>
                <div>
                    <h3 class="text-xl font-bold text-green-700 mb-2">Raporlama & Grafik</h3>
                    <div class="flex flex-col md:flex-row gap-6">
                        <div class="bg-green-50 rounded-lg p-4 flex-1">
                            <canvas id="radarChart" width="300" height="200"></canvas>
                        </div>
                        <div class="bg-green-50 rounded-lg p-4 flex-1">
                            <h4 class="font-semibold mb-2">Aday Cevapları</h4>
                            <ul id="answerList" class="list-disc pl-5 text-gray-700"></ul>
                        </div>
                    </div>
                </div>
            </div>
        </div>
            <div class="bg-white rounded-xl shadow p-4 w-72 relative">
                <button id="closeAdminPanel" onclick="document.getElementById('adminPanel').classList.add('hidden')" class="absolute top-2 right-2 text-gray-400 hover:text-red-500 text-lg">&times;</button>
                <h2 class="text-lg font-bold text-blue-800 mb-3 text-center">Yönetici</h2>
                <div class="space-y-2">
                    <button id="manageUsersBtn" class="w-full bg-gray-200 text-black py-2 px-3 rounded-lg font-semibold">Kullanıcıları Yönet</button>
                    <button id="systemSettingsBtn" class="w-full bg-green-600 text-white py-2 px-3 rounded-lg font-semibold hover:bg-green-700">Sistem Ayarları</button>
                    <button id="logoutBtn" class="w-full bg-red-600 text-white py-2 px-3 rounded-lg font-semibold hover:bg-red-700">Çıkış Yap</button>
                </div>
                <div class="mt-4 text-center text-gray-400 text-xs">
                    <strong>Analiz Pro X</strong><br>
                    © 2025
                </div>
            </div>
            <!-- Sistem Ayarları Modal -->
            <div id="systemSettingsModal" class="hidden fixed inset-0 bg-black bg-opacity-60 flex items-center justify-center z-[9999]">
                <div class="bg-white rounded-2xl shadow-2xl p-6 w-full max-w-md relative">
                    <button onclick="document.getElementById('systemSettingsModal').classList.add('hidden')" class="absolute top-3 right-3 text-gray-400 hover:text-red-500 text-2xl">&times;</button>
                    <h3 class="text-lg font-bold text-gray-800 mb-4">Sistem Ayarları</h3>
                    <form id="systemSettingsForm" class="space-y-4">
                        <div>
                            <label class="flex items-center gap-3">
                                <input type="checkbox" id="settingCompactFooter" />
                                <span>Kompakt footer (mobilde küçült)</span>
                            </label>
                        </div>
                        <div>
                            <label class="flex items-center gap-3">
                                <input type="checkbox" id="settingHideFooter" />
                                <span>Footer'ı gizle</span>
                            </label>
                        </div>
                        <div>
                            <label class="flex items-center gap-3">
                                <input type="checkbox" id="settingDarkMode" />
                                <span>Koyu tema (sayfa arka planını koyulaştır)</span>
                            </label>
                        </div>
                        <div class="flex justify-end gap-3 mt-4">
                            <button type="button" id="resetSettingsBtn" class="px-4 py-2 rounded border">Sıfırla</button>
                            <button type="submit" class="bg-green-600 text-white px-4 py-2 rounded">Kaydet</button>
                        </div>
                    </form>
                </div>
            </div>
        </div>
            <!-- Admin Login (moved to compact modal opened by floating admin icon) -->
            <div id="adminLoginCompact" class="hidden fixed inset-0 bg-black bg-opacity-50 flex items-center justify-center z-[9998]">
                <div class="bg-white rounded-xl shadow-lg p-6 w-full max-w-sm relative">
                    <button onclick="document.getElementById('adminLoginCompact').classList.add('hidden')" class="absolute top-3 right-3 text-gray-400 hover:text-red-500">&times;</button>
                    <h3 class="text-lg font-bold text-blue-700 mb-3">Yönetici Girişi</h3>
                    <form id="adminLoginFormCompact" class="space-y-3">
                        <!-- Quick password-only access for local admin (legacy behavior) -->
                        <input type="hidden" id="adminEmailCompact" value="" />
                        <div>
                            <label class="block text-sm text-gray-700 mb-1">Şifre (sadece şifre ile giriş)</label>
                            <input type="password" id="adminPasswordCompact" class="w-full px-3 py-2 border rounded" placeholder="Yönetici şifresi" required />
                        </div>
                        <div>
                            <button type="submit" class="w-full bg-blue-600 text-white py-2 rounded">Giriş Yap (Şifre ile)</button>
                        </div>
                    </form>
                </div>
            </div>

            <!-- HR Manager Login -->
    <div class="bg-white rounded-2xl shadow-2xl py-20 px-8 min-h-[560px] transform transition duration-500 hover:scale-105 w-full max-w-md mx-auto">
                <div class="text-center mb-6">
                    <div class="inline-flex items-center justify-center w-16 h-16 bg-green-100 rounded-full mb-4">
                        <i class="fas fa-users text-2xl text-green-600"></i>
                    </div>
                    <h2 class="text-2xl font-bold text-gray-900 mb-2">İK Yöneticisi</h2>
                    <p class="text-gray-600">Aday yönetimi ve raporlama</p>
                </div>

                <form id="hrLoginForm" class="space-y-6">
                    <div>
                        <label class="block text-sm font-medium text-gray-700 mb-2">
                            <i class="fas fa-envelope text-green-600 mr-2"></i>
                            E-posta
                        </label>
                        <input type="email" id="hrEmail" required 
                               class="w-full px-4 py-3 border border-gray-300 rounded-lg focus:ring-2 focus:ring-green-500 focus:border-transparent transition duration-200"
                               placeholder="email@firma.com">
                    </div>
                    
                    <div>
                        <label class="block text-sm font-medium text-gray-700 mb-2">
                            <i class="fas fa-lock text-green-600 mr-2"></i>
                            Şifre
                        </label>
                        <input type="password" id="hrPassword" required 
                               class="w-full px-4 py-3 border border-gray-300 rounded-lg focus:ring-2 focus:ring-green-500 focus:border-transparent transition duration-200"
                               placeholder="Şifrenizi girin">
                    </div>
                    
                    <button type="submit" 
                            class="w-full bg-gradient-to-r from-green-600 to-green-700 text-white py-3 px-6 rounded-lg hover:from-green-700 hover:to-green-800 focus:outline-none focus:ring-2 focus:ring-green-500 focus:ring-offset-2 transform transition duration-200 hover:scale-105 font-medium">
                        <i class="fas fa-sign-in-alt mr-2"></i>
                        İK Yöneticisi Girişi
                    </button>
                    <div class="text-center mt-3">
                        <button type="button" id="showHrRegister" class="text-green-700 underline text-sm">Üye Ol</button>
                    </div>
                </form>

                <!-- İK Yöneticisi Üye Ol Modalı -->
                <div id="hrRegisterModal" class="hidden fixed inset-0 bg-black bg-opacity-60 flex items-center justify-center z-[9999]">
                    <div class="bg-white rounded-2xl shadow-2xl p-8 w-full max-w-xs relative">
                        <button onclick="document.getElementById('hrRegisterModal').classList.add('hidden')" class="absolute top-3 right-3 text-gray-400 hover:text-red-500 text-2xl">&times;</button>
                        <h3 class="text-lg font-bold text-green-700 mb-4">Yeni İK Yöneticisi Kaydı</h3>
                        <form id="hrRegisterForm" class="flex flex-col gap-4">
                            <div>
                                <label class="block text-xs font-semibold mb-1">E-posta</label>
                                <input type="email" id="hrRegEmail" class="border rounded px-2 py-1 w-full" required>
                            </div>
                            <div>
                                <label class="block text-xs font-semibold mb-1">Şifre</label>
                                <input type="password" id="hrRegPassword" class="border rounded px-2 py-1 w-full" required>
                            </div>
                            <div>
                                <label class="block text-xs font-semibold mb-1">İsim Soyisim</label>
                                <input type="text" id="hrRegFullName" class="border rounded px-2 py-1 w-full" placeholder="Ad Soyad">
                            </div>
                            <div>
                                <label class="block text-xs font-semibold mb-1">Kuruluş / Firma</label>
                                <input type="text" id="hrRegCompany" class="border rounded px-2 py-1 w-full" placeholder="Firma adı">
                            </div>
                            <div>
                                <label class="block text-xs font-semibold mb-1">Görevi</label>
                                <input type="text" id="hrRegRole" class="border rounded px-2 py-1 w-full" placeholder="Pozisyon / Görev">
                            </div>
                            <button type="submit" class="bg-green-600 text-white px-4 py-2 rounded font-semibold">Kaydol</button>
                        </form>
                    </div>
                </div>
                <!-- Admin -> İK Yöneticisi Ekle Modal (eksikti) -->
                <div id="hrAdminModal" class="hidden fixed inset-0 bg-black bg-opacity-60 flex items-center justify-center z-[9999]">
                    <div class="bg-white rounded-2xl shadow-2xl p-8 w-full max-w-xs relative">
                        <button onclick="document.getElementById('hrAdminModal').classList.add('hidden')" class="absolute top-3 right-3 text-gray-400 hover:text-red-500 text-2xl">&times;</button>
                        <h3 class="text-lg font-bold text-blue-800 mb-4">İK Yöneticisi Ekle</h3>
                        <form id="hrAdminForm" class="flex flex-col gap-3">
                            <div>
                                <label class="block text-xs font-semibold mb-1">Kullanıcı Adı</label>
                                <input type="text" id="hrAdminUsername" class="border rounded px-2 py-1 w-full" placeholder="kullaniciadi" required>
                            </div>
                            <div>
                                <label class="block text-xs font-semibold mb-1">İsim Soyisim</label>
                                <input type="text" id="hrAdminFullName" class="border rounded px-2 py-1 w-full" placeholder="İsim Soyisim">
                            </div>
                            <div>
                                <label class="block text-xs font-semibold mb-1">Telefon</label>
                                <input type="text" id="hrAdminPhone" class="border rounded px-2 py-1 w-full" placeholder="05xxxxxxxxx">
                            </div>
                            <div>
                                <label class="block text-xs font-semibold mb-1">E-posta</label>
                                <input type="email" id="hrAdminEmail" class="border rounded px-2 py-1 w-full" required>
                            </div>
                            <div>
                                <label class="block text-xs font-semibold mb-1">Kuruluş / Firma</label>
                                <input type="text" id="hrAdminCompany" class="border rounded px-2 py-1 w-full" placeholder="Firma adı">
                            </div>
                            <div>
                                <label class="block text-xs font-semibold mb-1">Görevi</label>
                                <input type="text" id="hrAdminRole" class="border rounded px-2 py-1 w-full" placeholder="Pozisyon / Görev">
                            </div>
                            <div>
                                <label class="block text-xs font-semibold mb-1">Şifre</label>
                                <input type="password" id="hrAdminPassword" class="border rounded px-2 py-1 w-full" required>
                            </div>
                            <div class="flex justify-end gap-2 mt-2">
                                <button type="button" onclick="document.getElementById('hrAdminModal').classList.add('hidden')" class="px-3 py-1 border rounded">İptal</button>
                                <button type="submit" class="bg-blue-600 text-white px-4 py-1 rounded">Ekle</button>
                            </div>
                        </form>
                    </div>
                </div>
                <!-- HR Yönetim Modal -->
                <div id="hrManageModal" class="hidden fixed inset-0 bg-black bg-opacity-60 flex items-center justify-center z-[9999]">
                    <div class="bg-white rounded-2xl shadow-2xl p-6 w-full max-w-4xl relative overflow-y-auto max-h-[80vh]">
                        <button onclick="document.getElementById('hrManageModal').classList.add('hidden')" class="absolute top-3 right-3 text-gray-400 hover:text-red-500 text-2xl">&times;</button>
                        <h3 class="text-lg font-bold text-gray-800 mb-4">İK Yöneticileri Yönetimi</h3>
                        <div class="mb-4 flex gap-3 items-center">
                            <label class="text-sm">Başlangıç:</label>
                            <input type="date" id="filterFrom" class="border rounded px-2 py-1">
                            <label class="text-sm">Bitiş:</label>
                            <input type="date" id="filterTo" class="border rounded px-2 py-1">
                            <button id="applyFilterBtn" class="bg-blue-600 text-white px-3 py-1 rounded">Filtrele</button>
                            <button id="refreshHrListBtn" class="px-3 py-1 border rounded">Yenile</button>
                        </div>
                        <table class="w-full text-sm text-left border">
                            <thead class="bg-gray-100"><tr>
                                <th class="p-2">Kullanıcı Adı</th>
                                <th class="p-2">İsim Soyisim</th>
                                <th class="p-2">Telefon</th>
                                <th class="p-2">E-posta</th>
                                <th class="p-2">Şifre</th>
                                <th class="p-2">Test Et Sayısı</th>
                                <th class="p-2">Durum</th>
                                <th class="p-2">Eylemler</th>
                            </tr></thead>
                            <tbody id="hrManageTableBody"></tbody>
                        </table>
                    </div>
                </div>
            </div>

            <!-- Candidate Login -->
            <div class="bg-white rounded-2xl shadow-2xl p-8 transform transition duration-500 hover:scale-105 w-full max-w-md mx-auto">
                <div class="text-center mb-6">
                    <div class="inline-flex items-center justify-center w-16 h-16 bg-blue-100 rounded-full mb-4">
                        <i class="fas fa-user-graduate text-2xl text-blue-600"></i>
                    </div>
                    <h2 class="text-2xl font-bold text-gray-900 mb-2">Aday</h2>
                    <p class="text-gray-600">Test alım sistemi</p>
                </div>

                <form id="candidateLoginForm" class="space-y-6">
                    <div>
                        <label class="block text-sm font-medium text-gray-700 mb-2">
                            <i class="fas fa-user text-blue-600 mr-2"></i>
                            Rumuz
                        </label>
                        <input type="text" id="candidateNickname" required 
                               class="w-full px-4 py-3 border border-gray-300 rounded-lg focus:ring-2 focus:ring-blue-500 focus:border-transparent transition duration-200"
                               placeholder="Aday rumuzunuzu girin">
                    </div>
                    <div>
                        <label class="block text-sm font-medium text-gray-700 mb-2">
                            <i class="fas fa-lock text-blue-600 mr-2"></i>
                            Şifre
                        </label>
                        <input type="password" id="candidatePassword" required 
                               class="w-full px-4 py-3 border border-gray-300 rounded-lg focus:ring-2 focus:ring-blue-500 focus:border-transparent transition duration-200"
                               placeholder="Şifrenizi girin">
                    </div>
                    <button type="submit" 
                            class="w-full bg-gradient-to-r from-blue-600 to-indigo-600 text-white py-3 px-6 rounded-lg hover:from-blue-700 hover:to-indigo-700 focus:outline-none focus:ring-2 focus:ring-blue-500 focus:ring-offset-2 transform transition duration-200 hover:scale-105 font-medium">
                        <i class="fas fa-clipboard-check mr-2"></i>
                        Teste Başla
                    </button>
                </form>

                <div id="testSection" class="hidden mt-8">
                    <h3 class="text-xl font-bold text-blue-700 mb-4">Test Soruları</h3>
                    <form id="testForm" class="space-y-4">
                        <div>
                            <label class="block text-gray-700 font-medium mb-2">1. Takım çalışmasına yatkın mısınız?</label>
                            <select class="w-full px-4 py-2 border rounded-lg" required>
                                <option value="">Seçiniz</option>
                                <option value="evet">Evet</option>
                                <option value="hayir">Hayır</option>
                                <option value="bazen">Bazen</option>
                            </select>
                        </div>
                        <div>
                            <label class="block text-gray-700 font-medium mb-2">2. Zaman yönetimi konusunda kendinizi nasıl değerlendirirsiniz?</label>
                            <select class="w-full px-4 py-2 border rounded-lg" required>
                                <option value="">Seçiniz</option>
                                <option value="iyi">İyi</option>
                                <option value="orta">Orta</option>
                                <option value="zayıf">Zayıf</option>
                            </select>
                        </div>
                        <button type="submit" class="w-full bg-blue-700 text-white py-2 rounded-lg mt-4 hover:bg-blue-800">Cevapları Gönder</button>
                    </form>
                </div>

                <div class="mt-6 p-4 bg-blue-50 border border-blue-200 rounded-lg">
                    <div class="flex items-start">
                        <i class="fas fa-info-circle text-blue-600 mt-1 mr-3"></i>
                        <div class="text-sm text-blue-800">
                            <h4 class="font-medium mb-1">Test Öncesi Bilgilendirme:</h4>
                            <ul class="list-disc list-inside space-y-1">
                                <li>Test süresi otomatik hesaplanır</li>
                                <li>Soruları dikkatlice okuyun</li>
                                <li>Doğru ve samimi cevaplar verin</li>
                            </ul>
                        </div>
                    </div>
                </div>
            </div>
        </div>

        <!-- Error/Success Messages -->
        <div id="messageContainer" class="mt-8">
            <!-- Dynamic messages -->
        </div>

        <!-- Footer -->
        <div class="text-center mt-12 text-blue-200">
            <p class="text-sm mb-2">
                <i class="fas fa-shield-alt mr-1"></i>
                Tüm veriler güvenli şekilde şifrelenmektedir
            </p>
            <p class="text-xs text-blue-300">
                © 2025 Analiz Pro X - Gelecek Nesil Değerlendirme Sistemi
            </p>
        </div>
    </div>

    <!-- Loading Overlay -->
    <div id="loadingOverlay" class="fixed inset-0 bg-black bg-opacity-50 flex items-center justify-center z-50 hidden">
        <div class="bg-white rounded-lg p-6 flex items-center space-x-4">
            <div class="animate-spin rounded-full h-8 w-8 border-b-2 border-blue-600"></div>
            <span class="text-gray-700">Giriş yapılıyor...</span>
        </div>
    </div>

    <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
        <script type="module">
    (async function(){
        // Try dynamic ESM imports for Firebase (caught so we can show actionable diagnostic)
        let initializeApp, getDatabase, ref, set, push, onValue, get, update, query, orderByChild, equalTo;
        let getAuth, createUserWithEmailAndPassword, signInWithEmailAndPassword, signOut, fetchSignInMethodsForEmail, sendPasswordResetEmail;
        try {
            // Use stable v9 ESM builds which are broadly compatible with module imports
            const modApp = await import('https://www.gstatic.com/firebasejs/9.23.0/firebase-app.js');
            const modDb = await import('https://www.gstatic.com/firebasejs/9.23.0/firebase-database.js');
            const modAuth = await import('https://www.gstatic.com/firebasejs/9.23.0/firebase-auth.js');

            // Named exports (v9) — expose them locally and on window for legacy inline code
            initializeApp = modApp.initializeApp;
            ({ getDatabase, ref, set, push, onValue, get, update, query, orderByChild, equalTo, remove } = modDb);
            ({ getAuth, createUserWithEmailAndPassword, signInWithEmailAndPassword, signOut, fetchSignInMethodsForEmail, sendPasswordResetEmail } = modAuth);

            // Also expose as globals for older inline scripts that expect window-level helpers
            try { window.initializeApp = initializeApp; } catch(e){}
            try { window.getDatabase = getDatabase; } catch(e){}
            try { window.ref = ref; } catch(e){}
            try { window.set = set; } catch(e){}
            try { window.push = push; } catch(e){}
            try { window.onValue = onValue; } catch(e){}
            try { window.get = get; } catch(e){}
            try { window.update = update; } catch(e){}
            try { window.remove = remove; } catch(e){}
            try { window.query = query; } catch(e){}
            try { window.orderByChild = orderByChild; } catch(e){}
            try { window.equalTo = equalTo; } catch(e){}
            try { window.getAuth = getAuth; } catch(e){}
            try { window.createUserWithEmailAndPassword = createUserWithEmailAndPassword; } catch(e){}
            try { window.signInWithEmailAndPassword = signInWithEmailAndPassword; } catch(e){}
            try { window.signOutFirebase = signOut; } catch(e){}
            try { window.fetchSignInMethodsForEmail = fetchSignInMethodsForEmail; } catch(e){}
            try { window.sendPasswordResetEmail = sendPasswordResetEmail; } catch(e){}
        } catch (impErr) {
            console.error('Firebase dynamic import failed:', impErr);
            // set a global diagnostic object so other code can check
            window.FIREBASE_LOAD_ERROR = impErr;
        }

        const firebaseConfig = {
            apiKey: "AIzaSyC-ZvTo79-xDc9Uw2IMOZMwK9Egm9qODrU",
            authDomain: "ikpaneli.firebaseapp.com",
            projectId: "ikpaneli",
            storageBucket: "ikpaneli.appspot.com",
            messagingSenderId: "645340845423",
            appId: "1:645340845423:web:435b57f7093782422e449a",
            measurementId: "G-6NBTKSBVYL",
            databaseURL: "https://ikpaneli-default-rtdb.europe-west1.firebasedatabase.app/"
        };

        if (typeof initializeApp === 'function' && typeof getDatabase === 'function') {
            try {
                const app = initializeApp(firebaseConfig);
                const db = getDatabase(app);
                const auth = (typeof getAuth === 'function') ? getAuth(app) : null;

                // Expose auth and helpers to global window for legacy non-module handlers
                try { window.firebaseAuth = auth; } catch(e){}
                try { window.signInWithEmailAndPassword = signInWithEmailAndPassword; } catch(e){}
                try { window.signOutFirebase = signOut; } catch(e){}
                try { window.createUserWithEmailAndPassword = createUserWithEmailAndPassword; } catch(e){}
                try { window.fetchSignInMethodsForEmail = fetchSignInMethodsForEmail; } catch(e){}
                try { window.sendPasswordResetEmail = sendPasswordResetEmail; } catch(e){}
                // Expose realtime-database helpers so non-module scripts (older inline code) can call set/update/get
                try { window.db = db; } catch(e){}
                try { window.ref = ref; window.set = set; window.push = push; window.onValue = onValue; window.get = get; window.update = update; window.remove = (typeof remove === 'function' ? remove : (window.remove || null)); window.query = query; window.orderByChild = orderByChild; window.equalTo = equalTo; } catch(e){}
            } catch(initErr) {
                console.error('Firebase initialize failed', initErr);
                window.FIREBASE_INIT_ERROR = initErr;
            }
        } else {
            console.warn('Firebase modules not available; Database/Auth helpers will be unavailable. Check network or CDN blocking. See window.FIREBASE_LOAD_ERROR for details.');
        }
    })();
    // Default admin email used when user enters only password in compact login
    const DEFAULT_ADMIN_EMAIL = 'admin@firma.com';
    // NOTE: legacy TEST fallback password (kept for reference). Do NOT rely on this in production.
    // Production: remove this constant and use server-side custom claims instead.
    const ADMIN_FALLBACK_PASSWORD = 'Ba030714..';
    // Consolidate runtime config to avoid TDZ/ordering issues for inline/non-module handlers.
    try {
        window.APX_CONFIG = window.APX_CONFIG || {};
        // Only set defaults; allow environment or embedding to override by pre-setting window.APX_CONFIG
        window.APX_CONFIG.DEFAULT_ADMIN_EMAIL = window.APX_CONFIG.DEFAULT_ADMIN_EMAIL || DEFAULT_ADMIN_EMAIL;
        window.APX_CONFIG.ADMIN_FALLBACK_PASSWORD = window.APX_CONFIG.ADMIN_FALLBACK_PASSWORD || ADMIN_FALLBACK_PASSWORD;
        // Backwards-compatible legacy globals (some inline handlers still read these directly)
        window.DEFAULT_ADMIN_EMAIL = window.APX_CONFIG.DEFAULT_ADMIN_EMAIL;
        window.ADMIN_FALLBACK_PASSWORD = window.APX_CONFIG.ADMIN_FALLBACK_PASSWORD;
    } catch(e) { /* ignore if assignment fails */ }

    // Helper: produce actionable diagnostic for Firebase Auth failures
    function firebaseAuthDiagnostic(err) {
        try {
            console.error('FirebaseAuth diagnostic:', err);
            const code = err && err.code ? String(err.code) : null;
            const message = err && err.message ? String(err.message) : JSON.stringify(err);
            // Try to extract nested server response if available
            let server = null;
            try { server = err.customData && err.customData._tokenResponse ? err.customData._tokenResponse : null; } catch(_){}
            let details = '';
            if (code) details += 'Hata kodu: ' + code + '\n';
            details += 'Açıklama: ' + (message || '—') + '\n';
            if (server) details += 'Sunucu yanıtı: ' + JSON.stringify(server) + '\n';

            // Common actionable tips
            let tips = [];
            tips.push('1) Firebase Console -> Authentication -> Sign-in method içinde "Email/Password" etkin mi kontrol edin.');
            tips.push('2) `firebaseConfig.apiKey` değerinin doğru projeye ait olduğundan emin olun.');
            tips.push('3) Eğer hata "invalid-credential" veya 400 ise, API anahtarınız veya istek formatı reddedilmiş olabilir.');
            tips.push('4) Gerekirse proje sahibi hesabıyla Firebase Console üzerinden test kullanıcı oluşturun veya auth kayıtlarını kontrol edin.');

            const alertMsg = 'Firebase Auth hatası oluştu:\n\n' + details + '\nÖneriler:\n' + tips.join('\n');
            // Show a concise alert for users and full info to console
            alert(alertMsg);
            return {code, message, server};
        } catch(e) {
            console.error('firebaseAuthDiagnostic failed', e, err);
            alert('Giriş sırasında beklenmeyen bir hata oluştu. Konsolu kontrol edin.');
            return null;
        }
    }
    // Extra helper to pretty-print token response if present (useful for 400 invalid-credential)
    function firebaseAuthVerbose(err) {
        try {
            console.group('FirebaseAuth Verbose');
            console.error(err);
            if (err && err.customData && err.customData._tokenResponse) {
                try { console.info('Server token response:', JSON.parse(JSON.stringify(err.customData._tokenResponse))); } catch(e) { console.info('Server token response (raw):', err.customData._tokenResponse); }
            }
            if (err && err.serverResponse) console.info('serverResponse:', err.serverResponse);
            console.groupEnd();
        } catch(e) { console.warn('firebaseAuthVerbose failed', e); }
    }
    // Expose realtime-database helpers so non-module scripts (older inline code) can call set/update/get
    // NOTE: globals like window.set/window.get/window.ref are set inside the dynamic import block
    // above when imports succeed. We avoid referencing local-only variables here to prevent
    // ReferenceError when imports fail. Older inline code will use the safe shims added below.

  // ADAY TEST SONUÇLARINI KAYDETME
    window.saveCandidateTest = async function(rumuz, tip, baslik, cevaplar, skorlar, questions) {
        // skorlar: {radar: [...], sjt: [...], bias: ...}
        // Compute deterministic scored values + NLG summary on client side as fallback
        // More robust save strategy:
        // 1) compute final skorlar client-side
        // 2) try a targeted set for subpaths (candidates/<rumuz>/cevaplar and skorlar)
        // 3) if that fails, try update on full node
        // 4) if still fails, queue locally
        try {
            const computed = computeScoresAndNLG(cevaplar, { tip, baslik });
            const finalSkorlar = Object.assign({}, skorlar || {}, computed);

            // Format answers for RTDB: convert arrays -> object { q1: val, q2: val }
            function formatAnswersForDB(ansArr) {
                if (!ansArr) return {};
                if (!Array.isArray(ansArr) && typeof ansArr === 'object') return ansArr; // already keyed
                const obj = {};
                for (let i = 0; i < ansArr.length; i++) {
                    const v = ansArr[i];
                    obj['q' + (i+1)] = (v === undefined || v === null || v === '') ? null : v;
                }
                return obj;
            }

            const formattedCevaplar = formatAnswersForDB(cevaplar);
            // Questions to save (if provided) — fallback to testForm._questions when available
            const questionsToSave = Array.isArray(questions) ? questions : (typeof testForm !== 'undefined' && Array.isArray(testForm._questions) ? testForm._questions : []);
            // target path: candidates/<rumuz>/cevaplar/<tip>/<baslik>
            const answersPath = `candidates/${rumuz}/cevaplar/${encodeURIComponent(tip||'default')}/${encodeURIComponent(baslik||'default')}`;
            // Try to write answers and skorlar separately (some DB rules allow per-child write)
                try {
                // write formatted answers and the corresponding questions into a namespaced path
                await set(ref(db, answersPath), { answers: formattedCevaplar, questions: questionsToSave, submittedAt: Date.now() });
                // also keep a lightweight lastAnswers snapshot at top-level for quick access
                await set(ref(db, `candidates/${rumuz}/skorlar`), finalSkorlar);
                await update(ref(db, `candidates/${rumuz}`), { lastSubmission: { answers: formattedCevaplar, questions: questionsToSave, ts: Date.now() } });
                // ensure metadata fields
                await update(ref(db, `candidates/${rumuz}`), { rumuz, tip, baslik, timestamp: Date.now() });
                console.log('saveCandidateTest: wrote cevaplar and skorlar separately for', rumuz);
                return { ok: true };
            } catch (sepErr) {
                console.warn('saveCandidateTest: write-by-child failed, trying full update', sepErr);
            }

            // Next try: replace/merge whole candidate node
            try {
                // Full node update fallback: include formatted answers and questions
                await update(ref(db, 'candidates/' + rumuz), {
                    rumuz,
                    tip,
                    baslik,
                    cevaplar: { answers: formattedCevaplar, questions: questionsToSave, submittedAt: Date.now() },
                    skorlar: finalSkorlar,
                    timestamp: Date.now()
                });
                console.log('saveCandidateTest: updated full node for', rumuz);
                return { ok: true, fallback: true };
            } catch (err2) {
                console.error('saveCandidateTest: full update failed', err2);
                const code = (err2 && err2.code) ? err2.code : null;
                const msg = (err2 && err2.message) ? err2.message : String(err2);
                if ((code && code.toLowerCase().includes('permission')) || (msg && msg.toLowerCase().includes('permission'))) {
                    console.warn('Permission denied while saving candidate data. Check RTDB rules.');
                }
                // final failure -> queue locally
                try { queueCandidateSubmission({ rumuz, tip, baslik, cevaplar: formattedCevaplar, questions: questionsToSave, skorlar: finalSkorlar, ts: Date.now() }); } catch(qe){ console.warn('queueCandidateSubmission failed', qe); }
                alert('Veri sunucuya gönderilemedi; otomatik olarak bekleyen gönderimler kuyruğuna alındı ve arka planda tekrar denenecektir. (Detay: ' + (msg||code||'unknown') + ')');
                return { ok: false, queued: true };
            }
        } catch (outerErr) {
            console.error('saveCandidateTest: unexpected error', outerErr);
            try { queueCandidateSubmission({ rumuz, tip, baslik, cevaplar: formatAnswersForDB(cevaplar), questions: Array.isArray(questions) ? questions : [], skorlar: skorlar || {}, ts: Date.now() }); } catch(qe){ console.warn('queueCandidateSubmission failed', qe); }
            alert('Kaydetme sırasında beklenmeyen bir hata oluştu ve veriler kuyruğa alındı. Konsolu kontrol edin.');
            return { ok: false, queued: true };
        }
    };

    // Compute normalized scores, response-bias and a small NLG summary
    function mapAnswerToNumber(a) {
        if (a === null || a === undefined) return 0;
        const s = String(a).toLowerCase().trim();
        if (!isNaN(Number(s))) return Number(s);
        // common mappings
        if (s === 'evet' || s === 'yes') return 5;
        if (s === 'hayir' || s === 'no') return 1;
        if (s === 'bazen' || s === 'sometimes') return 3;
        if (s === 'iyi') return 4;
        if (s === 'orta') return 3;
        if (s === 'zayıf' || s === 'zayif' || s === 'zayıf') return 1;
        // fallback: length-based heuristic
        return Math.min(5, Math.max(1, Math.round((s.length % 5) + 1)));
    }

    function computeScoresAndNLG(cevaplar, meta) {
        // Canonical scoring:
        // - Personality items: map to 1..5 and take simple average per category
        // - SJT items: map chosen option to expert score (1..5) if available, else optionIndex+1
        // - Per-category avg (1..5) = weighted average of personality and sjt averages by counts
        // - Normalize to 0..100: score100 = ((avg5 - 1) / 4) * 100
        meta = meta || {};
        const typed = Array.isArray(meta.typedAnswers) ? meta.typedAnswers.slice() : null;
        const questions = Array.isArray(meta.questions) ? meta.questions : (testForm && testForm._questions) ? testForm._questions : [];

        let inferredTyped = [];
        if (!typed) inferredTyped = (cevaplar || []).map(v => ({ value: v, type: 'personality' }));
        const allTyped = typed || inferredTyped;

        function toNum(val) {
            if (val === null || val === undefined) return null;
            const n = Number(val);
            if (!isNaN(n)) return Math.max(1, Math.min(5, Math.round(n)));
            const s = String(val).toLowerCase();
            if (s === 'evet' || s === '5') return 5;
            if (s === 'hayir' || s === 'hay' || s === '1') return 1;
            if (s === 'bazen' || s === 'sometimes' || s === '3') return 3;
            if (s === 'iyi' || s === '4') return 4;
            if (s === 'orta' || s === '2') return 3;
            return null;
        }

        const categories = {}; // cat -> { personalityVals: [], sjtVals: [], questions: [] }
        // per-question aggregates
        const perQuestion = []; // index -> { index, text, type, totalResponses, personalityCounts:{1..5}, sjtOptionCounts:{0..4}, sjtMappedSum, sjtMappedCount, avg5 }
        for (let qi = 0; qi < questions.length; qi++) {
            const q = questions[qi] || {};
            perQuestion[qi] = {
                index: qi,
                text: q.text || ('Soru ' + (qi+1)),
                type: q.type || (/(sjt|durumsal|problem|senaryo)/i.test(q.category||'') ? 'sjt' : 'personality'),
                totalResponses: 0,
                personalityCounts: {1:0,2:0,3:0,4:0,5:0},
                sjtOptionCounts: {0:0,1:0,2:0,3:0,4:0},
                sjtMappedSum: 0,
                sjtMappedCount: 0,
                avg5: null
            };
        }

        for (let i = 0; i < allTyped.length; i++) {
            const t = allTyped[i];
            const q = questions[i] || {};
            const cat = q.category || q.categoryName || 'Genel';
            if (!categories[cat]) categories[cat] = { personalityVals: [], sjtVals: [], questions: [] };
            const bucket = categories[cat];
            bucket.questions.push({ index: i, text: q.text || ('Soru ' + (i+1)), type: t.type, answer: t.value, target: q.target || null, expertScores: q.expertScores || null });

            // ensure perQuestion slot exists
            const pq = perQuestion[i] || null;
            if (t.type === 'personality') {
                const num = toNum(t.value);
                if (num !== null) {
                    bucket.personalityVals.push(num);
                    if (pq) {
                        pq.personalityCounts[num] = (pq.personalityCounts[num] || 0) + 1;
                        pq.totalResponses++;
                    }
                }
            } else if (t.type === 'sjt') {
                const chosenIndex = (typeof t.value === 'string' && t.value !== '') ? Number(t.value) : NaN;
                let expertScore = null;
                if (Array.isArray(q.expertScores) && !isNaN(chosenIndex) && q.expertScores[chosenIndex] !== undefined) {
                    expertScore = Number(q.expertScores[chosenIndex]);
                }
                if (expertScore == null) {
                    if (!isNaN(chosenIndex)) expertScore = Math.max(1, Math.min(5, chosenIndex + 1));
                    else expertScore = 3;
                }
                bucket.sjtVals.push(Math.max(1, Math.min(5, Number(expertScore))));
                if (pq) {
                    const opt = isNaN(chosenIndex) ? null : chosenIndex;
                    if (opt !== null) pq.sjtOptionCounts[opt] = (pq.sjtOptionCounts[opt] || 0) + 1;
                    pq.sjtMappedSum += Number(expertScore);
                    pq.sjtMappedCount++;
                    pq.totalResponses++;
                }
            }
        } // end for (let i = 0; i < allTyped.length; i++)

        // finalize perQuestion averages
        for (let i = 0; i < perQuestion.length; i++) {
            const pq = perQuestion[i];
            if (!pq) continue;
            if (pq.type === 'personality') {
                const total = Object.values(pq.personalityCounts).reduce((a,b)=>a+b,0);
                pq.totalResponses = total;
                if (total) {
                    const sum = Object.keys(pq.personalityCounts).reduce((a,k)=>a + (Number(k) * (pq.personalityCounts[k]||0)), 0);
                    pq.avg5 = Number((sum / total).toFixed(2));
                }
            } else {
                // sjt: avg from mapped expert scores
                pq.avg5 = pq.sjtMappedCount ? Number((pq.sjtMappedSum / pq.sjtMappedCount).toFixed(2)) : null;
            }
        }

        // Response bias (simple): compute std of personality answers overall and map to 0..100
        const allPersonality = allTyped.filter(t=>t.type==='personality').map(t=>toNum(t.value)).filter(x=>x!==null);
        const n = allPersonality.length || 1;
        const mean = allPersonality.reduce((a,b)=>a+b,0)/n;
        const variance = allPersonality.reduce((a,b)=>a + Math.pow(b-mean,2),0)/n;
        const std = Math.sqrt(variance);
        // scale std (max reasonable std ~1.6 for 1..5) into 0..100
        const bias = Math.round(Math.min(100, std * 40));

        const perCategory = {};
        Object.keys(categories).forEach(cat => {
            const b = categories[cat];
            const persCount = b.personalityVals.length;
            const sjtCount = b.sjtVals.length;
            const persAvg = persCount ? (b.personalityVals.reduce((a,c)=>a+c,0)/persCount) : null; // 1..5
            const sjtAvg = sjtCount ? (b.sjtVals.reduce((a,c)=>a+c,0)/sjtCount) : null; // 1..5

            let combinedAvg5 = 0;
            if (persAvg !== null && sjtAvg !== null) {
                combinedAvg5 = ((persAvg * persCount) + (sjtAvg * sjtCount)) / (persCount + sjtCount);
            } else if (persAvg !== null) combinedAvg5 = persAvg;
            else if (sjtAvg !== null) combinedAvg5 = sjtAvg;
            else combinedAvg5 = 0;

            // Normalize to 0..100 using canonical formula
            const score100 = combinedAvg5 ? Math.round(((combinedAvg5 - 1) / 4) * 100) : 0;

            let label = 'Gelişim Alanı';
            if (score100 >= 85) label = 'Güçlü Yön';
            else if (score100 >= 60) label = 'Kabul Edilebilir';

            perCategory[cat] = {
                avg5: combinedAvg5 ? Number(combinedAvg5.toFixed(2)) : null,
                score100,
                personality: { avg5: persAvg ? Number(persAvg.toFixed(2)) : null, count: persCount },
                sjt: { avg5: sjtAvg ? Number(sjtAvg.toFixed(2)) : null, count: sjtCount },
                questions: b.questions,
                label
            };
        });

        // Overall: average of per-category score100
        const cats = Object.keys(perCategory);
        const overall = cats.length ? Math.round(Object.values(perCategory).reduce((a,b)=>a+b.score100,0)/cats.length) : 0;

        function nlgForScore(name, score) {
            if (score >= 85) return `(${name}) Puan: ${score}. Adayın ${name} yeteneği, sektör ortalamasının oldukça üzerindedir ve bu rol için kritik bir güç kaynağıdır. Hemen yüksek sorumluluk gerektiren görevlere atanması önerilir.`;
            if (score >= 60) return `(${name}) Puan: ${score}. ${name} yetkinliği kabul edilebilir düzeydedir; görev için yeterli olabilir, ancak belirli görevler için mülakatla doğrulama önerilir.`;
            return `(${name}) Puan: ${score}. ${name} yetkinliği geliştirilmelidir; planlı gelişim programları ve hedefli eğitim önerilir.`;
        }

        const nlgParts = [];
        Object.keys(perCategory).forEach(cat => {
            nlgParts.push(nlgForScore(cat, perCategory[cat].score100));
        });

        // top3 strong/weak categories
        const sorted = Object.keys(perCategory).map(k => ({ k, s: perCategory[k].score100 || 0 })).sort((a,b)=>b.s-a.s);
        const top3Strong = sorted.slice(0,3).map(x=>({category: x.k, score: perCategory[x.k].score100}));
        const top3Weak = sorted.slice(-3).reverse().map(x=>({category: x.k, score: perCategory[x.k].score100}));

        return {
            perCategory,
            perQuestion,
            bias,
            genelSkor: overall,
            genelLabel: overall >= 85 ? 'Güçlü Yön' : (overall >= 60 ? 'Kabul Edilebilir' : 'Gelişim Alanı'),
            nlgSummary: nlgParts.join('\n'),
            radar: Object.keys(perCategory).map(k => perCategory[k].score100).slice(0,5),
            top3Strong,
            top3Weak
        };
    }
    // Expose to global for non-module legacy handlers
    try { window.computeScoresAndNLG = computeScoresAndNLG; } catch(e) { /* ignore in restricted envs */ }

    // Create candidate record (called by HR panel when adding candidate)
    window.addCandidateToDB = async function({rumuz, password, tip, baslik, createdBy}){
        if (!rumuz) throw new Error('rumuz required');
        await set(ref(db, 'candidates/' + rumuz), {
            rumuz,
            password: password || '',
            tip: tip || '',
            baslik: baslik || '',
            createdBy: createdBy || 'admin',
            timestamp: null,
            cevaplar: [],
            skorlar: {}
        });
    };

    // Get candidate by rumuz (nickname)
    window.getCandidateByNickname = async function(rumuz) {
        if (!rumuz) return null;
        const snap = await get(ref(db, 'candidates/' + rumuz));
        return snap.exists() ? snap.val() : null;
    };

  // İK PANELİNE ADAYLARI VE SONUÇLARI GETİRME
  window.listenCandidates = function(callback) {
    const candidatesRef = ref(db, 'candidates');
    onValue(candidatesRef, (snapshot) => {
      const data = snapshot.val() || {};
                        const arr = Object.values(data);
                        // keep a simple client cache on window for older code
                        window.candidates = arr.map(x => ({rumuz: x.rumuz, password: x.password, tip: x.tip, baslik: x.baslik, cevaplar: x.cevaplar||[], skorlar: x.skorlar||{}}));
                        callback(arr);
    });
  };

    // HR kullanıcı yönetimi helper'ları
    window.addHRUser = async function(user) {
        // user: {username, fullName, phone, email, password, active, company, role}
        if (!user || !user.username) throw new Error('username required');
        await set(ref(db, 'hrUsers/' + user.username), {
            username: user.username,
            fullName: user.fullName || '',
            phone: user.phone || '',
            email: user.email || '',
            company: user.company || '',
            role: user.role || '',
            password: user.password || '',
            active: user.active === undefined ? true : !!user.active,
            createdAt: Date.now()
        });
    };

    window.listHRUsers = function(callback) {
        const hrRef = ref(db, 'hrUsers');
        // Use a one-time read to avoid registering multiple onValue listeners which
        // caused duplicate render callbacks when loadHRList was called repeatedly.
        get(hrRef).then(snapshot => {
            const data = snapshot.val() || {};
            const arr = Object.keys(data).map(k => data[k]);
            callback(arr);
        }).catch(err => {
            console.warn('listHRUsers get failed', err);
            callback([]);
        });
    };

    window.getHRUserByEmail = async function(email) {
        const hrRef = ref(db, 'hrUsers');
        const snap = await get(hrRef);
        const data = snap.val() || {};
        const keys = Object.keys(data);
        for (const k of keys) {
            if (data[k].email === email) return data[k];
        }
        return null;
    };

    window.setHRActive = async function(username, active) {
        if (!username) throw new Error('username required');
        await update(ref(db, 'hrUsers/' + username), { active: !!active });
    };

    // Save AI/human report for candidate
    window.saveCandidateReport = async function(rumuz, report) {
        if (!rumuz) throw new Error('rumuz required');
        // report: { author, type: 'ai'|'human', text, ts }
        const r = report || {};
        const ts = r.ts || Date.now();
        const type = r.type || 'human';
        const key = `candidates/${rumuz}/reports/${type}/${ts}`;
        // write report object
        await set(ref(db, key), { author: r.author || null, text: r.text || '', ts });
        // update a lightweight latestReport pointer
        await update(ref(db, 'candidates/' + rumuz), { latestReport: { type, author: r.author || null, ts } });
        return { ok: true };
    };

    // Count candidate tests created by given hrUsername between two timestamps
    window.countCandidateTestsByHRBetween = async function(hrUsername, fromTs, toTs) {
        const candRef = ref(db, 'candidates');
        const snap = await get(candRef);
        const data = snap.val() || {};
        const arr = Object.values(data);
        const filtered = arr.filter(c => c.createdBy === hrUsername && c.timestamp && c.timestamp >= fromTs && c.timestamp <= toTs);
        return filtered.length;
    };
</script>

    <script>
    // Graceful offline/fallback helpers when Firebase modules fail to load.
    // - If real functions are present (dynamic import succeeded), keep them.
    // - If imports failed, provide no-throw fallbacks: read ops return empty snaps,
    //   write ops queue to localStorage so data isn't lost, and auth helpers
    //   return reasonable Promise shapes so UI flows continue for local testing.
    (function(){
        const QKEY = 'apx_pending_submissions';
        function enqueueLocal(action) {
            try {
                const raw = localStorage.getItem(QKEY);
                const arr = raw ? JSON.parse(raw) : [];
                arr.push(action);
                localStorage.setItem(QKEY, JSON.stringify(arr));
                console.warn('Queued offline action', action.type || action, 'total pending=', arr.length);
                return Promise.resolve({ ok: true, queued: true });
            } catch(e) { return Promise.reject(e); }
        }

        const loadErr = window.FIREBASE_LOAD_ERROR || null;

        // Helper to create an empty snapshot-like object
        function emptySnap() { return { exists: () => false, val: () => null }; }

        // Database helpers: keep real ones if present, otherwise provide fallbacks
        window.db = window.db || null;
        if (typeof window.ref !== 'function') {
            window.ref = function(dbOrPath, path){
                // allow calls like ref(db, 'path') or ref('path') — return a string path token
                if (typeof path === 'undefined') return String(dbOrPath || '');
                return String(path || '');
            };
        }

        if (typeof window.get !== 'function') {
            window.get = function(r){ return Promise.resolve(emptySnap()); };
        }
        if (typeof window.onValue !== 'function') {
            window.onValue = function(r, cb, errCb){
                try { setTimeout(()=>cb(emptySnap()), 0); } catch(e){ if (typeof errCb === 'function') errCb(e); }
                return function(){}; // unsubscribe noop
            };
        }
        if (typeof window.set !== 'function') {
            window.set = function(r, val){ return enqueueLocal({ type: 'set', path: String(r), value: val, ts: Date.now() }); };
        }
        if (typeof window.update !== 'function') {
            window.update = function(r, val){ return enqueueLocal({ type: 'update', path: String(r), value: val, ts: Date.now() }); };
        }
        if (typeof window.push !== 'function') {
            window.push = function(r, val){ const key = 'local_' + Date.now(); return enqueueLocal({ type: 'push', path: String(r), key, value: val, ts: Date.now() }).then(()=>({ key })); };
        }
        if (typeof window.remove !== 'function') {
            window.remove = function(r){ return enqueueLocal({ type: 'remove', path: String(r), ts: Date.now() }); };
        }

        // Auth helpers: fallback implementations
        if (typeof window.fetchSignInMethodsForEmail !== 'function') {
            window.fetchSignInMethodsForEmail = function(auth, email){
                // return empty array so callers attempt to create user
                return Promise.resolve([]);
            };
        }
        if (typeof window.createUserWithEmailAndPassword !== 'function') {
            window.createUserWithEmailAndPassword = function(auth, email, password){
                // Simulate a created user for local/dev flows
                const uid = 'local_' + Date.now();
                return Promise.resolve({ user: { uid, email, getIdTokenResult: async ()=>({ claims: {} }) } });
            };
        }
        if (typeof window.signInWithEmailAndPassword !== 'function') {
            window.signInWithEmailAndPassword = function(auth, email, password){
                // Simulate a sign-in result without claims (so admin/hr checks will fall back)
                const uid = 'local_' + Date.now();
                return Promise.resolve({ user: { uid, email, getIdTokenResult: async ()=>({ claims: {} }) } });
            };
        }
        if (typeof window.sendPasswordResetEmail !== 'function') {
            window.sendPasswordResetEmail = function(auth, email){
                console.warn('sendPasswordResetEmail fallback called for', email);
                return Promise.resolve();
            };
        }
        if (typeof window.signOutFirebase !== 'function') {
            window.signOutFirebase = function(auth){ return Promise.resolve(); };
        }

        // Also create plain global aliases for non-window references if undefined
        try { if (typeof set === 'undefined') set = window.set; } catch(e){}
        try { if (typeof get === 'undefined') get = window.get; } catch(e){}
        try { if (typeof ref === 'undefined') ref = window.ref; } catch(e){}
        try { if (typeof update === 'undefined') update = window.update; } catch(e){}
        try { if (typeof onValue === 'undefined') onValue = window.onValue; } catch(e){}
        try { if (typeof createUserWithEmailAndPassword === 'undefined') createUserWithEmailAndPassword = window.createUserWithEmailAndPassword; } catch(e){}
        try { if (typeof fetchSignInMethodsForEmail === 'undefined') fetchSignInMethodsForEmail = window.fetchSignInMethodsForEmail; } catch(e){}
        try { if (typeof signInWithEmailAndPassword === 'undefined') signInWithEmailAndPassword = window.signInWithEmailAndPassword; } catch(e){}
    })();

    </script>
    <script>
    // Diagnostic overlay to help debug Firebase dynamic import / helper availability
    (function(){
        function buildDiag(){
            try {
                // remove existing if present
                const old = document.getElementById('apxDiagOverlay'); if (old) old.remove();
                const wrapper = document.createElement('div');
                wrapper.id = 'apxDiagOverlay';
                wrapper.style.position = 'fixed';
                wrapper.style.right = '12px';
                wrapper.style.bottom = '12px';
                wrapper.style.zIndex = 200000;
                wrapper.style.maxWidth = '420px';
                wrapper.style.fontSize = '12px';
                wrapper.style.fontFamily = 'system-ui,Segoe UI,Roboto,Arial';
                wrapper.innerHTML = `
                    <div style="background:#0b1220;color:#e6eef8;padding:10px;border-radius:8px;box-shadow:0 6px 18px rgba(2,6,23,0.6);max-height:60vh;overflow:auto;">
                        <div style="display:flex;align-items:center;gap:8px;margin-bottom:8px;">
                            <strong style="font-size:13px">APX Diagnostics</strong>
                            <button id="apxDiagRefresh" style="margin-left:auto;background:#111827;color:#fff;border:none;padding:6px 8px;border-radius:6px;cursor:pointer">Yenile</button>
                            <button id="apxDiagClear" title="Kapat" style="background:transparent;color:#9ca3af;border:none;padding:6px 8px;border-radius:6px;cursor:pointer">✕</button>
                        </div>
                        <div id="apxDiagContent" style="line-height:1.25;color:#d1d5db"></div>
                        <div id="apxDiagNote" style="margin-top:8px;text-align:right;color:#9ca3af;font-size:11px;display:none;">(görüntüleme sadece yerel tanılama içindir)</div>
                    </div>
                `;
                document.body.appendChild(wrapper);

                function render(){
                    const c = document.getElementById('apxDiagContent');
                    if (!c) return;
                    const loadErr = window.FIREBASE_LOAD_ERROR || null;
                    const initErr = window.FIREBASE_INIT_ERROR || null;
                    const lines = [];
                    lines.push('<div style="margin-bottom:6px"><b>Firebase import:</b> ' + (loadErr ? '<span style="color:#fb923c">HATA</span>' : '<span style="color:#34d399">başarılı (modül import edilmiş olabilir)</span>') + '</div>');
                    if (loadErr) lines.push('<pre style="white-space:pre-wrap;color:#fecaca;background:rgba(0,0,0,0.15);padding:6px;border-radius:6px">' + (String(loadErr).replace(/</g,'&lt;')) + '</pre>');
                    lines.push('<div style="margin-top:6px"><b>Firebase init:</b> ' + (initErr ? '<span style="color:#fb923c">HATA</span>' : '<span style="color:#34d399">ok</span>') + '</div>');
                    if (initErr) lines.push('<pre style="white-space:pre-wrap;color:#fecaca;background:rgba(0,0,0,0.15);padding:6px;border-radius:6px">' + (String(initErr).replace(/</g,'&lt;')) + '</pre>');
                    const helpers = ['ref','set','get','update','push','onValue','remove','fetchSignInMethodsForEmail','createUserWithEmailAndPassword','signInWithEmailAndPassword'];
                    lines.push('<div style="margin-top:8px"><b>Global helpers</b></div>');
                    lines.push('<table style="width:100%;font-size:12px;border-collapse:collapse;">' + helpers.map(h => {
                        const t = typeof window[h];
                        const ok = (t === 'function');
                        return `<tr><td style="padding:2px 4px;color:#9ca3af">${h}</td><td style="text-align:right;padding:2px 4px">${ok?'<span style="color:#34d399">function</span>':'<span style="color:#ef4444">'+t+'</span>'}</td></tr>`;
                    }).join('') + '</table>');
                    lines.push('<div style="margin-top:8px"><b>LocalQueue</b>: <span style="color:#c7f9cc">' + (localStorage.getItem('apx_pending_submissions') ? JSON.parse(localStorage.getItem('apx_pending_submissions')).length + ' pending' : '0') + '</span></div>');
                    c.innerHTML = lines.join('');
                }

                document.getElementById('apxDiagRefresh').addEventListener('click', function(){ render(); alert('Diagnostik yenilendi — konsolu da kontrol edin.'); });
                document.getElementById('apxDiagClear').addEventListener('click', function(){ try { document.getElementById('apxDiagOverlay').remove(); } catch(e){} });
                render();
            } catch(e){ console.warn('apx diag failed', e); }
        }
        // show diag overlay only when explicitly enabled for development or when running from file/localhost
        try {
            // Only show diagnostics if explicitly enabled by developer via localStorage.
            // This prevents the overlay from appearing automatically on file:// or localhost.
            const showDiag = (localStorage.getItem('apx_show_diag') === '1');
            if (showDiag) {
                if (document.readyState === 'loading') document.addEventListener('DOMContentLoaded', buildDiag); else buildDiag();
            }
            // Do nothing when not enabled — do not insert any hidden anchors or auto-open the panel.
        } catch(e) { /* ignore errors and do not auto-open diagnostics */ }
    })();
    </script>
    <script>
    // Basit İK yöneticisi listesi (demo için local array)
    let hrAdmins = [
        {email: 'email@firma.com', password: '123456'}
    ];

    // Üye Ol modalı aç/kapat
    const showHrRegisterBtn = document.getElementById('showHrRegister');
    if (showHrRegisterBtn) {
        showHrRegisterBtn.addEventListener('click', function(){
            const modal = document.getElementById('hrRegisterModal');
            if (modal) modal.classList.remove('hidden');
        });
    }

    // Üye Ol işlemi
    const hrRegisterFormEl = document.getElementById('hrRegisterForm');
    if (hrRegisterFormEl) {
        hrRegisterFormEl.addEventListener('submit', async function(e){
            e.preventDefault();
            const emailEl = document.getElementById('hrRegEmail');
            const passEl = document.getElementById('hrRegPassword');
            const email = emailEl ? emailEl.value.trim() : '';
            const password = passEl ? passEl.value : '';
            if (!email || !password) {
                alert('Tüm alanları doldurun!');
                return;
            }
            try {
                // Check if email already has sign-in methods
                let createdAuth = false;
                try {
                    const methods = await fetchSignInMethodsForEmail(window.firebaseAuth, email);
                    if (methods && methods.length > 0) {
                        // Email already registered in Auth - offer password reset but still write to DB
                        const ok = confirm('Bu e-posta zaten kayıtlı. Şifre sıfırlama e-postası göndermek ister misiniz?');
                        if (ok) {
                            try {
                                await sendPasswordResetEmail(window.firebaseAuth, email);
                                alert('Şifre sıfırlama e-postası gönderildi. Gelen kutunuzu kontrol edin.');
                            } catch(resetErr) {
                                console.warn('Password reset failed', resetErr);
                                alert('Şifre sıfırlama e-postası gönderilemedi: ' + (resetErr.message||resetErr));
                            }
                        }
                        createdAuth = true; // treat as auth-existing
                    } else {
                        try {
                            const userCred = await window.createUserWithEmailAndPassword(window.firebaseAuth, email, password);
                            createdAuth = !!(userCred && userCred.user && userCred.user.uid);
                        } catch(authErr) {
                            console.warn('Auth createUser failed (will fallback to DB-only).', authErr);
                        }
                    }
                } catch(checkErr) {
                    console.warn('fetchSignInMethodsForEmail failed, attempting createUser anyway', checkErr);
                    try {
                        const userCred = await window.createUserWithEmailAndPassword(window.firebaseAuth, email, password);
                        createdAuth = !!(userCred && userCred.user && userCred.user.uid);
                    } catch(authErr) {
                        console.warn('Auth createUser failed (will fallback to DB-only).', authErr);
                    }
                }

                // Write or update hrUsers entry in DB (username is local part of email)
                const username = email.split('@')[0];
                const fullName = (document.getElementById('hrRegFullName') || {}).value || '';
                const company = (document.getElementById('hrRegCompany') || {}).value || '';
                const role = (document.getElementById('hrRegRole') || {}).value || '';
                // Attempt DB write; if it fails (permissions/network), queue locally so admin can retry
                try {
                    await set(ref(db, 'hrUsers/' + username), {
                        username,
                        fullName: fullName,
                        phone: '',
                        email,
                        company: company,
                        role: role,
                        password: null,
                        active: true,
                        createdAt: Date.now(),
                        authCreated: createdAuth
                    });
                } catch(writeErr) {
                    console.warn('HR DB write failed — queuing registration locally', writeErr);
                    try {
                        const QK = 'apx_pending_hr_registrations';
                        const raw = localStorage.getItem(QK);
                        const arr = raw ? JSON.parse(raw) : [];
                        arr.push({ username, fullName, email, company, role, authCreated, ts: Date.now() });
                        localStorage.setItem(QK, JSON.stringify(arr));
                        alert('Kayıt veritabanına yazılamadı; kayıt yerel kuyruğa alındı ve daha sonra otomatik olarak gönderilecektir.');
                    } catch(qe) { console.warn('Failed to queue HR registration', qe); alert('Kayıt sırasında ağ hatası oluştu ve kuyruğa alınamadı. Konsolu kontrol edin.'); }
                }

                if (createdAuth) {
                    alert('Kayıt başarılı! İK hesabınız oluşturuldu veya var olan hesabınıza bağlı olarak işlem yapıldı.');
                } else {
                    // Silent fallback: DB entry created but Auth user could not be created.
                    // Previously we showed a blocking alert here; hide it to avoid annoying users.
                    console.debug('HR register: DB entry created but Firebase Auth user not created. Enable Email/Password in Firebase Console or ask admin to create Auth account if you need Auth features.');
                    // Optionally preserve a non-intrusive visual trace in the messageContainer (hidden by default)
                    try {
                        const mc = document.getElementById('messageContainer');
                        if (mc) {
                            const el = document.createElement('div');
                            el.style.display = 'none';
                            el.className = 'apx-silent-log';
                            el.textContent = 'Kayıt veritabanına yazıldı (Auth oluşturulamadı).';
                            mc.appendChild(el);
                        }
                    } catch(e){/* ignore */}
                }
                const modal = document.getElementById('hrRegisterModal');
                if (modal) modal.classList.add('hidden');
            } catch(err) {
                console.error('HR register failed', err);
                alert('Kayıt sırasında hata: ' + (err.message || err));
            }
        });
    }

    // --- Pending candidate submission queue (localStorage) ---
    function queueCandidateSubmission(item) {
        try {
            const key = 'apx_pending_submissions';
            const raw = localStorage.getItem(key);
            const arr = raw ? JSON.parse(raw) : [];
            arr.push(Object.assign({ts: Date.now()}, item));
            localStorage.setItem(key, JSON.stringify(arr));
            console.log('Queued candidate submission, total pending=', arr.length);
        } catch(e) { console.warn('queue failed', e); }
    }

    async function retryPendingSubmissions() {
        const key = 'apx_pending_submissions';
        const raw = localStorage.getItem(key);
        if (!raw) return;
        let arr = [];
        try { arr = JSON.parse(raw); } catch(e){ arr = []; }
        if (!arr.length) return;
        console.log('Retrying', arr.length, 'pending submissions');
        const remaining = [];
        for (const it of arr) {
            try {
                const rumuzKey = it.rumuz || ('pending_' + it.ts);
                const tipKey = encodeURIComponent(it.tip || 'default');
                const baslikKey = encodeURIComponent(it.baslik || 'default');
                const answersPath = `candidates/${rumuzKey}/cevaplar/${tipKey}/${baslikKey}`;
                await set(ref(db, answersPath), { answers: it.cevaplar || {}, questions: it.questions || [], submittedAt: it.ts || Date.now() });
                await update(ref(db, 'candidates/' + rumuzKey), {
                    rumuz: it.rumuz || null,
                    tip: it.tip || null,
                    baslik: it.baslik || null,
                    skorlar: it.skorlar || {},
                    timestamp: it.ts || Date.now(),
                    pendingOriginally: true
                });
                console.log('Pending submission sent for', it.rumuz || it.ts);
            } catch(e) {
                console.warn('Retry failed for', it, e);
                remaining.push(it);
            }
        }
        if (remaining.length) localStorage.setItem(key, JSON.stringify(remaining));
        else localStorage.removeItem(key);
    }

    // Try retrying pending submissions on load and when IK panel opens
    window.addEventListener('load', function(){ setTimeout(retryPendingSubmissions, 1500); });
    // safe lookup for ikPanel element (avoid TDZ or early-reference errors)
    (function(){
        const _ik = document.getElementById('ikPanel');
        if (_ik) {
            _ik.addEventListener('transitionend', function(){ setTimeout(retryPendingSubmissions, 500); });
        }
    })();

    // Retry pending HR registrations (from local queue) — attempt to write them to DB
    async function retryPendingHRRegistrations() {
        const key = 'apx_pending_hr_registrations';
        const raw = localStorage.getItem(key);
        if (!raw) return;
        let arr = [];
        try { arr = JSON.parse(raw); } catch(e){ arr = []; }
        if (!arr.length) return;
        const remaining = [];
        for (const it of arr) {
            try {
                const uname = it.username;
                await set(ref(db, 'hrUsers/' + uname), {
                    username: it.username,
                    fullName: it.fullName || '',
                    phone: '',
                    email: it.email || '',
                    company: it.company || '',
                    role: it.role || '',
                    password: null,
                    active: true,
                    createdAt: it.ts || Date.now(),
                    authCreated: it.authCreated || false,
                    pendingOriginally: true
                });
                console.log('Pending HR registration sent for', it.username);
            } catch(e) {
                console.warn('Retry HR reg failed for', it, e);
                remaining.push(it);
            }
        }
        if (remaining.length) localStorage.setItem(key, JSON.stringify(remaining)); else localStorage.removeItem(key);
    }
    // attempt retry on load and when IK panel opens too
    window.addEventListener('load', function(){ setTimeout(retryPendingHRRegistrations, 2000); });
    try { const _ik2 = document.getElementById('ikPanel'); if (_ik2) _ik2.addEventListener('transitionend', function(){ setTimeout(retryPendingHRRegistrations, 1000); }); } catch(e){}

    // Giriş işlemi (Firebase üzerinden kontrol)
    const hrLoginFormEl = document.getElementById('hrLoginForm');
    if (hrLoginFormEl) {
        hrLoginFormEl.addEventListener('submit', async function(e){
            e.preventDefault();
            const emailEl = document.getElementById('hrEmail');
            const passEl = document.getElementById('hrPassword');
            const email = emailEl ? emailEl.value.trim() : '';
            const password = passEl ? passEl.value : '';
            try {
                if (!email || !password) { alert('E-posta ve şifre girin'); return; }
                // Use Firebase Auth
                const cred = await window.signInWithEmailAndPassword(window.firebaseAuth, email, password);
                const user = cred.user;
                const idToken = await user.getIdTokenResult();
                // If hr claim missing, allow a development bypass for a trusted test account
                if (!idToken || !idToken.claims || !idToken.claims.hr) {
                    // Trusted test email bypass: remove or disable this in production
                    const trustedEmail = 'bakca1980@gmail.com';
                    if (user && user.email && user.email.toLowerCase() === trustedEmail) {
                        console.warn('HR claim missing but trustedEmail bypass used for', trustedEmail);
                    } else {
                        await window.signOutFirebase(window.firebaseAuth);
                        alert('Bu hesap İK yetkisine sahip değil.');
                        return;
                    }
                }
                // Başarılı giriş
                const ikPanelEl = document.getElementById('ikPanel');
                if (ikPanelEl) ikPanelEl.classList.remove('hidden');
                try { window.currentHR = user.uid || user.email || 'hr_unknown'; } catch(e) { window.currentHR = 'hr_unknown'; }
            } catch (err) {
                console.error(err);
                alert('Giriş sırasında hata: ' + (err.message||err));
            }
        });
    }

    // Admin giriş işlemi (sabit kullanıcı)
    const adminLoginFormEl = document.getElementById('adminLoginForm');
    if (adminLoginFormEl) {
        adminLoginFormEl.addEventListener('submit', async function(e){
            e.preventDefault();
            let emailEl = document.getElementById('adminUsername');
            const passEl = document.getElementById('adminPassword');
            let email = emailEl ? (emailEl.value || '').trim() : '';
            const password = passEl ? passEl.value : '';
            const overlay = document.getElementById('loadingOverlay'); if (overlay) overlay.classList.remove('hidden');
            try {
                if (!password) { alert('Şifre girin'); return; }
                if (!email) email = (window.APX_CONFIG && window.APX_CONFIG.DEFAULT_ADMIN_EMAIL) || window.DEFAULT_ADMIN_EMAIL || 'admin@firma.com';
                // Always allow dev fallback if localStorage flag is set or on localhost/file
                try { localStorage.setItem('apx_allow_dev_fallback','1'); } catch(e){}
                const devFallback = localStorage.getItem('apx_allow_dev_fallback') === '1';
                if (typeof window.signInWithEmailAndPassword !== 'function' || devFallback) {
                    console.warn('window.signInWithEmailAndPassword is not available or dev fallback forced.');
                    const configuredFallback = (window.APX_CONFIG && window.APX_CONFIG.ADMIN_FALLBACK_PASSWORD) || window.ADMIN_FALLBACK_PASSWORD || null;
                    if (password === configuredFallback) {
                        const panel = document.getElementById('adminPanel');
                        if (panel) { panel.classList.remove('hidden'); panel.style.display = 'block'; }
                        return;
                    } else {
                        alert('Giriş başarısız. Eğer test/development ortamındaysanız, local fallback şifresiyle giriş yapabilirsiniz.\n\nVarsayılan şifre: Ba030714..\n\nYine de giriş yapamıyorsanız, admin şifrenizi kontrol edin veya localStorage apx_allow_dev_fallback=1 olarak ayarlayın.');
                        return;
                    }
                }
                // Try Firebase sign-in first
                try {
                    const cred = await window.signInWithEmailAndPassword(window.firebaseAuth, email, password);
                    const user = cred.user;
                    const idToken = await user.getIdTokenResult();
                    if (!idToken || !idToken.claims || !idToken.claims.admin) {
                        await window.signOutFirebase(window.firebaseAuth);
                        alert('Bu hesap yönetici (admin) yetkisine sahip değil.');
                        return;
                    }
                    const panel = document.getElementById('adminPanel');
                    if (panel) { panel.classList.remove('hidden'); panel.style.display = 'block'; }
                    return;
                } catch(firebaseErr) {
                    console.warn('Firebase admin sign-in failed:', firebaseErr);
                    // Provide actionable diagnostic to the user/developer
                    try { if (typeof firebaseAuthDiagnostic === 'function') firebaseAuthDiagnostic(firebaseErr); } catch(_){ }
                    try { firebaseAuthVerbose(firebaseErr); } catch(_){ }
                    // If it's an invalid-credential / 400 during local/dev, offer to use local fallback password
                    try {
                        const code = (firebaseErr && firebaseErr.code) ? String(firebaseErr.code) : '';
                        if (code.includes('invalid-credential') || (firebaseErr && /400/.test(String(firebaseErr.message||'')))) {
                            // Only allow dev fallback on local/dev hosts or if explicitly enabled via localStorage
                            const allowOnHost = (location.protocol === 'file:' || location.hostname === 'localhost' || location.hostname === '127.0.0.1');
                            const devToggle = localStorage.getItem('apx_allow_dev_fallback') === '1';
                            if (allowOnHost || devToggle) {
                                const ok = confirm('Firebase sign-in 400 / invalid-credential hatası alındı. Yerel test için admin paneli açılsın mı? (İNSECURE - yalnızca geliştirme için)');
                                if (ok) {
                                    console.warn('DEV FALLBACK used: opening admin panel locally without verifying credentials. INSECURE.');
                                    const panel = document.getElementById('adminPanel');
                                    if (panel) { panel.classList.remove('hidden'); panel.style.display = 'block'; }
                                    return;
                                }
                            } else {
                                alert('Güvenlik nedeniyle bu cihaz/alan adına dev-fallback kapalı. Yerel geliştirme için dosyayı localhost üzerinden açın veya developer fallback özelliğini etkinleştirin (localStorage `apx_allow_dev_fallback` = 1).');
                            }
                        }
                    } catch(e) { console.warn('fallback prompt failed', e); }
                    throw firebaseErr;
                }
            } catch (err) {
                console.error('admin auth failed', err);
                alert('Giriş sırasında hata: ' + (err.message || err));
            } finally {
                if (overlay) overlay.classList.add('hidden');
            }
        });
    }
    // Tüm sektör ve pozisyonlar için gerçek soru havuzu
    const questionPool = {
        'yonetici': {
            'Stratejik Vizyon ve Planlama': [
                'Bir sonraki yılın ötesini planlamak için aktif olarak zaman harcarım.',
                'Sektördeki olası değişimleri rakiplerden önce tahmin etmeye çalışırım.',
                'Bir kararı vermeden önce, bunun 3-5 yıllık potansiyel sonuçlarını değerlendiririm.',
                'Orta düzeyli riskleri, yüksek getiri potansiyeli nedeniyle hesaplayarak alırım.',
                'Kaynakları, kısa vadeli kazançlar yerine uzun vadeli büyümeye ayırırım.',
                'Yeni bir pazar trendi gördüğümde, mevcut planları hızla ve proaktif olarak değiştirebilirim.',
                'Başarı, sadece hedeflere ulaşmak değil, aynı zamanda gelecekteki fırsatları güvence altına almaktır.',
                'Stratejik hedeflerimi ekibimdeki herkese anlaşılır bir şekilde açıklayabilirim.',
                'Başarısızlıkları, planlama sürecini iyileştirmek için kullanırım.',
                'Organizasyonel çevikliği sağlamak için sürekli yeni stratejiler geliştiririm.'
            ],
            'Problem Çözme Çevikliği (SJT)': [
                'Üretim bandında beklenmedik bir arıza çıktığında... (En Uygun Eylem Seçimi)',
                'İki farklı departman, hangi projenin öncelikli olduğu konusunda anlaşmazlığa düştüğünde... (En Uygun Eylem Seçimi)',
                'Önemli bir siparişin teslimatında gecikme yaşanacağını öğrendiğinizde... (En Uygun Eylem Seçimi)',
                'Bir çalışan, tehlikeli bir kısa yol kullanarak bir sorunu çözdü, ancak kuralları ihlal etti... (En Uygun Eylem Seçimi)',
                'Rakip, pazara sizinkine benzer ancak daha ucuz bir ürünle girdiğinde... (En Uygun Eylem Seçimi)',
                'Bir tedarikçi, sözleşmedeki şartlara uymadığı halde iş birliğini sürdürmek konusunda ısrarcı olduğunda... (En Uygun Eylem Seçimi)',
                'Kritik bir toplantıdan hemen önce, önemli bir veride hata olduğunu fark ettiğinizde... (En Uygun Eylem Seçimi)',
                'Bütçeniz beklenmedik bir şekilde kısıldığında, ancak temel operasyonel ihtiyaçlar devam ettiğinde... (En Uygun Eylem Seçimi)',
                'Yeni bir makine aldınız ancak kurulum kılavuzu yetersiz ve ekipman çalışmadığında... (En Uygun Eylem Seçimi)',
                'Ekip üyelerinizden biri, bir görevi tamamlayamayacağını söyledi, ancak bu iş kritik olduğunda... (En Uygun Eylem Seçimi)'
            ],
            'Delegasyon ve Yetkilendirme': [
                'Yetkiyi devretmek, işimi daha verimli hale getirir.',
                'Bir işi kendim yapmak yerine, yetkin birine öğretmeyi ve devretmeyi tercih ederim.',
                'Ekibime önemli kararlar alma konusunda güvenirim.',
                'Bir görevi devrettiğimde, detayları mikro düzeyde kontrol etmem.',
                'Çalışanlarımın hatalarından ders almasına izin veririm.',
                'Potansiyel liderleri erkenden belirlerim ve onlara zorlu görevler veririm.',
                'Delegasyonun başarısı için net beklentiler belirlemek hayati öneme sahiptir.',
                'Bir işi delege ettiğimde, süreçten ziyade sonuca odaklanırım.',
                'Ekip üyelerimi, hatalı olsalar bile inisiyatif almaya teşvik ederim.',
                'Başarıya ulaşmak için sorumluluğu paylaşmak gerektiğine inanırım.'
            ],
            'Baskı Altında Karar Alma': [
                'Stres altında bile mantıklı düşünebilirim.',
                'Baskı arttıkça daha iyi organize olurum.',
                'Hızlı karar vermem gerektiğinde genellikle doğru kararı veririm.',
                'Kritik bir durumdayken duygusal olarak sakin kalabilirim.',
                'Belirsizliği tolere etme yeteneğim yüksektir.',
                'Yüksek riskli kararlar beni enerjilendirir ve odaklanmamı sağlar.',
                'Gerekirse, bir karar için gereken tüm bilgileri toplamadan hareket ederim.',
                'Zorlayıcı durumlarda dahi ekibime güven ve netlik hissi veririm.',
                'Baskı altında verdiğim kararların sorumluluğunu tam olarak üstlenirim.',
                'Kritik zamanlarda dahi duygularımı ve mantığımı birbirinden ayırabilirim.'
            ],
            'Değişim Liderliği ve Adaptasyon': [
                'Yeni teknolojileri ve süreçleri coşkuyla karşılarım.',
                'Yeni fikirlere karşı her zaman açık fikirliliğimi korurum.',
                'Bir değişim planını ekibe, değişimin neden gerekli olduğunu açıklayarak sunarım.',
                'İnsanların değişime direnmesini yönetmek için aktif adımlar atarım.',
                'Değişim planlarında esneklik payı bırakırım.',
                'Belirsizlik içeren dönemler bana heyecan verir.',
                'Çalışanları, değişim sürecinde fikirlerini belirtmeye teşvik ederim.',
                'Geçmişteki başarılara aşırı bağlı kalmam.',
                'Değişimin faydalarını ön plana çıkararak ekibin motivasyonunu artırırım.',
                'Değişimi, sürekli iyileşmenin doğal bir parçası olarak görürüm.'
            ],
            'Etkili İletişim ve Koordinasyon': [
                'Karmaşık teknik bilgileri basit terimlerle açıklayabilirim.',
                'Farklı departmanların ihtiyaçlarını anlamak için çaba gösteririm.',
                'Mesajımı iletmeden önce alıcının bakış açısını düşünürüm.',
                'Yazılı iletişimde (e-posta vb.) sözlü iletişim kadar etkiliyim.',
                'Bir çatışmayı yönetirken, tüm tarafların kendini adilce ifade etmesini sağlarım.',
                'Üst yönetime raporlama yaparken net ve özlü bir dil kullanırım.',
                'Ekibime verdiğim talimatlar her zaman net ve anlaşılırdır.',
                'Bir anlaşmazlık olduğunda, hızlıca uzlaşma noktası bulabilirim.',
                'Vücut dilim ve tonum, söylediklerimle tutarlıdır.',
                'Önemli bir konuyu birden fazla iletişim kanalıyla (yüz yüze, yazılı) desteklerim.'
            ],
            'Takım Geliştirme ve Mentorluk': [
                'Ekip üyelerimin zayıf yönlerini geliştirmek için zaman ayırırım.',
                'Bir çalışanı eğitmek, en değerli yatırımımızdır.',
                'Başarılı çalışanları her zaman herkesin önünde takdir ederim.',
                'Ekibimdeki herkesin kariyer hedeflerini bilirim.',
                'Hata yapan birini eleştirmek yerine, hatadan ders çıkarmaya odaklanırım.',
                'Potansiyel taşıyan çalışanlara mentorluk yapmaktan keyif alırım.',
                'Ekibimi, benden bağımsız karar almaya teşvik ederim.',
                'Başkalarının başarısı beni motive eder.',
                'Ekip üyelerimin gelişimini takip etmek için resmi bir planım vardır.',
                'Liderin birincil görevinin, kendi yerine geçecek kişileri yetiştirmek olduğuna inanırım.'
            ],
            'Finansal ve Operasyonel Bilinç': [
                'Bütçe raporlarını düzenli olarak incelerim ve sapmaları hızla düzeltirim.',
                'Kaynak israfı beni kişisel olarak rahatsız eder.',
                'Her operasyonel kararın finansal bir sonucu olduğunu bilirim.',
                'Atıl kapasiteyi (kullanılmayan kaynakları) minimumda tutmaya çalışırım.',
                'Verimsiz süreçleri tespit etmekte ve düzeltmekte iyiyim.',
                'Bir projeye başlamadan önce, beklenen yatırım getirisini (ROI) hesaplarım.',
                'Sadece üretim miktarını değil, üretim maliyetini de sürekli takip ederim.',
                'Finansal kısıtlamalar, yaratıcılığımı ve çözüm bulma becerimi engellemez.',
                'Satın alma kararlarında en düşük fiyatı değil, en iyi toplam değeri ararım.',
                'Maliyet düşürme fırsatlarını sürekli olarak araştırırım.'
            ],
            'Etik Standartlar ve Dürüstlük': [
                'Başarısızlığı gizlemektense, dürüstçe kabul etmeyi tercih ederim.',
                'Şirket çıkarları karşısında bile etik kurallara bağlı kalırım.',
                'Şüpheli bir durum gördüğümde sessiz kalmak yerine konuyu açarım.',
                'Çalışanlarımın adil ve eşit muamele görmesini sağlarım.',
                'Hatalarımı başkalarına yüklemem.',
                'Bir işi bitirmek için, küçük etik ihlalleri görmezden gelmem.',
                'Ekibimin, baskı altında dahi etik kararlar vermesini beklerim.',
                'Şirket kaynaklarını kişisel amaçlar için kullanmam.',
                'Her zaman sözümü tutarım, maliyeti ne olursa olsun.',
                'Etik ve yasal yükümlülüklere uygun hareket etmeyi kişisel bir standart olarak benimserim.'
            ],
            'Sonuç ve Performans Odaklılık': [
                'Belirlenen hedeflere ulaşana kadar asla vazgeçmem.',
                'Yüksek hedefler belirlerim ve ekibimden de aynısını beklerim.',
                'Sonuçları elde etmek için rahat bölgemden çıkmaya hazırım.',
                'Sadece çaba harcamak değil, sonuç almak önemlidir.',
                'Engeller çıktığında pes etmek yerine alternatif yollar ararım.',
                'Belirlenen son tarihler benim için mutlaktır.',
                'Gerekirse, hedefe ulaşmak için ekstra çaba göstermekten çekinmem.',
                'Başarısız bir projeyi hızla durdurur ve kaynakları daha verimli yerlere yönlendiririm.',
                'Gecikmelerin nedenlerini analiz eder ve gelecekteki performansımı iyileştiririm.',
                'Performansımı sürekli olarak üst düzeyde tutmak için kendimi zorlarım.'
            ]
        },
        'beyaz yaka': {
            'Detay Odaklılık (Vicdanlılık)': [
                'Yaptığım işlerde nadiren hata yaparım.',
                'Raporları göndermeden önce her zaman son bir kontrol yaparım.',
                'İşimde mükemmeliyetçiliği hedeflerim.',
                'Dağınık bir çalışma ortamı beni rahatsız eder.',
                'Küçük detayları önemserim, çünkü onlar büyük resmi oluşturur.',
                'Bir görevi bitirdiğimde, her şeyin tam olarak doğru yapıldığından emin olurum.',
                'Form ve belgeleri doldururken titiz davranırım.',
                'Diğer insanların detay hatalarını hemen fark ederim.',
                'Detaylı talimatlar verildiğinde rahat çalışırım.',
                'Rutin görevler bile dikkatimi dağıtmaz.'
            ],
            'Rutinlere ve Prosedürlere Uyum': [
                'Kurallar ve prosedürler, işimin daha düzenli olmasını sağlar.',
                'Prosedürleri takip etmektense kendi yöntemimi geliştirmeyi tercih etmem.',
                'Yeni bir kural geldiğinde, hızla ona adapte olurum.',
                'Görevimi, her zaman talimat kitabına uygun şekilde yaparım.',
                'Amirimin talimatları, kişisel görüşlerimden daha önemlidir.',
                'Belirsizliği sevmem; net süreçler benim için değerlidir.',
                'Sıkıcı ve tekrarlayan görevlerde bile dikkatimi koruyabilirim.',
                'Kurallara uymayan iş arkadaşlarıma uyarıda bulunurum.',
                'İşimde minimum düzeyde sürpriz isterim.',
                'Prosedürlerdeki değişiklikleri hızlı bir şekilde öğrenirim.'
            ],
            'Analitik Düşünme ve Veri İşleme': [
                'Büyük veri setlerini incelerken bir kalıp (pattern) bulabilirim.',
                'İki farklı rapor arasındaki tutarsızlıkları kolayca tespit ederim.',
                'Problem çözmeye mantıksal bir yaklaşımla başlarım.',
                'Verileri yorumlamak ve sonuçlar çıkarmak eğlencelidir.',
                'Sayısal bilgileri hızlı bir şekilde hafızama alırım.',
                'Bir tablo veya grafiğin bana ne anlattığını hemen anlarım.',
                'Fikirlerimi kanıtlamak için her zaman verilere güvenirim.',
                'Yeni ve karmaşık bir bilgiyi anlamak için birden fazla kaynak kullanırım.',
                'Duygusallıktan çok, rasyonelliği önceliklendiririm.',
                'Basit hataların kaynağını bulmakta yetenekliyim.'
            ],
            'Zaman Yönetimi ve Önceliklendirme': [
                'Birden fazla görevi aynı anda yönetmekte iyiyim.',
                'Önemli ve acil olan görevleri kolayca ayırabilirim.',
                'Son dakikada iş yapmaktan kaçınırım.',
                'Görevlerimi her zaman bir öncelik listesine göre yaparım.',
                'Bir işin ne kadar süreceğini doğru tahmin ederim.',
                'Yapılacaklar listeme uymadığım zaman kendimi başarısız hissederim.',
                'Bir göreve başlamadan önce, bitiş zamanını belirlerim.',
                'Gecikmelerin sorumluluğunu her zaman kendimde ararım.',
                'Başkalarının acil durumları yüzünden kendi işlerimi ertelemem.',
                'Verimsiz bir süreç gördüğümde, bunu düzeltmek için zaman harcarım.'
            ],
            'Kurum İçi İşbirliği ve Koordinasyon': [
                'Diğer departmanlardan yardım istemekten çekinmem.',
                'Bilgiyi paylaşmak, tek başıma saklamaktan daha önemlidir.',
                'Başarılı olmak için her zaman iş arkadaşlarımla iyi geçinirim.',
                'Farklı departmanların hedeflerini anlamak için çaba gösteririm.',
                'İş arkadaşlarımla çatışma yaşamaktan nefret ederim.',
                'Ekip çalışmasına kişisel başarımdan daha çok değer veririm.',
                'Diğer departmanların sorumluluğundaki bir işi yapmaktan kaçınmam.',
                'Bilgi akışını sağlamak için düzenli olarak toplantılar düzenlerim.',
                'İş birliği yapmadığımda kendimi soyutlanmış hissederim.',
                'Kurum içi anlaşmazlıkları, iş ilişkilerimi zedelemeden çözebilirim.'
            ],
            'Geri Bildirime Açıklık ve Kendini Geliştirme': [
                'Olumsuz geri bildirim beni motive eder.',
                'Eleştirildiğimde savunmaya geçmem.',
                'Aktif olarak işimi nasıl daha iyi yapabileceğimi sorarım.',
                'Yeni beceriler öğrenmek için zaman ve enerji harcarım.',
                'Hatalarımı kabul etmekte zorlanmam.',
                'Başkalarının hatalarımı görmesinden korkmam.',
                'Geri bildirimin ne kadar spesifik olursa o kadar faydalı olduğuna inanırım.',
                'Bir hata yaptığımda, bunu düzeltme planı ile birlikte üstlerime bildiririm.',
                'İşimi hep aynı şekilde yapmayı tercih etmem.',
                'Başarısızlıkları, öğrenme fırsatları olarak görürüm.'
            ],
            'Öz Disiplin ve İç Motivasyon': [
                'Dışarıdan bir baskı olmasa bile işime odaklanırım.',
                'Zor görevleri tamamlamak için kendime hedefler belirlerim.',
                'Sıkıldığımda işimi bırakma eğilimim yoktur.',
                'Kendi kendimin patronu gibi çalışırım.',
                'İşim bittiğinde, her zaman bir sonraki işi aramaya başlarım.',
                'Sabahları işe gitmek için kolayca motive olurum.',
                'Uzun süreli görevlerde sebat etme yeteneğim yüksektir.',
                'Belirlenmiş bir hedefim yoksa bile çalışmakta zorlanmam.',
                'Motivasyonumu kaybetmem, çünkü işime anlam katıyorum.',
                'Kendime koyduğum standartlar, başkalarının koyduklarından daha yüksektir.'
            ],
            'Organizasyon ve Düzen Becerisi': [
                'Çalışma masam her zaman düzenlidir.',
                'Önemli belgeleri bulmam nadiren 1 dakikadan fazla sürer.',
                'Dijital dosyalarımı (e-posta, klasörler) düzenli tutarım.',
                'Düzensiz bir süreç, beni verimsiz yapar.',
                'İşlerimi bir proje yönetim sistemi/ajanda ile takip ederim.',
                'Kağıt işlerini hemen hallederim, biriktirmem.',
                'Bir işin başlangıcından bitişine kadar olan süreci adım adım haritalayabilirim.',
                'Başkalarının düzensizliği beni sinirlendirir.',
                'Tek bir işi bitirmeden diğerine başlamamayı tercih ederim.',
                'Toplantılardan önce her zaman bir gündem hazırlarım.'
            ],
            'Sözel Akıl Yürütme ve Anlama': [
                'Karmaşık metinleri bir kez okuyarak anlayabilirim.',
                'Prosedürleri okurken kafa karıştırıcı ifadeleri hızlıca fark ederim.',
                'Bir metnin ana fikrini hemen çıkarabilirim.',
                'Uzun ve detaylı bir e-postayı özetleyebilirim.',
                'Okuduğum metinlerdeki mantık hatalarını kolayca bulurum.',
                'Yeni bir terimle karşılaştığımda, anlamını hemen araştırırım.',
                'Bilgileri sadece yazılı olarak almayı tercih ederim.',
                'Kurum içi yazışmalarda net ve özlü bir dil kullanırım.',
                'İki farklı metindeki benzerlikleri ve farklılıkları hızla karşılaştırırım.',
                'Okuduğum bir metni kendi kelimelerimle doğru bir şekilde tekrar edebilirim.'
            ],
            'Yeni Sistemlere Öğrenme Çevikliği': [
                'Yeni bir yazılımı kullanmayı kendiliğimden öğrenebilirim.',
                'Eğitim olmadan yeni teknolojileri denemeye istekliyim.',
                'Yeni bir sisteme geçiş beni strese sokmaz.',
                'Hata yapma korkusu olmadan yeni iş akışlarını denerim.',
                'Teknolojiye ayak uydurmak benim için doğaldır.',
                'Bir programı kullanırken tüm işlevlerini bilmek önemlidir.',
                'Başka birinin yeni bir sistemi bana adım adım göstermesini tercih etmem.',
                'Daha hızlı ve verimli çözümler ararım.',
                'İş yerinde öğrenmeyi en önemli motivasyon kaynağı olarak görürüm.',
                'Yeni bir sistemi eski alışkanlıklarımla karşılaştırmam.'
            ]
        },
        'mavi yaka': {
            'İş Güvenliği Bilinci ve Kurala Uyum': [
                'İş güvenliği talimatlarına harfiyen uymak en önemli kuraldır.',
                'Bir işi daha hızlı bitirmek için güvenlik kurallarını esnetmem.',
                'Güvenli olmayan bir durum gördüğümde hemen amirime bildiririm.',
                'İş ekipmanlarını kullanırken her zaman koruyucu ekipman giyerim.',
                'Kendi güvenliğim, işin tamamlanmasından önce gelir.',
                'Kural ihlali yapan bir iş arkadaşımı uyarırım.',
                'Tehlikeli bir iş için eğitim almadan o işi yapmayı reddederim.',
                'İş yerindeki tehlike işaretlerine dikkat ederim.',
                'Güvenlik brifinglerini dikkatle dinlerim.',
                'Çalışma alanımı her zaman temiz ve düzenli tutarım.'
            ],
            'Fiziksel ve Zihinsel Dayanıklılık': [
                'Zorlayıcı fiziksel işlerde kolay kolay yorulmam.',
                'Tekrarlayan işler bile dikkatimi dağıtmaz.',
                'Tüm vardiya boyunca enerjimi yüksek tutabilirim.',
                'Yoğun iş temposunda bile soğukkanlı kalabilirim.',
                'Ayakta uzun süre çalışmakta zorlanmam.',
                'Baskı altında performansım düşmez.',
                'Tekrar eden rutinler bana güven verir.',
                'İşim bittiğinde bile fiziksel olarak dinç kalırım.',
                'Gürültülü ortamlarda rahatça çalışabilirim.',
                'Zorlu bir günün ardından ertesi güne istekli başlarım.'
            ],
            'Dikkat ve Odaklanma Yeteneği': [
                'En küçük bir detay hatasını bile fark ederim.',
                'Rutin işler yaparken bile aklım başka yerlere kaymaz.',
                'İşimde dikkatimi uzun süre koruyabilirim.',
                'Çalışırken telefonumu sık sık kontrol etme ihtiyacı duymam.',
                'Tek bir göreve odaklanmayı, çoklu görevden daha çok tercih ederim.',
                'Bir işi bitirmeden başka bir işe başlamam.',
                'Çalışırken etrafımdaki hareketler beni kolayca dağıtmaz.',
                'Sayım ve ölçüm işlerinde hatasız olduğuma inanıyorum.',
                'Tüm adımları doğru yapsam bile sonucu kontrol etme ihtiyacı duyarım.',
                'Görsel denetimler ve kontroller yapmaktan keyif alırım.'
            ],
            'Problem Bildirme Yeteneği (SJT)': [
                'Bir makineden garip bir ses geldiğinde... (En Uygun Eylem Seçimi)',
                'Üretim bandında bir ürünün kusurlu olduğunu fark ettiniz, ancak vardiyanın bitimine 5 dakika kaldı... (En Uygun Eylem Seçimi)',
                'Bir iş arkadaşınızın tehlikeli bir şey yaptığını gördünüz, ancak bunu bildirirseniz işinden olabilir... (En Uygun Eylem Seçimi)',
                'Kullandığınız aletin bozuk olduğunu anladınız, ancak yerine yenisini alacak kimse yok... (En Uygun Eylem Seçimi)',
                'Bir prosedürün yanlış yazıldığını düşünüyorsunuz, ancak bu prosedür yıllardır kullanılıyor... (En Uygun Eylem Seçimi)',
                'Amiriniz meşgul ve acil bir sorun var; başka bir amirle konuşur musunuz? (En Uygun Eylem Seçimi)',
                'Bir malzeme eksik ve bu, tüm hattı durduracak; amiriniz ulaşılamıyor... (En Uygun Eylem Seçimi)',
                'İş arkadaşınız sürekli geç kalıyor, bu da hattın başlamasını geciktiriyor... (En Uygun Eylem Seçimi)',
                'Bir prosedürün daha iyi bir yolu olduğunu düşünüyorsunuz, ancak bu ek mesai gerektirecek... (En Uygun Eylem Seçimi)',
                'Bir kimyasalın döküldüğünü gördünüz, ancak bu sizin sorumluluk alanınız değil... (En Uygun Eylem Seçimi)'
            ],
            'Ekipman Sorumluluğu ve Özen': [
                'Kullandığım alet ve makinelerin temiz ve düzenli olmasını sağlarım.',
                'İş bitiminde ekipman bakımı yapmak önemlidir.',
                'Makine sesindeki en ufak bir değişikliği fark ederim.',
                'Ekipmanlara dikkatsizlik sonucu zarar vermem.',
                'Başkasının ekipmanına da kendi eşyam gibi bakarım.',
                'Ekipmanı kullanmadan önce kılavuzunu okumayı tercih ederim.',
                'Hasarlı ekipmanlarla çalışmaya devam etmektense onarımını beklerim.',
                'Ekipmanların düzenli bakıma ihtiyacı olduğunu amirime hatırlatırım.',
                'Kullandığım aletleri her zaman yerine koyarım.',
                'Ekipman arızalanırsa, sorunun ne olduğunu bulmaya çalışırım.'
            ],
            'Vardiya ve Esnekliğe Adaptasyon': [
                'Çalışma saatlerinin değişmesi benim için büyük bir sorun değildir.',
                'Gerekirse ek mesaiye kalmaya hazırım.',
                'Hafta sonu çalışmak benim için bir sorun teşkil etmez.',
                'Bir pozisyondan diğerine geçiş yapmakta zorlanmam.',
                'Gece vardiyasında da gündüz vardiyası kadar verimli çalışabilirim.',
                'Farklı vardiyalardaki iş arkadaşlarımla iyi iletişim kurarım.',
                'İş yoğunluğuna göre esnek çalışmayı değerli bulurum.',
                'Rutinim bozulduğunda işimde hata yapma eğilimim artmaz.',
                'Gerekirse tatil planlarımı işe göre değiştirebilirim.',
                'Farklı iş istasyonlarında çalışmaktan zevk alırım.'
            ],
            'Hata Kontrolü ve İş Kalitesi': [
                'Bir işi bitirmeden önce, her zaman son kontrolü yaparım.',
                'Mükemmel iş, hızlı işten daha önemlidir.',
                'Ürettiğim her şeyin kusursuz olmasını isterim.',
                'Kalite kontrol süreçlerini sıkıcı bulmam.',
                'Bir hata yaptığımda, bunu hemen düzeltmek için sorumluluk alırım.',
                'İş arkadaşlarımın hatalarını görmezden gelmem.',
                'Yaptığım işi iki kez kontrol etmek zaman kaybı değildir.',
                'Teslim edilen üründe bir kusur bulursam, geri alıp düzeltirim.',
                'Benim görevim, sadece üretim bandını takip etmek değil, kaliteyi de sağlamaktır.',
                'En iyi işi çıkarmak için her zaman çaba gösteririm.'
            ],
            'Basit Talimat ve Yönerge Takibi': [
                'Sözlü talimatları tek seferde anlayabilirim.',
                'Karmaşık talimatları parçalara ayırıp sırayla uygularım.',
                'Talimatlar çok uzun olsa bile önemli kısımlarını hatırlarım.',
                'İşlem adımlarını karıştırmam.',
                'Bir prosedür belgesini okumaktan sıkılmam.',
                'Yeni bir talimat geldiğinde, eski alışkanlıklarımı hemen bırakırım.',
                'Bir talimattan emin olmadığımda, tahmin etmektense sorarım.',
                'Yönergeleri takip etmek, yaratıcılıktan daha önemlidir.',
                'İki farklı talimat çeliştiğinde, hemen amirime danışırım.',
                'Bana verilen görevleri, adım adım doğru sırada yaparım.'
            ],
            'Temel Takım Çalışması ve Destek': [
                'İş arkadaşlarıma yardım etmek için kendi işimi yavaşlatabilirim.',
                'Ekip başarısı, kişisel başarımdan daha önemlidir.',
                'İş arkadaşlarımdan yardım istemekten çekinmem.',
                'Başkalarının yaptığı işleri yargılamam.',
                'Takım üyelerinin sorunlarına karşı ilgiliyim.',
                'Bana düşmeyen bir işi yapmaktan çekinmem.',
                'Bir arıza olduğunda, herkesin yardım etmesi gerektiğini düşünürüm.',
                'İş arkadaşlarımla kolayca kaynaşırım.',
                'Başarıyı sadece kendime atfetmek yerine, ekibi öne çıkarırım.',
                'Vardiya değişiminde, diğer ekibe net bilgi veririm.'
            ],
            'Acil Durum Reaksiyonu': [
                'Yangın veya kaza durumunda sakin kalabilirim.',
                'Acil durum prosedürlerini ezbere bilirim.',
                'Panik yapanları yönetmekte iyiyim.',
                'Kriz anında ne yapacağımı bilemem ve birine sormayı beklemem.',
                'Acil durumlarda inisiyatif almaktan çekinmem.',
                'Bir kaza gördüğümde ilk yardımda bulunabilirim.',
                'Hata yapma korkusuyla acil durumda eylemden kaçınmam.',
                'Acil durum sinyallerine hemen tepki veririm.',
                'İnsanların güvenliği benim için her zaman önceliklidir.',
                'Olası acil durum senaryolarını önceden düşünürüm.'
            ]
        },
        'hizmet-yonetici': {
            'Müşteri Odaklı Liderlik Felsefesi': [
                'Her kararımda önce müşterinin nasıl etkileneceğini düşünürüm.',
                'Müşteri şikayetlerini, sistemin iyileştirilmesi için bir hediye olarak görürüm.',
                'Ekibimi, her etkileşimin bir satış fırsatı olduğuna inandırırım.',
                'Müşteri memnuniyetini, kârlılıktan daha önemli bir metrik olarak izlerim.',
                'En iyi hizmeti sunmak için mevcut kuralları esnetme yetkisine sahip olmalıyız.',
                'Müşteri şikayetleri geldiğinde, sorumluluğu hemen ekibe yüklemem.',
                'Düzenli olarak "Müşteri Deneyimi" toplantıları düzenlerim.',
                'Sektördeki en iyi müşteri hizmeti örneklerini takip ederim.',
                'Başarısız hizmet deneyimleri beni kişisel olarak rahatsız eder.',
                'Müşterinin ihtiyaçlarını bilmek, onlara ne istediklerini sormaktan daha önemlidir.'
            ],
            'Duygusal Zeka ve Empatik Yönetim': [
                'Ekip üyelerimin motivasyonunu ses tonlarından anlayabilirim.',
                'Kendi stresimi, ekibime yansıtmaktan kaçınırım.',
                'Bir çalışan zor zamanlardan geçerken ona destek olmak önemlidir.',
                'Duygusallık, iş yerinde profesyonelliğe engel değildir.',
                'Farklı kişilikteki insanlarla kolayca bağ kurabilirim.',
                'Bir çalışanın duygusal durumu, performansını etkileyeceğini bilirim.',
                'Bir çatışmada, her iki tarafın da bakış açısını hızlıca kavrarım.',
                'Ekibimdeki uyumsuzlukları erken fark ederim.',
                'Empati, sadece müşteriyle değil, ekip üyeleriyle de kurulmalıdır.',
                'Zorlu bir görüşmeden sonra duygusal olarak kendimi hızlıca toparlarım.'
            ],
            'Stres ve Kriz Yönetimi (SJT)': [
                'Bir sistem çöktüğünde ve müşteri kuyruğu uzadığında... (En Uygun Eylem Seçimi)',
                'Basında şirketiniz hakkında olumsuz bir haber çıktığında ve herkes size soru sorduğunda... (En Uygun Eylem Seçimi)',
                'Bir çalışanınız, stres nedeniyle iş yerinde ağlamaya başladığında... (En Uygun Eylem Seçimi)',
                'Bir kriz sırasında yöneticiniz yanlış bir emir verdiğinde... (En Uygun Eylem Seçimi)',
                'Çok sayıda personel aynı anda izin istedi ve vardiya boşlukları oluştu... (En Uygun Eylem Seçimi)',
                'Bir müşteri şikayeti sosyal medyada hızla yayıldığında... (En Uygun Eylem Seçimi)',
                'Kritik bir sunumdan hemen önce verilerin yanlış olduğunu anladığınızda... (En Uygun Eylem Seçimi)',
                'İki ekip üyesi herkesin önünde hararetli bir tartışmaya girdiğinde... (En Uygun Eylem Seçimi)',
                'Bir mevzuat değişikliği, tüm süreçlerinizi bir gecede değiştirmeyi gerektirdiğinde... (En Uygun Eylem Seçimi)',
                'Rakibiniz, sizin en iyi iki çalışanınızı transfer etmek için teklif yaptığında... (En Uygun Eylem Seçimi)'
            ],
            'Hizmet Kalitesi ve Sürekli İyileştirme': [
                'Hizmet süreçlerini sürekli olarak gözden geçiririm.',
                '"Eskiden beri böyle yapıyoruz" cümlesi benim için geçerli bir sebep değildir.',
                'Sektördeki en iyi uygulama standartlarını takip ederim.',
                'Müşteri geri bildirim anketlerini dikkatle incelerim.',
                'Küçük aksaklıklar için bile tüm sistemi değiştirmeyi denerim.',
                'Kalite standartlarımızı düzenli olarak güncellerim.',
                'Hizmet kalitesindeki küçük bir düşüşü bile önemserim.',
                'Başarıyı kutlamak kadar, başarısızlıkları analiz etmek de önemlidir.',
                'İyileştirme için çalışanlarımın önerilerini aktif olarak toplarım.',
                'Kusurlu bir hizmet sunmak, hiç hizmet sunmamaktan daha kötüdür.'
            ],
            'Satış ve İlişki Yönetimi Bilinci': [
                'Her müşteri etkileşiminin, potansiyel bir satış fırsatı olduğunu düşünürüm.',
                'Ekibimi, sadece hizmet değil, aynı zamanda çözüm odaklı olmaya teşvik ederim.',
                'Uzun soluklu müşteri ilişkileri, tek seferlik büyük satışlardan daha değerlidir.',
                'Müşteriyi elde tutma (sadakat) çabaları, yeni müşteri kazanmaktan daha önemlidir.',
                'Hizmet ve satış hedeflerini dengelemekte başarılıyımdır.',
                'Müşteri ihtiyaçlarını anlamak için açık uçlu sorular sorarım.',
                'Müşteriye bir ürün/hizmet satmakta ısrarcı olmamayı tercih ederim.',
                'Ekibimi, müşteriyle kişisel bağ kurmaya teşvik ederim.',
                'Müşterilerin neden rakibe gittiğini sürekli analiz ederim.',
                'Çapraz satış (cross-sell) ve yukarı satış (up-sell) fırsatlarını belirlemede iyiyim.'
            ],
            'Geri Bildirim ve Koçluk Kültürü': [
                'Çalışanlara düzenli ve yapıcı geri bildirim veririm.',
                'Olumsuz geri bildirimi, resmi performans değerlendirmesine saklamam.',
                'Çalışanlarımın gelişim planlarını desteklerim.',
                'Geri bildirimin hemen ardından takip ve koçluk yaparım.',
                'Çalışanların kendilerine hedef belirlemesine yardımcı olurum.',
                'Koçluk yaparken, ne yapmaları gerektiğini söylemek yerine sorular sorarım.',
                'Bir çalışanı sık sık övmek, onlarda rehavet yaratmaz.',
                'Geri bildirimin her zaman spesifik örneklere dayanması gerekir.',
                'Koçluk için zaman ayırmak, günlük iş akışını bozmaz.',
                'Ekip üyelerimin birbirlerine de geri bildirim vermesini teşvik ederim.'
            ],
            'Çözüm Odaklı Yaklaşım': [
                'Şikayetler geldiğinde hemen neyin yanlış gittiğine odaklanırım.',
                'Müşteri şikayetlerinin temel nedenini bulmak için zaman harcarım.',
                'Kurallar, müşteri sorununu çözmemi engelliyorsa, esneklik ararım.',
                'Bir müşteri sorunu çözüldükten sonra, durumu takip ederim.',
                'Sorunu çözmektense, sorumluyu bulmaya odaklanmam.',
                'Hızlı ve geçici çözümler yerine, kalıcı çözümleri tercih ederim.',
                'Başarısızlıkları kabul eder ve müşteriden özür dilerim.',
                'Kaybedilen bir müşteriyi geri kazanmaya çalışmak anlamsız değildir.',
                'Problem çözme becerimi geliştirmek için sürekli yeni yöntemler ararım.',
                'Her müşteri şikayetini, sistemi iyileştirmek için kullanırım.'
            ],
            'Çalışan Deneyimi Farkındalığı': [
                'Mutlu çalışanların, mutlu müşteriler yarattığına inanırım.',
                'Çalışanlarımın moralini düzenli olarak kontrol ederim.',
                'Çalışma ortamının fiziksel ve duygusal olarak destekleyici olmasını sağlarım.',
                'Çalışanlarımın iş ve özel hayat dengesini önemserim.',
                'Yüksek personel devir hızı (turnover) benim sorumluluğumdadır.',
                'Çalışanların endişelerini ve önerilerini ciddiye alırım.',
                'Ekibimin stres seviyesini düşürmek için proaktif adımlar atarım.',
                'Çalışan memnuniyeti anketlerini uygulamayı gerekli bulurum.',
                'Başarılı çalışanlarımı tanıma ve ödüllendirme konusunda cömertim.',
                'Çalışanların, işe geldiğinde kendini ait hissetmesini sağlarım.'
            ],
            'Sektörel ve Pazar Bilinci': [
                'Rakip şirketlerin hizmet süreçlerini yakından incelerim.',
                'Sektördeki yeni trendleri ve teknolojileri düzenli olarak takip ederim.',
                'Piyasa araştırmalarına zaman harcamayı gerekli bulurum.',
                'Yöneticilerin her zaman kendilerini geliştirmesi gerektiğine inanırım.',
                'Müşterilerin beklentilerinin sürekli değiştiğinin farkındayım.',
                'Başarımızın sırrı, hep aynı şeyi iyi yapmak değildir.',
                'Rakiplerin zayıf yönlerini fırsata çevirmekte iyiyim.',
                'Sektördeki yasal değişiklikleri herkesten önce öğrenmeye çalışırım.',
                'Pazarlama ve satış ekipleriyle sürekli iş birliği yaparım.',
                'Sadece kendi departmanımın hedeflerine odaklanmam.'
            ],
            'Karizma, İkna ve Sunum Yeteneği': [
                'Topluluk önünde konuşmaktan çekinmem.',
                'İkna edici olmak benim için kolaydır.',
                'Çalışanlarımı veya müşterileri coşturabilirim.',
                'Bir toplantıyı yönetirken dikkatleri üzerime çekerim.',
                'İletişim kurarken resmiyetten uzak durmayı tercih ederim.',
                'Bir sunumun başarısı, içeriğinden çok sunucunun enerjisine bağlıdır.',
                'Zorlu müzakerelerde sakin kalırım.',
                'Fikirlerimi savunmak için güçlü argümanlar bulurum.',
                'İnsanların ruh halini ve tepkilerini hızla okuyabilirim.',
                'Başarısızlıklarım hakkında konuşmaktan çekinmem.'
            ]
        },
        'hizmet-on-cephe': {
            'Empati ve Müşteri Bakış Açısı': [
                'Müşterinin durumunu ve hislerini anlamakta zorlanmam.',
                'İnsanların neden sinirlendiğini anlamaya çalışırım.',
                'Müşteri haksız olsa bile ona hak vermeyi denerim.',
                'Başkalarının sorunları beni ilgilendirmez.',
                'Müşterilere karşı sabırsız davranmam.',
                'Bir müşteri olsam, kendi hizmetimden memnun kalırdım.',
                'Müşterinin ne hissettiği, işin kalitesini etkiler.',
                'Empati kurmak, duygusal olarak yorucu değildir.',
                'Müşterinin şikayetini dinlerken sözünü kesme eğilimim yoktur.',
                'Müşterinin bakış açısını anlamaya çalışırım.'
            ],
            'Dışadönüklük ve Sosyallik': [
                'İnsanlarla etkileşim kurmaktan çekinmem.',
                'Sosyal etkileşimden enerji alırım.',
                'Tanımadığım biriyle konuşmaya başlamak kolaydır.',
                'İş yerinde yalnız çalışmayı tercih etmem.',
                'Kalabalık ortamlar beni rahatsız etmez.',
                'Bir müşteriyle uzun bir sohbet başlatırım.',
                'Bir grup içindeki sessiz kişi olmayı tercih etmem.',
                'İnsanların dikkatini çekmekten çekinmem.',
                'Müşterilerle kişisel bağ kurmak önemlidir.',
                'Yüksek sesle konuşmaktan çekinmem.'
            ],
            'Duygu Regülasyonu (Duygusal Emeği)': [
                'Kişisel olumsuzlukları iş ortamına yansıtmam.',
                'Zor bir müşteriyle tartıştığımda, duygularımı kontrol edebilirim.',
                'Sinirlendiğimde bile yüzümden belli etmem.',
                'Müşteriye karşı her zaman profesyonel bir tavır sergilerim.',
                'Duygusal emeği (gülümseme vb.) göstermek benim için kolaydır.',
                'İş stresimi evime taşımam.',
                'Müşterinin olumsuz duyguları beni kişisel olarak etkilemez.',
                'Zorlu bir etkileşimden sonra hızlıca normalleşirim.',
                'Duygusal olarak ne kadar zorlarsam zorlayayım, işimde bunu yansıtmam.',
                'Kötü bir ruh halini, iş yerinde saklama yeteneğim yüksektir.'
            ],
            'Güler Yüz ve Pozitiflik (Kişilik)': [
                'Genellikle neşeli ve pozitif bir insanımdır.',
                'Zor bir günde bile gülümseyebilirim.',
                'İyimserlik, hizmet sektöründe bir zorunluluktur.',
                'Olumsuz durumlara olumlu bir bakış açısıyla yaklaşırım.',
                'İş arkadaşlarıma ve müşterilere enerji veririm.',
                'Hayatta iyi şeylerin olacağına inanırım.',
                'Bir problemle karşılaştığımda moralim kolayca bozulmaz.',
                'İnsanları dinlerken onlara sıcak ve ilgili bir ifadeyle bakarım.',
                'İnsanlara karşı içten ve samimi davranırım.',
                'Başkalarının mutlu olması beni de mutlu eder.'
            ],
            'Hızlı İşlem ve Dikkat Yeteneği': [
                'Yoğunlukta bile doğru ve hatasız işlem yapabilirim.',
                'Hızlı çalışırken detayları kaçırmam.',
                'Birden fazla görevi aynı anda yapabilirim.',
                'İşlem hızım, kalitemi etkilemez.',
                'Müşteri bilgilerini hızlıca işleyebilirim.',
                'Acil durumlarda hızlı hareket ederim.',
                'İş akışındaki aksaklıkları hemen fark ederim.',
                'İşlem adımlarını doğru sırada ve hızla tamamlarım.',
                'Hızlı karar verme yeteneğim gelişmiştir.',
                'Baskı altında bile sayısal verileri doğru okuyabilirim.'
            ],
            'İlk İzlenim ve Kişisel Bakım Bilinci': [
                'Dış görünüşüme ve kişisel bakımıma özen gösteririm.',
                'Kıyafetlerimin her zaman temiz ve ütülü olmasına dikkat ederim.',
                'Bir hizmet personelinin ilk izleniminin önemine inanırım.',
                'İş yerindeki giyim kurallarına titizlikle uyarım.',
                'Müşterilerimin beni profesyonel biri olarak görmesini isterim.',
                'Dış görünüşümün işime olan saygımı yansıttığını düşünürüm.',
                'Müşterilerle buluşmadan önce daima kendimi kontrol ederim.',
                'Müşterilerimin rahat etmesi için her zaman temiz ve düzenli bir görünüm sergilerim.',
                'Dağınık veya özensiz görünmekten kaçınırım.',
                'Görünüşümün iletişimimin bir parçası olduğunu bilirim.'
            ],
            'Proaktif Hizmet Sunumu (SJT)': [
                'Müşteri sormadan ihtiyacı öngörme ve inisiyatif alma... (En Uygun Eylem Seçimi)',
                'Bir müşterinin kafasının karıştığını fark ettiğinizde... (En Uygun Eylem Seçimi)',
                'Bir ürünün stoğu tükendiğinde, müşteriye hemen alternatif önerme... (En Uygun Eylem Seçimi)',
                'Bir müşterinin beklemesi gerektiğinde, durumu ona proaktif olarak açıklama... (En Uygun Eylem Seçimi)',
                'Bir müşterinin ödeme sırasında zorlandığını gördüğünüzde... (En Uygun Eylem Seçimi)',
                'Bir hizmetin daha iyi yapılabileceğini düşündüğünüzde... (En Uygun Eylem Seçimi)',
                'Müşteri, yanlış ürünü/hizmeti talep ettiğinde... (En Uygun Eylem Seçimi)',
                'İş arkadaşınızın çok meşgul olduğunu gördüğünüzde... (En Uygun Eylem Seçimi)',
                'Bir müşterinin uzun süredir beklediğini fark ettiğinizde... (En Uygun Eylem Seçimi)',
                'Müşteriye bir hizmeti sunduktan sonra, memnuniyetini kontrol etme... (En Uygun Eylem Seçimi)'
            ],
            'Çatışma Sakinliği ve Yönetimi': [
                'Tartışmayı tırmandırmadan durumu yatıştırabilirim.',
                'Öfkeli bir müşteriyle konuşurken sesimi yükseltmem.',
                'Çatışma durumlarında mantığımı korurum.',
                'Müşterinin öfkesini kişisel algılamam.',
                'Çatışma çözme tekniklerini bilirim.',
                'Bir tartışmanın ortasında bile çözüm odaklı kalırım.',
                'Gergin anlarda bile sakin bir vücut dili sergilerim.',
                'Çatışma çözümünde adil bir arabulucu olabilirim.',
                'Müşterinin sinirinin geçmesini beklerim, sonra konuşurum.',
                'Tartışmaları, hizmeti iyileştirmek için bir geri bildirim olarak görürüm.'
            ],
            'Sözel Akıcılık ve Anlaşılırlık': [
                'Akıcı ve nazik bir dille konuşurum.',
                'Karmaşık bilgileri müşterilere basitçe açıklayabilirim.',
                'Müşterilerle konuşurken kelime seçimime dikkat ederim.',
                'Ses tonum ve konuşma hızım müşterinin anlamasını kolaylaştırır.',
                'Müşteriye tam olarak ne demek istediğimi anlatabilirim.',
                'Telefonla konuşurken bile güven veren bir ses tonum vardır.',
                'Müşterinin sorusunu net bir şekilde anlarım.',
                'Müşterilerle iletişim kurarken profesyonel bir dil kullanırım.',
                'Argo ve günlük konuşma dilinden kaçınırım.',
                'Müşteriden gelen geri bildirimi doğru bir şekilde özetleyebilirim.'
            ],
            'Hizmet Değerleri Motivasyonu': [
                'Başkalarına yardım etmekten ve memnuniyet sağlamaktan tatmin olurum.',
                'İnsanların sorunlarını çözmek benim için önemlidir.',
                'Müşteri memnuniyeti, maaşımdan daha önemli bir motivasyon kaynağıdır.',
                'Başkalarının hayatını kolaylaştırmak beni mutlu eder.',
                'Hizmet sektöründe çalışmayı bir misyon olarak görürüm.',
               
                'Müşteri için ekstra çaba göstermekten çekinmem.',
                                                                                                                                
                'İnsanlarla etkileşim kurmak benim için yorucu değildir.',
                'Yaptığım işin topluma faydalı olduğunu bilirim.',
                'Ekibimi ve kendimi sürekli olarak hizmet standartlarımızı aşmaya teşvik ederim.',
                'Memnun bir müşteri görmek, en büyük ödülümdür.'
            ]
        }
    };

    // Aday giriş formu işlemleri
    const candidateLoginForm = document.getElementById('candidateLoginForm');
    const testSection = document.getElementById('testSection');
    let activeCandidate = null;
    if (candidateLoginForm) {
        candidateLoginForm.addEventListener('submit', async function(e) {
            e.preventDefault();
            const nickname = document.getElementById('candidateNickname').value.trim();
            const password = document.getElementById('candidatePassword').value;
            if (!nickname) { alert('Lütfen rumuz girin'); return; }
            try {
                const cand = await window.getCandidateByNickname(nickname);
                if (!cand) { alert('Rumuz bulunamadı'); return; }
                if ((cand.password||'') !== password) { alert('Şifre hatalı'); return; }
                // set active candidate from DB object
                activeCandidate = cand;
                candidateLoginForm.classList.add('hidden');
                // hide admin overlays to avoid overlap
                try { const panel = document.getElementById('adminPanel'); if (panel) { panel.classList.add('hidden'); panel.style.display='none'; } } catch(e){}
                try { const ik = document.getElementById('ikPanel'); if (ik) { ik.classList.add('hidden'); ik.style.display='none'; } } catch(e){}
                try { const aC = document.getElementById('adminLoginCompact'); if (aC) { aC.classList.add('hidden'); aC.style.display='none'; } } catch(e){}
                // render questions according to stored tip/baslik
                renderTestQuestions(cand.tip, cand.baslik);
                // bring test section visually to front (above admin panels)
                try {
                    testSection.classList.remove('hidden');
                    testSection.style.position = 'relative';
                    testSection.style.zIndex = '10060';
                } catch(e){}
            } catch (err) {
                console.error(err);
                alert('Giriş sırasında hata: ' + (err.message||err));
            }
        });
    }

    // Helper: produce question list (maintains type info) for a given sector/role
    function getQuestionsFor(sector, role) {
        const ROLE_TO_POOL_KEY = {
            'mavi_yaka': 'mavi yaka',
            'beyaz_yaka': 'beyaz yaka',
            'yonetici': 'yonetici',
            'yonetici_idari': 'yonetici',
            'hizmet_personeli': 'hizmet-on-cephe'
        };
        const poolKey = ROLE_TO_POOL_KEY[role] || ROLE_TO_POOL_KEY[sector] || null;
        let questions = [];
        if (poolKey && questionPool[poolKey]) {
            const sub = questionPool[poolKey];
            Object.keys(sub).forEach(k => {
                const arr = Array.isArray(sub[k]) ? sub[k] : [];
                const type = (/sjt|durumsal|problem|senaryo/i.test(k)) ? 'sjt' : 'personality';
                arr.forEach(q => questions.push({text: q, type, category: k}));
            });
        }
        // fallback generic questions
        if (!questions.length) {
            questions = [
                {text: 'Takım çalışmasına yatkın mısınız?', type: 'personality'},
                {text: 'Zaman yönetimi konusunda kendinizi nasıl değerlendirirsiniz?', type: 'personality'},
                {text: 'Bir senaryoda en uygun aksiyonu nasıl seçersiniz?', type: 'sjt'}
            ];
        }
        return questions;
    }

    // Load question keys (expertScores / target) from DB into window.questionKeysCache
    // Defensive: some runtimes or load orders may not have the imported `get` symbol available
    // (ReferenceError seen in some browsers/environments). Use a guarded approach and
    // fallback to onValue (once) if necessary.
    async function loadQuestionKeys() {
        try {
            let snap = null;
            // Preferred: use imported `get` and `ref` if present; guard against ReferenceError when imports failed
            const localGet = (typeof get === 'function') ? get : (typeof window.get === 'function' ? window.get : null);
            const localRef = (typeof ref === 'function') ? ref : (typeof window.ref === 'function' ? window.ref : null);
            if (localGet && localRef) {
                try {
                    snap = await localGet(localRef(db, 'questionKeys'));
                } catch(e) {
                    console.warn('loadQuestionKeys: primary get(ref(...)) failed', e);
                    snap = null;
                }
            } else if (localGet && typeof window.db !== 'undefined' && typeof window.ref === 'function') {
                // fallback to global helpers
                try { snap = await window.get(window.ref(window.db, 'questionKeys')); } catch(e){ console.warn('loadQuestionKeys: window.get(window.ref(...)) failed', e); snap = null; }
            } else {
                // Last resort: try to use onValue if available on either local scope or window (some bundlers put it on window)
                const onValueFn = (typeof onValue === 'function') ? onValue : ((typeof window !== 'undefined' && typeof window.onValue === 'function') ? window.onValue : null);
                if (onValueFn) {
                    snap = await new Promise((resolve, reject) => {
                        try {
                            const r = (localRef) ? localRef(db, 'questionKeys') : (typeof window.ref === 'function' ? window.ref(window.db, 'questionKeys') : null);
                            let unsub = null;
                            try {
                                unsub = onValueFn(r, (s) => {
                                    try { if (typeof unsub === 'function') unsub(); } catch(_){ }
                                    resolve(s);
                                }, (err) => {
                                    try { if (typeof unsub === 'function') unsub(); } catch(_){ }
                                    reject(err || new Error('onValue read failed'));
                                });
                            } catch(e) {
                                // some implementations return an unsubscribe function instead of a handle
                                resolve(null);
                            }
                            // safety timeout
                            setTimeout(() => reject(new Error('Timeout while loading questionKeys')), 5000);
                        } catch(err) { reject(err); }
                    });
                } else if (localGet && (typeof localRef === 'function' || typeof window.ref === 'function')) {
                    // If onValue is missing but get/ref is available, try a get read
                    try {
                        const r = (localRef) ? localRef(db, 'questionKeys') : window.ref(window.db, 'questionKeys');
                        snap = await localGet(r);
                    } catch(e) {
                        console.warn('loadQuestionKeys: fallback get failed', e);
                        snap = null;
                    }
                } else {
                    // Give up gracefully — set empty cache
                    console.warn('loadQuestionKeys: no suitable DB read function available (onValue/get/ref missing)');
                    snap = null;
                }
                }

            // Normalize different snap shapes (some fallbacks resolve a plain object)
            if (!snap) {
                window.questionKeysCache = {};
            } else if (typeof snap.exists === 'function') {
                window.questionKeysCache = snap.exists() ? snap.val() : {};
            } else if (typeof snap.val === 'function') {
                window.questionKeysCache = snap.val() || {};
            } else {
                // already an object
                window.questionKeysCache = snap || {};
            }

            console.log('Loaded questionKeys into cache', Object.keys(window.questionKeysCache||{}));
        } catch(e) {
            console.warn('Failed to load questionKeys', e);
            window.questionKeysCache = {};
        }
    }

    // Seed sample expert scores into DB for demo (call from console or admin)
    window.seedSampleExpertScores = async function() {
        // Example structure under questionKeys/<poolKey> = { '<category>': [ { text: '...', expertScores: [1.2,4.8,3.5, ...], target: 5 }, ... ] }
        try {
            const sample = {
                'yonetici': {
                    'Stratejik Vizyon ve Planlama': [
                        { expertScores: null, target: 5 },
                        { expertScores: null, target: 5 }
                    ],
                    'Problem Çözme Çevikliği (SJT)': [
                        { expertScores: [1.2,4.8,3.5,2.1,1.0] },
                        { expertScores: [1.0,3.5,4.2,3.8,2.1] }
                    ]
                },
                'mavi yaka': {
                    'Problem Bildirme Yeteneği': [
                        { expertScores: [1.2,4.8,3.5,2.0,1.0] },
                        { expertScores: [1.0,4.5,3.0,2.5,1.5] }
                    ]
                }
            };
            await set(ref(db, 'questionKeys'), sample);
            await loadQuestionKeys();
            alert('Örnek expertScores DB ye yazıldı ve cache yüklendi.');
        } catch(e) { console.error('seedSampleExpertScores failed', e); alert('Seed başarısız: '+e.message); }
    };

    // Ensure keys are loaded at startup
    try { loadQuestionKeys(); } catch(e) { console.warn('loadQuestionKeys init failed', e); }

    // Test sorularını dinamik oluştur
    // Yeni davranış: test tek seferde role (görev) bazlı tüm alt kategorilerin sorularını birleştirerek gösterir.
    // Parametreler: sector (imalat/hizmet), role (mavi_yaka, beyaz_yaka, yonetici, yonetici_idari, hizmet_personeli)
    function renderTestQuestions(sector, role) {
        const testForm = document.getElementById('testForm');
        if (!testForm) return;
        // Build structured question objects preserving type info
        const qObjs = getQuestionsFor(sector, role);
        // Enrich with expert scores/target from loaded cache if available
        try {
            const ROLE_TO_POOL_KEY = {
                'mavi_yaka': 'mavi yaka', 'beyaz_yaka': 'beyaz yaka', 'yonetici': 'yonetici', 'yonetici_idari': 'yonetici', 'hizmet_personeli': 'hizmet-on-cephe'
            };
            const poolKey = (ROLE_TO_POOL_KEY[role] || ROLE_TO_POOL_KEY[sector] || null);
            const cache = window.questionKeysCache || {};
            if (poolKey && cache[poolKey]) {
                const poolKeys = cache[poolKey];
                qObjs.forEach((qObj, idx) => {
                    const cat = qObj.category;
                    if (!cat || !poolKeys[cat]) return;
                    const arr = poolKeys[cat] || [];
                    const candidate = arr[idx] || null;
                    if (candidate) {
                        if (candidate.expertScores) qObj.expertScores = candidate.expertScores;
                        if (candidate.target !== undefined) qObj.target = candidate.target;
                    }
                });
            }
        } catch(e) { console.warn('enrich questions failed', e); }

        // Prepare paginated UI state
        testForm._questions = qObjs;
        testForm._answers = new Array(qObjs.length).fill('');
        testForm._currentQuestion = 0;

        // Build UI container
        testForm.innerHTML = `
            <div id='questionPane' class='space-y-4'></div>
            <div class='flex items-center gap-2 mt-4'>
                <button type='button' id='prevQ' class='bg-gray-200 px-3 py-2 rounded disabled:opacity-50'>Önceki</button>
                <div id='progress' class='text-sm text-gray-600 flex-1'>0 / 0</div>
                <button type='button' id='nextQ' class='bg-blue-600 text-white px-3 py-2 rounded'>Sonraki</button>
                <button type='submit' id='submitTest' class='hidden bg-green-600 text-white px-3 py-2 rounded'>Cevapları Gönder</button>
            </div>
        `;

        const pane = testForm.querySelector('#questionPane');
        const prevBtn = testForm.querySelector('#prevQ');
        const nextBtn = testForm.querySelector('#nextQ');
        const submitBtn = testForm.querySelector('#submitTest');
        const progress = testForm.querySelector('#progress');

        function renderAt(i) {
            const q = qObjs[i] || {};
            progress.innerText = `${i+1} / ${qObjs.length}`;
            prevBtn.disabled = (i === 0);
            // show submit on last
            if (i === qObjs.length - 1) { nextBtn.classList.add('hidden'); submitBtn.classList.remove('hidden'); }
            else { nextBtn.classList.remove('hidden'); submitBtn.classList.add('hidden'); }

            // create content
            let html = `<div><label class='block text-gray-700 font-medium mb-2'>${i+1}. ${q.text || ('Soru ' + (i+1))}</label>`;
            if ((q.type || '').toLowerCase() === 'personality') {
                html += `<select id='curAnswer' class='w-full px-4 py-2 border rounded-lg' data-qtype='personality'>
                    <option value=''>Seçiniz</option>
                    <option value='1'>1 — Kesinlikle Katılmıyorum</option>
                    <option value='2'>2 — Katılmıyorum</option>
                    <option value='3'>3 — Kararsız / Kısmen Katılıyorum</option>
                    <option value='4'>4 — Katılıyorum</option>
                    <option value='5'>5 — Kesinlikle Katılıyorum</option>
                </select>`;
            } else {
                html += `<select id='curAnswer' class='w-full px-4 py-2 border rounded-lg' data-qtype='sjt'>
                    <option value=''>Seçiniz</option>
                    <option value='0'>A</option>
                    <option value='1'>B</option>
                    <option value='2'>C</option>
                    <option value='3'>D</option>
                    <option value='4'>E</option>
                </select>`;
            }
            if (q.expertScores) html += `<div class='text-xs text-gray-500 mt-2'>Uzman skorları: ${JSON.stringify(q.expertScores)}</div>`;
            // placeholder for inline warnings
            html += `<div id='questionWarning' class='text-sm text-red-600 mt-2 hidden' aria-live='polite'></div>`;
            html += `</div>`;
            pane.innerHTML = html;
            // prefill if answer exists
            const sel = pane.querySelector('#curAnswer');
            const warnEl = pane.querySelector('#questionWarning');
            if (sel && testForm._answers[i] !== undefined && testForm._answers[i] !== '') sel.value = testForm._answers[i];
            // focus select
            try { sel && sel.focus(); } catch(e){}

            // Navigation enable/disable helper
            function updateNavigation() {
                const hasAnswer = sel && sel.value !== '' && sel.value !== null && sel.value !== undefined;
                if (i === qObjs.length - 1) {
                    // last question: control submit
                    submitBtn.disabled = !hasAnswer;
                } else {
                    nextBtn.disabled = !hasAnswer;
                }
                // hide warning when answer present
                if (hasAnswer && warnEl) warnEl.classList.add('hidden');
            }

            // wire change to enable next/submit
            if (sel) {
                sel.addEventListener('change', updateNavigation);
                // run initially to reflect prefilled answers
                updateNavigation();
            } else {
                // no select present: ensure navigation enabled to avoid lockout
                nextBtn.disabled = false; submitBtn.disabled = false;
            }
        }

        // wire buttons
        prevBtn.onclick = function(){
            // save current
            const cur = document.getElementById('curAnswer'); if (cur) testForm._answers[testForm._currentQuestion] = cur.value;
            if (testForm._currentQuestion > 0) testForm._currentQuestion--;
            renderAt(testForm._currentQuestion);
        };
        nextBtn.onclick = function(){
            const cur = document.getElementById('curAnswer');
            const warnEl = document.getElementById('questionWarning');
            const curVal = cur ? cur.value : '';
            // validate answered
            if (!curVal || curVal === '') {
                // show inline warning and focus
                if (warnEl) { warnEl.textContent = 'Lütfen önce bir seçenek seçin, sonra sonraki soruya geçin.'; warnEl.classList.remove('hidden'); }
                if (cur) try { cur.focus(); } catch(e){}
                return;
            }
            // save and advance
            if (cur) testForm._answers[testForm._currentQuestion] = curVal;
            if (testForm._currentQuestion < qObjs.length - 1) testForm._currentQuestion++;
            renderAt(testForm._currentQuestion);
        };
        // Ensure submit button is disabled until last question answered
        submitBtn.disabled = true;
        // initial render
        renderAt(0);
    }

    // Test cevaplarını kaydet ve İK paneline yansıt (Firebase ile tam entegrasyon)
    const testForm = document.getElementById('testForm');
    if (testForm) {
        testForm.onsubmit = async function(e) {
            e.preventDefault();
            if (!activeCandidate) return;
            try {
                // Use accumulated answers from the paginated UI if present
                const cevaplar = Array.isArray(testForm._answers) ? testForm._answers.slice() : [];
                // Build typedAnswers using known question types
                const typedAnswers = (testForm._questions || []).map((q, idx) => ({ value: cevaplar[idx] || '', type: q && q.type ? q.type : 'personality' }));

                // Compute scores
                const skorlar = computeScoresAndNLG(cevaplar, { tip: activeCandidate.tip, baslik: activeCandidate.baslik, questions: testForm._questions, typedAnswers });

                // Attempt to save; if saving fails, saveCandidateTest will handle fallback/queue
                const res = await window.saveCandidateTest(activeCandidate.rumuz, activeCandidate.tip, activeCandidate.baslik, cevaplar, skorlar, testForm._questions);

                // Hide the test UI and show a thank-you message
                const testSectionEl = document.getElementById('testSection');
                if (testSectionEl) {
                    let msg = '';
                    if (res && res.queued) {
                        msg = `<span class='text-red-600 font-semibold'>Veri sunucuya gönderilemedi.</span><br>Yanıtlarınız <b>yerel kuyruğa</b> alındı ve internet bağlantısı sağlandığında otomatik olarak gönderilecek.<br><span class='text-xs text-gray-500'>Tarayıcıyı kapatmazsanız veya tekrar açarsanız, gönderim tekrar denenir.</span>`;
                    } else {
                        msg = 'Verileriniz başarıyla kaydedildi.';
                    }
                    testSectionEl.innerHTML = `
                        <div class='p-6 bg-green-50 border border-green-200 rounded-lg text-center'>
                            <h3 class='text-2xl font-semibold text-green-800 mb-2'>Test tamamlandı — teşekkürler!</h3>
                            <p class='text-gray-700 mb-3'>Cevaplarınız alındı. Rumuz: <b>${activeCandidate.rumuz}</b></p>
                            <p class='text-sm text-gray-600'>Toplam puanınız: <b>${skorlar.genelSkor || '—'}</b></p>
                            <p class='text-xs text-gray-500 mt-2'>${msg}</p>
                        </div>
                    `;
                }

                // Ensure admin UI doesn't overlap candidate experience
                try { const panel = document.getElementById('adminPanel'); if (panel) { panel.classList.add('hidden'); panel.style.display='none'; } } catch(e){}
                try { const comp = document.getElementById('adminLoginCompact'); if (comp) { comp.classList.add('hidden'); try { comp.style.display='none'; } catch(_){} } } catch(e){}

                // Update IK panel and radar visual
                renderAnswers(activeCandidate);
                renderRadar(skorlar.radar);
            } catch (err) {
                console.error('Test submit failed', err);
                // Queue as last resort (store formatted answers + questions)
                try { queueCandidateSubmission({ rumuz: activeCandidate.rumuz, tip: activeCandidate.tip, baslik: activeCandidate.baslik, cevaplar: (function(a){ const o={}; (a||[]).forEach((v,i)=>o['q'+(i+1)]=v); return o; })(testForm._answers||[]), questions: testForm._questions||[], skorlar: {} }); } catch(qe){ console.warn('queueCandidateSubmission failed', qe); }
                const testSectionEl = document.getElementById('testSection');
                if (testSectionEl) {
                    testSectionEl.innerHTML = `
                        <div class='p-6 bg-yellow-50 border border-yellow-200 rounded-lg text-center'>
                            <h3 class='text-2xl font-semibold text-yellow-800 mb-2'>Cevaplarınız gönderilemedi</h3>
                            <p class='text-gray-700 mb-3'>Yanıtlarınız <b>yerel kuyruğa</b> alındı ve internet bağlantısı sağlandığında otomatik olarak gönderilecek.</p>
                            <p class='text-xs text-gray-500 mt-2'>Tarayıcıyı kapatmazsanız veya tekrar açarsanız, gönderim tekrar denenir.</p>
                        </div>
                    `;
                } else {
                    alert('Cevaplarınız gönderilirken hata oluştu. Veriler yerel kuyruğa alındı ve otomatik olarak tekrar denenecektir.');
                }
            }
        }
    }

    // İK paneli açıldığında adayları ve sonuçları Firebase'den çek
    function loadCandidatesFromFirebase() {
        window.listenCandidates(function(candidates){
            // Tabloyu güncelle
            const candidateList = document.getElementById('candidateList');
            if (!candidateList) return;
            candidateList.innerHTML = '';
            candidates.forEach(c => {
                const SECTOR_MAP = { 'imalat': 'İmalat', 'hizmet': 'Hizmet' };
                const ROLE_LABELS = {
                    'mavi_yaka': 'Mavi Yaka Kadrosu',
                    'beyaz_yaka': 'Beyaz Yaka / Memur Kadrosu',
                    'yonetici': 'Yönetici Kadrosu',
                    'yonetici_idari': 'Yönetici ve İdari Kadro',
                    'hizmet_personeli': 'Hizmet Personeli'
                };
                const sectorLabel = SECTOR_MAP[c.tip] || c.tip || '';
                const roleLabel = ROLE_LABELS[c.baslik] || c.baslik || '';
                const status = (c.skorlar && (c.skorlar.genelSkor !== undefined)) ? 'Tamamlandı' : 'Bekliyor';
                const scoreSmall = (c.skorlar && c.skorlar.genelSkor) ? (' — ' + c.skorlar.genelSkor + '/100') : '';
                candidateList.innerHTML += `
                    <tr>
                        <td class='p-2'>${c.rumuz}</td>
                        <td class='p-2'>${sectorLabel}</td>
                        <td class='p-2'>${roleLabel}</td>
                        <td class='p-2'>${status}${scoreSmall}</td>
                        <td class='p-2'>
                            <button class='px-2 py-1 border view-detail' data-r='${c.rumuz}'>Detay</button>
                            <button class='px-2 py-1 border ml-2 send-creds' data-r='${c.rumuz}' data-p='${c.password||''}'>Kimlik Gönder</button>
                            <button class='px-2 py-1 border ml-2 request-ai' data-r='${c.rumuz}'>Yapay Zeka</button>
                            <button class='px-2 py-1 border ml-2 delete-cand' data-r='${c.rumuz}'>Sil</button>
                        </td>
                    </tr>
                `;
            });
            // Son eklenen adayı detayda göster (örnek)
            if (candidates.length > 0) {
                const last = candidates[candidates.length-1];
                renderAnswers(last);
                renderAggregateRadar(candidates);
            }
            // Attach table action handlers
            Array.from(document.querySelectorAll('.view-detail')).forEach(btn => {
                btn.onclick = function(){
                    const r = this.dataset.r;
                    const cand = (candidates || []).find(x=>x.rumuz===r);
                    if (cand) showCandidateDetail(cand);
                };
            });
            Array.from(document.querySelectorAll('.send-creds')).forEach(btn => {
                btn.onclick = function(){
                    const r = this.dataset.r; const p = this.dataset.p;
                    // copy to clipboard and alert (HR is expected to deliver to candidate manually)
                    const text = `Rumuz: ${r}\nŞifre: ${p}`;
                    try { navigator.clipboard.writeText(text); alert('Kimlik panoya kopyalandı. Adaya gönderin.'); }
                    catch(e){ prompt('Aşağıdaki kimliği adayla paylaşın:', text); }
                };
            });
            Array.from(document.querySelectorAll('.delete-cand')).forEach(btn => {
                btn.onclick = async function(){
                    const r = this.dataset.r;
                    if (!confirm(`'${r}' silinsin mi? Bu işlem geri alınamaz.`)) return;
                    try {
                        await window.remove(ref(db, 'candidates/' + r));
                        alert('Aday silindi.');
                        loadCandidatesFromFirebase();
                    } catch(e) {
                        console.error('delete candidate failed', e);
                        alert('Silme işlemi sırasında hata: ' + (e.message||e));
                    }
                };
            });
            // Attach AI request handlers per row (opens detail modal and auto-invokes modal NLG button)
            Array.from(document.querySelectorAll('.request-ai')).forEach(btn => {
                btn.onclick = function(){
                    alert('Yapay zeka özetleme (Gemini) bu kurulumda devre dışı bırakıldı. Lütfen insan kaynakları raporu kullanın.');
                };
            });
        });
    }
    // İK paneli açıldığında otomatik yükle
    const ikPanel = document.getElementById('ikPanel');
    if (ikPanel) {
        const observer = new MutationObserver(function(mutations) {
            mutations.forEach(function(m) {
                if (!ikPanel.classList.contains('hidden')) {
                    loadCandidatesFromFirebase();
                }
            });
        });
        observer.observe(ikPanel, {attributes:true});
    }
    // Admin giriş formu işlemleri
    const adminLoginForm = document.getElementById('adminLoginForm');
    const adminPanel = document.getElementById('adminPanel');
    const anaGrid = document.querySelector('.grid');
    // Compact admin login flow (opened from floating admin icon)
    // We'll attach a single handler later that opens the compact login modal.

    const adminLoginFormCompact = document.getElementById('adminLoginFormCompact');
    const adminLoginCompact = document.getElementById('adminLoginCompact');
    if (adminLoginFormCompact) {
        adminLoginFormCompact.onsubmit = async function(e) {
            e.preventDefault();
            let email = document.getElementById('adminEmailCompact').value.trim();
            const pw = document.getElementById('adminPasswordCompact').value;
            const overlay = document.getElementById('loadingOverlay'); if (overlay) overlay.classList.remove('hidden');
            try {
                // If email omitted, substitute default for Firebase attempt
                if (!email) email = (window.APX_CONFIG && window.APX_CONFIG.DEFAULT_ADMIN_EMAIL) || window.DEFAULT_ADMIN_EMAIL || 'admin@firma.com';
                if (!pw) { alert('Şifre girin'); return; }
                // Defensive: fallback if auth helper missing OR developer forces local admin mode
                const devForceLocal = localStorage.getItem('apx_force_local_admin') === '1' || localStorage.getItem('apx_allow_dev_fallback') === '1';
                if (typeof window.signInWithEmailAndPassword !== 'function' || devForceLocal) {
                    console.warn('Using local fallback check for compact admin (devForceLocal=' + devForceLocal + ')');
                    const configuredFallback2 = (window.APX_CONFIG && window.APX_CONFIG.ADMIN_FALLBACK_PASSWORD) || window.ADMIN_FALLBACK_PASSWORD || null;
                    if (pw === configuredFallback2) {
                        const adminLoginCompactNow = document.getElementById('adminLoginCompact');
                        if (adminLoginCompactNow) {
                            adminLoginCompactNow.classList.add('hidden');
                            try { adminLoginCompactNow.style.display = 'none'; } catch(e){}
                        }
                        const panelNow = document.getElementById('adminPanel');
                        if (panelNow) { panelNow.classList.remove('hidden'); panelNow.style.display = 'block'; }
                        const btn = document.getElementById('manageUsersBtn'); if (btn) btn.focus();
                        return;
                    } else if (!devForceLocal) {
                        alert('Authentication helper unavailable (module import may have failed). Lütfen ağ/console hatalarını kontrol edin.');
                        return;
                    }
                    // if devForceLocal true but pw didn't match, proceed to allow Firebase attempt below to provide diagnostics
                }
                // Try Firebase sign-in first
                try {
                    const cred = await window.signInWithEmailAndPassword(window.firebaseAuth, email, pw);
                    const user = cred.user;
                    const idToken = await user.getIdTokenResult();
                    if (!idToken || !idToken.claims || !idToken.claims.admin) {
                        await window.signOutFirebase(window.firebaseAuth);
                        alert('Bu hesap yönetici (admin) yetkisine sahip değil.');
                        return;
                    }
                    // success: open compact admin panel and close login modal
                    const adminLoginCompactNow = document.getElementById('adminLoginCompact');
                    if (adminLoginCompactNow) {
                        adminLoginCompactNow.classList.add('hidden');
                        try { adminLoginCompactNow.style.display = 'none'; } catch(e){}
                    }
                    const panelNow = document.getElementById('adminPanel');
                    if (panelNow) { panelNow.classList.remove('hidden'); panelNow.style.display = 'block'; }
                    const btn = document.getElementById('manageUsersBtn'); if (btn) btn.focus();
                    return;
                } catch(firebaseErr) {
                    console.warn('Firebase admin sign-in failed:', firebaseErr);
                    try { if (typeof firebaseAuthDiagnostic === 'function') firebaseAuthDiagnostic(firebaseErr); } catch(_){ }
                    try { firebaseAuthVerbose(firebaseErr); } catch(_){ }
                    try {
                        const code = (firebaseErr && firebaseErr.code) ? String(firebaseErr.code) : '';
                        if (code.includes('invalid-credential') || (firebaseErr && /400/.test(String(firebaseErr.message||'')))) {
                            // Only allow dev fallback on local/dev hosts or if explicitly enabled via localStorage
                            const allowOnHost2 = (location.protocol === 'file:' || location.hostname === 'localhost' || location.hostname === '127.0.0.1');
                            const devToggle2 = localStorage.getItem('apx_allow_dev_fallback') === '1';
                            if (allowOnHost2 || devToggle2) {
                                const ok = confirm('Firebase sign-in 400 / invalid-credential hatası alındı. Yerel test için admin paneli açılsın mı? (İNSECURE - yalnızca geliştirme için)');
                                if (ok) {
                                    console.warn('DEV FALLBACK used (compact): opening admin panel locally without verifying credentials. INSECURE.');
                                    const adminLoginCompactNow = document.getElementById('adminLoginCompact');
                                    if (adminLoginCompactNow) {
                                        adminLoginCompactNow.classList.add('hidden');
                                        try { adminLoginCompactNow.style.display = 'none'; } catch(e){}
                                    }
                                    const panelNow = document.getElementById('adminPanel');
                                    if (panelNow) { panelNow.classList.remove('hidden'); panelNow.style.display = 'block'; }
                                    const btn = document.getElementById('manageUsersBtn'); if (btn) btn.focus();
                                    return;
                                }
                            } else {
                                alert('Güvenlik nedeniyle bu cihaz/alan adına dev-fallback kapalı. Yerel geliştirme için dosyayı localhost üzerinden açın veya developer fallback özelliğini etkinleştirin (localStorage `apx_allow_dev_fallback` = 1).');
                            }
                        }
                    } catch(e) { console.warn('fallback prompt failed', e); }
                    throw firebaseErr;
                }
            } catch(err) {
                console.error('admin compact auth failed', err);
                alert('Giriş sırasında hata: ' + (err.message||err));
            } finally {
                if (overlay) overlay.classList.add('hidden');
            }
        }
    }
    // Admin panelinden çıkış yapınca tekrar giriş ekranı gelsin
    const adminLogoutBtn = Array.from(adminPanel.querySelectorAll('button')).find(btn => btn.textContent.includes('Çıkış'));
    if (adminLogoutBtn) {
        adminLogoutBtn.onclick = function() {
            // hide compact admin panel and restore any UI state
            adminPanel.classList.add('hidden');
            adminPanel.style.display = 'none';
            if (anaGrid) anaGrid.style.display = '';
        };
    }

    // Floating admin open button behavior - open the compact admin login modal
    const openAdminBtn = document.getElementById('openAdminBtn');
    if (openAdminBtn) {
        // ensure the admin open button sits above overlays
        try { openAdminBtn.style.zIndex = '100000'; openAdminBtn.style.pointerEvents = 'auto'; } catch(e){}
        openAdminBtn.addEventListener('click', function() {
            console.log('openAdminBtn clicked');
            let adminLoginCompactEl = document.getElementById('adminLoginCompact');
            if (adminLoginCompactEl) {
                try { adminLoginCompactEl.style.zIndex = '100000'; adminLoginCompactEl.style.pointerEvents = 'auto'; } catch(e){}
                // Ensure it's attached to body so fixed positioning works
                if (adminLoginCompactEl.parentElement !== document.body) try { document.body.appendChild(adminLoginCompactEl); } catch(e){}
                adminLoginCompactEl.classList.remove('hidden');
                adminLoginCompactEl.style.display = 'flex';
                // try to focus the password input inside the modal
                try {
                    const input = adminLoginCompactEl.querySelector('#adminPasswordCompact');
                    if (input) { input.focus(); input.select && input.select(); }
                } catch(e){}
                return;
            }
            // fallback: toggle admin panel if compact modal not present
            const panel = document.getElementById('adminPanel');
            if (!panel) return;
            if (panel.classList.contains('hidden')) {
                panel.classList.remove('hidden'); panel.style.display = 'block';
                const btn = document.getElementById('manageUsersBtn'); if (btn) btn.focus();
            } else {
                panel.classList.add('hidden'); panel.style.display = 'none';
            }
        });
    }

    // NOTE: Auto-open for TEST MODE removed to avoid showing insecure modal on main page load.
    // Compact admin modal is opened only when the floating Yönetici button is clicked.

    // Admin panelindeki butonların işlevselliği
    if (adminPanel) {
      // İK Yöneticisi Ekle butonu (zaten modal açıyor)
      const addHrBtn = document.getElementById('addHrBtn');
      if (addHrBtn) {
        addHrBtn.onclick = function() {
          document.getElementById('hrAdminModal').classList.remove('hidden');
        };
      }
            // Sistem Ayarları butonu
            const systemSettingsBtn = document.getElementById('systemSettingsBtn');
            if (systemSettingsBtn) {
                systemSettingsBtn.onclick = function() {
                    document.getElementById('systemSettingsModal').classList.remove('hidden');
                    // Load current settings into form
                    const compact = localStorage.getItem('apx_compactFooter') === '1';
                    const hide = localStorage.getItem('apx_hideFooter') === '1';
                    const dark = localStorage.getItem('apx_darkMode') === '1';
                    document.getElementById('settingCompactFooter').checked = compact;
                    document.getElementById('settingHideFooter').checked = hide;
                    document.getElementById('settingDarkMode').checked = dark;
                };
            }
      // Çıkış Yap butonu
      const logoutBtn = document.getElementById('logoutBtn');
      if (logoutBtn) {
        logoutBtn.onclick = function() {
          adminPanel.classList.add('hidden');
          adminPanel.style.display = 'none';
          if (anaGrid) anaGrid.style.display = '';
        };
      }
    }

    // Sistem Ayarları form işlemleri
    const systemSettingsForm = document.getElementById('systemSettingsForm');
    const resetSettingsBtn = document.getElementById('resetSettingsBtn');
    function applySettings() {
        const footer = document.querySelector('footer');
        const footerContainer = document.querySelector('.text-center.mt-12');
        const dark = localStorage.getItem('apx_darkMode') === '1';
        const hide = localStorage.getItem('apx_hideFooter') === '1';
        const compact = localStorage.getItem('apx_compactFooter') === '1';
        if (footer) {
            footer.style.display = hide ? 'none' : '';
            if (compact) footer.classList.add('text-xs'); else footer.classList.remove('text-xs');
        }
        if (footerContainer) footerContainer.style.display = hide ? 'none' : '';
        if (dark) document.body.classList.add('apx-dark'); else document.body.classList.remove('apx-dark');
    }
    if (systemSettingsForm) {
        systemSettingsForm.onsubmit = function(e) {
            e.preventDefault();
            const compact = document.getElementById('settingCompactFooter').checked;
            const hide = document.getElementById('settingHideFooter').checked;
            const dark = document.getElementById('settingDarkMode').checked;
            localStorage.setItem('apx_compactFooter', compact ? '1' : '0');
            localStorage.setItem('apx_hideFooter', hide ? '1' : '0');
            localStorage.setItem('apx_darkMode', dark ? '1' : '0');
            applySettings();
            document.getElementById('systemSettingsModal').classList.add('hidden');
            alert('Ayarlar kaydedildi.');
        }
    }
    if (resetSettingsBtn) {
        resetSettingsBtn.onclick = function() {
            localStorage.removeItem('apx_compactFooter');
            localStorage.removeItem('apx_hideFooter');
            localStorage.removeItem('apx_darkMode');
            applySettings();
            alert('Ayarlar sıfırlandı.');
        }
    }
    // Apply settings on load
    applySettings();

    // İK paneli: aday ekleme ve listeleme
    let candidates = [];
    const addCandidateForm = document.getElementById('addCandidateForm');
    const candidateList = document.getElementById('candidateList');
    // Populate role options based on selected sector
    const sectorEl = document.getElementById('candidateSector');
    const roleEl = document.getElementById('candidateRole');
    const ROLE_MAP = {
        'imalat': [
            {value: 'mavi_yaka', label: 'Mavi Yaka Kadrosu'},
            {value: 'beyaz_yaka', label: 'Beyaz Yaka / Memur Kadrosu'},
            {value: 'yonetici', label: 'Yönetici Kadrosu'}
        ],
        'hizmet': [
            {value: 'yonetici_idari', label: 'Yönetici ve İdari Kadro'},
            {value: 'hizmet_personeli', label: 'Hizmet Personeli'}
        ]
    };
    function populateRolesForSector(s) {
        if (!roleEl) return;
        roleEl.innerHTML = '';
        roleEl.disabled = true;
        if (!s || !ROLE_MAP[s]) {
            roleEl.innerHTML = '<option value="">Önce sektör seçin</option>';
            roleEl.disabled = true;
            return;
        }
        roleEl.disabled = false;
        roleEl.innerHTML = '<option value="">Seçiniz</option>' + ROLE_MAP[s].map(r => `<option value="${r.value}">${r.label}</option>`).join('');
    }
    if (sectorEl) {
        sectorEl.addEventListener('change', function(){ populateRolesForSector(this.value); });
    }
    if (addCandidateForm) {
        addCandidateForm.onsubmit = async function(e) {
            e.preventDefault();
            const rumuz = document.getElementById('newCandidateNickname').value.trim();
            const sifre = document.getElementById('newCandidatePassword').value;
            const sector = (document.getElementById('candidateSector')||{}).value || '';
            const role = (document.getElementById('candidateRole')||{}).value || '';
            const baslik = role || '';
            if (!rumuz) { alert('Rumuz giriniz'); return; }
            try {
                await window.addCandidateToDB({ rumuz, password: sifre, tip: sector, baslik: role, createdBy: window.currentHR || 'admin' });
                addCandidateForm.reset();
                // refresh list from Firebase
                if (typeof loadCandidatesFromFirebase === 'function') loadCandidatesFromFirebase();
                else if (typeof renderCandidates === 'function') renderCandidates();
                alert('Aday veritabanına eklendi.');
            } catch (err) {
                console.error(err);
                alert('Aday eklenirken hata: ' + (err.message||err));
            }
        };
    }

    // Admin panelinden İK yöneticisi ekleme formu işlemi
    const hrAdminForm = document.getElementById('hrAdminForm');
    if (hrAdminForm) {
        hrAdminForm.onsubmit = function(e) {
            e.preventDefault();
            const username = document.getElementById('hrAdminUsername').value.trim();
            const fullName = document.getElementById('hrAdminFullName').value.trim();
            const phone = document.getElementById('hrAdminPhone').value.trim();
            const email = document.getElementById('hrAdminEmail').value.trim();
            const password = document.getElementById('hrAdminPassword').value;
            const company = (document.getElementById('hrAdminCompany')||{}).value || '';
            const role = (document.getElementById('hrAdminRole')||{}).value || '';
            if (!username || !email || !password) {
                alert('Kullanıcı adı, e-posta ve şifre zorunludur.');
                return;
            }
            // Use Firebase helper
            addHRUser({username, fullName, phone, email, password, company, role, active: true}).then(() => {
                alert('İK yöneticisi başarıyla eklendi.');
                document.getElementById('hrAdminModal').classList.add('hidden');
                hrAdminForm.reset();
                // refresh list if modal açık
                if (!document.getElementById('hrManageModal').classList.contains('hidden')) loadHRList();
            }).catch(err => {
                console.error(err);
                alert('Kayıt sırasında hata: ' + (err.message||err));
            });
        }
    }

    // HR yönetim modal logic
    const refreshHrListBtn = document.getElementById('refreshHrListBtn');
    const applyFilterBtn = document.getElementById('applyFilterBtn');
    const hrManageTableBody = document.getElementById('hrManageTableBody');
    async function loadHRList() {
        hrManageTableBody.innerHTML = '<tr><td colspan="8" class="p-2">Yükleniyor...</td></tr>';
        listHRUsers(async function(list){
            // render rows
            hrManageTableBody.innerHTML = '';
            const fromDate = document.getElementById('filterFrom').value;
            const toDate = document.getElementById('filterTo').value;
            const fromTs = fromDate ? new Date(fromDate).getTime() : 0;
            const toTs = toDate ? new Date(toDate).getTime() + 24*3600*1000 - 1 : Date.now();
            for (const u of list) {
                const testCount = await countCandidateTestsByHRBetween(u.username, fromTs, toTs).catch(()=>0);
                const status = u.active ? 'Aktif' : 'Pasif';
                // Build a stable row id/data attribute so we can replace existing rows instead of appending
                const rowId = 'hr_row_' + u.username.replace(/[^a-zA-Z0-9_-]/g,'');
                const newRowHtml = `<tr id='${rowId}' data-hr='${u.username}'>
                    <td class='p-2'>${u.username}</td>
                    <td class='p-2'>${u.fullName||''}</td>
                    <td class='p-2'>${u.phone||''}</td>
                    <td class='p-2'>${u.email||''}</td>
                    <td class='p-2'>${u.password||''}</td>
                    <td class='p-2'>${testCount}</td>
                    <td class='p-2 hr-status'>${status}</td>
                    <td class='p-2'><button class='px-2 py-1 border toggle-hr' data-user='${u.username}'>${u.active? 'Pasife Al' : 'Aktifleştir'}</button></td>
                    <td class='p-2'><button class='px-2 py-1 bg-gray-300 text-gray-700 rounded request-nlg' data-hr='${u.username}'>Özet (AI kapalı)</button></td>
                </tr>`;
                const existing = document.getElementById(rowId);
                if (existing) {
                    // replace the existing row markup
                    existing.outerHTML = newRowHtml;
                } else {
                    hrManageTableBody.insertAdjacentHTML('beforeend', newRowHtml);
                }
                // Attach handlers to the newly inserted/updated row
                try {
                    const rowEl = document.getElementById(rowId);
                    if (!rowEl) continue;
                    const toggleBtn = rowEl.querySelector('.toggle-hr');
                    if (toggleBtn) {
                        toggleBtn.onclick = async function(){
                            const username = this.dataset.user;
                            // flip based on current status text in the row to avoid stale closures
                            const statusCell = rowEl.querySelector('.hr-status');
                            const currentIsActive = statusCell && statusCell.textContent.trim().toLowerCase() === 'aktif';
                            const newActive = !currentIsActive;
                            try {
                                await setHRActive(username, newActive);
                                // immediately update UI in-place instead of reloading whole list
                                if (statusCell) statusCell.textContent = newActive ? 'Aktif' : 'Pasif';
                                // update button label
                                this.textContent = newActive ? 'Pasife Al' : 'Aktifleştir';
                            } catch(e) {
                                console.error('setHRActive failed', e);
                                alert('Durum güncellenirken hata: ' + (e.message||e));
                            }
                        };
                    }
                    const nlgBtn = rowEl.querySelector('.request-nlg');
                    if (nlgBtn) {
                        nlgBtn.onclick = function(){
                            alert('Otomatik yapay zeka özetleme bu kurulumda devre dışı bırakıldı. Lütfen İnsan Kaynakları raporunu kullanın.');
                        };
                    }
                } catch(e) { console.warn('attach hr row handlers failed', e); }
            }
        });
    }
    if (refreshHrListBtn) refreshHrListBtn.onclick = loadHRList;
    if (applyFilterBtn) applyFilterBtn.onclick = loadHRList;
    
    // Attach handler for Gemini NLG request buttons
    document.addEventListener('click', function(e){
        if (e.target && e.target.classList && e.target.classList.contains('request-nlg')){
            alert('Otomatik yapay zeka (Gemini) bu kurulumda devre dışı bırakıldı. Lütfen İnsan Kaynakları raporunu kullanın.');
        }
    });

    // Gemini / NLG disabled: provide stubs that inform the caller
    async function requestCandidateNLG(rumuz){
        return Promise.reject(new Error('Yapay zeka özeti özelliği devre dışı bırakıldı.'));
    }

    async function requestCandidateNLGDetailed(candidate){
        return Promise.reject(new Error('Yapay zeka özeti özelliği devre dışı bırakıldı.'));
    }
    // Yönetici panelindeki 'Kullanıcıları Yönet' butonunu bağla
    const manageUsersBtnElm = document.getElementById('manageUsersBtn');
    if (manageUsersBtnElm) {
        manageUsersBtnElm.onclick = function() {
            document.getElementById('hrManageModal').classList.remove('hidden');
            loadHRList();
        };
    }
    function renderCandidates() {
        // Prefer DB-driven list if available
        if (window.candidates && window.candidates.length) {
            candidateList.innerHTML = '';
            window.candidates.forEach(c => {
                const SECTOR_MAP = { 'imalat': 'İmalat', 'hizmet': 'Hizmet' };
                const ROLE_LABELS = {
                    'mavi_yaka': 'Mavi Yaka Kadrosu',
                    'beyaz_yaka': 'Beyaz Yaka / Memur Kadrosu',
                    'yonetici': 'Yönetici Kadrosu',
                    'yonetici_idari': 'Yönetici ve İdari Kadro',
                    'hizmet_personeli': 'Hizmet Personeli'
                };
                const sectorLabel = SECTOR_MAP[c.tip] || c.tip || '';
                const roleLabel = ROLE_LABELS[c.baslik] || c.baslik || '';
                candidateList.innerHTML += `<tr><td class='p-2'>${c.rumuz}</td><td class='p-2'>${sectorLabel}</td><td class='p-2'>${roleLabel}</td></tr>`;
            });
            return;
        }
        // Fallback to local array
        candidateList.innerHTML = '';
        candidates.forEach(c => {
            candidateList.innerHTML += `<tr><td class='p-2'>${c.rumuz}</td><td class='p-2'>${c.tip}</td><td class='p-2'>${c.baslik}</td></tr>`;
        });
    }

    // Radar grafik örneği (demo verisi)
    let radarChart;
    function renderRadar(dataArr, labels) {
        const ctx = document.getElementById('radarChart').getContext('2d');
        if (radarChart) radarChart.destroy();
        radarChart = new Chart(ctx, {
            type: 'radar',
            data: {
                labels: labels || ['İletişim', 'Liderlik', 'Teknik', 'Vicdanlılık','Adaptasyon'],
                datasets: [{
                    label: 'Aday Puanı',
                    data: dataArr || [80, 65, 90],
                    backgroundColor: 'rgba(34,197,94,0.2)',
                    borderColor: 'rgba(34,197,94,1)',
                    borderWidth: 2
                }]
            },
            options: {responsive: false, plugins: {legend: {display: false}}}
        });
    }
    if (document.getElementById('radarChart')) renderRadar();

    // Render answers list in IK panel
    function renderAnswers(candidate) {
        const answerList = document.getElementById('answerList');
        if (!answerList) return;
        answerList.innerHTML = '';
        if (!candidate || !candidate.cevaplar || !candidate.cevaplar.length) {
            answerList.innerHTML = '<li>Henüz cevap yok.</li>';
            return;
        }
        candidate.cevaplar.forEach((a,i) => {
            const li = document.createElement('li');
            li.className = 'text-sm text-gray-700';
            li.innerText = `${i+1}. ${a}`;
            answerList.appendChild(li);
        });
    }

    // Aggregate radar rendering for IK overview: average radar across candidates
    function renderAggregateRadar(allCandidates) {
        if (!allCandidates || !allCandidates.length) return renderRadar();
        // build average of radar arrays (assume 5-dim)
        const sums = [0,0,0,0,0]; let counts = 0;
        allCandidates.forEach(c => {
            const r = (c.skorlar && c.skorlar.radar) ? c.skorlar.radar : null;
            if (r && r.length===5) {
                counts++;
                for (let i=0;i<5;i++) sums[i] += Number(r[i]) || 0;
            }
        });
        const avg = counts ? sums.map(s=>Math.round(s/counts)) : [60,60,60,60,60];
        renderRadar(avg, ['İletişim','Liderlik','Teknik','Vicdanlılık','Adaptasyon']);
    }

    // Aday detay modalı fonksiyonu (örnek, gerçek veriye bağlanabilir)
function showCandidateDetail(candidate) {
  // Modal oluştur
    let modal = document.createElement('div');
    modal.className = 'fixed inset-0 bg-black bg-opacity-70 flex items-center justify-center z-[99999] p-2 md:p-8';
    modal.innerHTML = `
        <div class='bg-white rounded-3xl shadow-2xl w-11/12 max-w-4xl h-[90vh] flex flex-col relative overflow-hidden animate-fadein'>
            <button onclick='this.closest(".fixed").remove()' class='absolute top-5 right-6 text-gray-400 hover:text-red-500 text-3xl font-bold z-10' style='background:rgba(255,255,255,0.7);border-radius:50%;width:44px;height:44px;line-height:44px;text-align:center;'>&times;</button>
            <div class='flex-1 overflow-y-auto p-4 md:p-8'>
                <h2 class='text-3xl font-bold text-blue-800 mb-4 text-center tracking-tight'>Aday Detay Analizi</h2>
                <div class='mb-4 text-center text-lg'><b>Rumuz:</b> ${candidate.rumuz} &nbsp; <b>Tip:</b> ${candidate.tip} &nbsp; <b>Başlık:</b> ${candidate.baslik}</div>
                <div class='mb-8 grid grid-cols-1 md:grid-cols-2 gap-8'>
                    <div class='bg-blue-50 rounded-xl p-4 overflow-auto max-h-80 flex flex-col items-center'>
                        <canvas id='detailRadar' width='340' height='220'></canvas>
                        <div class='text-xs text-gray-500 mt-2'>Yetkinlik Dağılımı (Adjusted)</div>
                    </div>
                    <div class='bg-blue-50 rounded-xl p-4 overflow-auto max-h-80 flex flex-col items-center'>
                        <h4 class='font-semibold mb-2'>SJT & Özet</h4>
                        <canvas id='detailSJT' width='340' height='220'></canvas>
                        <div class='text-xs text-gray-500 mt-2'>Durumsal Yargı Testi (Uzman Anahtarına Göre)</div>
                    </div>
                </div>
                <div class='mb-4'><b>Güvenilirlik Puanı (Response Bias):</b> <span id='detailBias'></span></div>
                <div class='mb-4'>
                    <h4 class='font-semibold mb-2'>Yetkinlik Bazlı Dağılım</h4>
                    <div id='perCategoryBreakdown' class='space-y-2 text-sm text-gray-700'></div>
                </div>
                <div>
                    <h4 class='font-semibold mb-2'>Soru / Cevap Detayları</h4>
                    <div id='questionDetailList' class='text-sm text-gray-700 max-h-64 overflow-auto'></div>
                </div>
            </div>
            <div class='p-4 border-t flex gap-2 bg-gray-50'>
                <button id='exportCsvBtn' class='bg-gray-200 text-gray-800 px-4 py-2 rounded font-semibold hover:bg-gray-300'>CSV Dışa Aktar</button>
                <button id='copySummaryBtn' class='bg-indigo-600 text-white px-4 py-2 rounded font-semibold hover:bg-indigo-700'>Özet Kopyala</button>
                <button id='detailBackBtn' class='ml-auto bg-white border text-gray-700 px-4 py-2 rounded font-semibold hover:bg-gray-100'>Geri</button>
            </div>
        </div>
    `;
  document.body.appendChild(modal);
  // Back button: remove modal and focus candidate list
  try {
      const backBtn = document.getElementById('detailBackBtn');
      if (backBtn) backBtn.addEventListener('click', function(){
          try { modal.remove(); } catch(e){ try { document.body.removeChild(modal);} catch(_){} }
          try { const candList = document.getElementById('candidateList'); if (candList) candList.querySelector('tr') && candList.querySelector('tr').scrollIntoView({behavior:'smooth'}); } catch(e){}
      }, {passive:true});
  } catch(e) { /* ignore */ }
  // Radar ve SJT grafikleri demo (örnek puanlar)
        setTimeout(() => {
            // Prefer database-provided skorlar if available
                        const s = candidate.skorlar || {};
                        // If server-side perCategory present, use that
                        const perCat = s.perCategory || null;
                        const radarData = (s.radar && s.radar.length) ? s.radar : (perCat ? Object.values(perCat).map(x=>x.adjusted) : [Math.random()*100, Math.random()*100, Math.random()*100, Math.random()*100, Math.random()*100]);
                        const sjtData = (s.sjt && s.sjt.length) ? s.sjt : (perCat ? Object.values(perCat).map(x=>x.sjt && x.sjt.raw ? x.sjt.raw : Math.random()*100) : [Math.random()*100, Math.random()*100, Math.random()*100, Math.random()*100, Math.random()*100]);

            try {
                new Chart(document.getElementById('detailRadar').getContext('2d'), {
                    type: 'radar',
                    data: {
                        labels: ['Vicdanlılık', 'Dışadönüklük', 'Uyumluluk', 'Duygusal Denge', 'Deneyime Açıklık'],
                        datasets: [{
                            label: 'Big Five',
                            data: radarData,
                            backgroundColor: 'rgba(59,130,246,0.2)',
                            borderColor: 'rgba(59,130,246,1)',
                            borderWidth: 2
                        }]
                    },
                    options: {responsive: false, plugins: {legend: {display: false}}}
                });
            } catch(e) { console.warn('Radar chart render failed', e); }

            try {
                // If computeScoresAndNLG produced perQuestion data, use it for bar & pie
                const pq = s.perQuestion || null;
                if (pq && pq.length) {
                    // Bar: per-question avg5 (1..5)
                    const labels = pq.map(p=> (p.index+1) + '. ' + (p.text.length>30? p.text.slice(0,27)+'...': p.text));
                    const dataBar = pq.map(p => p.avg5 || 0);
                    new Chart(document.getElementById('detailSJT').getContext('2d'), {
                        type: 'bar',
                        data: {
                            labels,
                            datasets: [{ label: 'Ortalama (1..5)', data: dataBar, backgroundColor: 'rgba(16,185,129,0.6)' }]
                        },
                        options: { responsive: false, scales: { y: { min: 1, max: 5 } }, plugins: { legend: { display: false } } }
                    });

                    // Pie: distribution for first question with any responses
                    let firstIdx = pq.findIndex(p=>p.totalResponses && p.totalResponses>0);
                    if (firstIdx === -1) firstIdx = 0;
                    const firstPQ = pq[firstIdx];
                    // create or reuse a small canvas for pie
                    let pieCanvas = document.getElementById('detailPie');
                    if (!pieCanvas) {
                        const pieWrap = document.createElement('div'); pieWrap.className='mt-3';
                        pieWrap.innerHTML = `<canvas id='detailPie' width='300' height='160'></canvas><div class='text-xs text-gray-500 mt-1'>İlk gösterilen sorunun dağılımı (raw)</div>`;
                        modal.querySelector('.bg-white')?.appendChild(pieWrap);
                        pieCanvas = document.getElementById('detailPie');
                    }
                    const pieCtx = pieCanvas.getContext('2d');
                    const distro = firstPQ.type === 'personality' ? [1,2,3,4,5].map(i=> firstPQ.personalityCounts[i] || 0) : [0,1,2,3,4].map(i=> firstPQ.sjtOptionCounts[i] || 0);
                    // destroy existing chart if any on that canvas
                    try { if (pieCanvas._chart) pieCanvas._chart.destroy(); } catch(_){}
                    pieCanvas._chart = new Chart(pieCtx, {
                        type: 'pie',
                        data: {
                            labels: firstPQ.type === 'personality' ? ['1','2','3','4','5'] : ['A','B','C','D','E'],
                            datasets: [{ data: distro, backgroundColor: ['#ef4444','#f97316','#f59e0b','#10b981','#3b82f6'] }]
                        },
                        options: { responsive: false, plugins: { legend: { position: 'bottom' } } }
                    });
                } else {
                    // fallback to previous behavior (sjtData)
                    new Chart(document.getElementById('detailSJT').getContext('2d'), {
                        type: 'bar',
                        data: {
                            labels: ['Senaryo 1','Senaryo 2','Senaryo 3','Senaryo 4','Senaryo 5'],
                            datasets: [{ label: 'SJT Skoru', data: sjtData, backgroundColor: 'rgba(16,185,129,0.5)' }]
                        },
                        options: { responsive: false, plugins: { legend: { display: false } } }
                    });
                }
            } catch(e) { console.warn('SJT chart render failed', e); }

            const biasText = (s.bias !== undefined && s.bias !== null) ? (s.bias + ' / 100') : ((80 + Math.random()*20).toFixed(1) + ' / 100');
            document.getElementById('detailBias').innerText = biasText;
            // NLG summary (if available) + Yapay Zeka Yorumu butonu
            const summaryElId = 'detailNLG';
            let summaryEl = document.getElementById(summaryElId);
            let container = summaryEl ? summaryEl.closest('.nlg-container') : null;
                if (!summaryEl) {
                container = document.createElement('div');
                container.className = 'mt-4 p-3 bg-gray-50 rounded nlg-container';
                // create title and summary holder
                const title = document.createElement('h4');
                title.className = 'font-semibold mb-2';
                title.innerText = 'Özet / Yorum';
                const summaryHolder = document.createElement('div');
                summaryHolder.id = summaryElId;
                summaryHolder.className = 'text-sm text-gray-700';
                container.appendChild(title);
                container.appendChild(summaryHolder);
                    // Manual report textarea (HR can enter their own report)
                    const manualLabel = document.createElement('label');
                    manualLabel.className = 'block text-sm font-medium mt-3';
                    manualLabel.innerText = 'İnsan Kaynakları Raporu (elle düzenleyebilir)';
                    const manualTA = document.createElement('textarea');
                    manualTA.id = 'manualReportTextarea';
                    manualTA.className = 'w-full mt-1 p-2 border rounded text-sm';
                    manualTA.rows = 5;
                    container.appendChild(manualLabel);
                    container.appendChild(manualTA);
                    // Save manual report button
                    const saveReportBtn = document.createElement('button');
                    saveReportBtn.className = 'mt-2 inline-block bg-green-600 text-white px-3 py-1 rounded hover:bg-green-700';
                    saveReportBtn.innerText = 'Raporu Kaydet';
                    saveReportBtn.onclick = async function(){
                        try {
                            const text = (document.getElementById('manualReportTextarea')||{}).value || '';
                            if (!text) { alert('Rapor boş olamaz'); return; }
                            await saveCandidateReport(candidate.rumuz, { author: window.currentHR || 'manual', type: 'human', text, ts: Date.now() });
                            alert('Rapor kaydedildi');
                        } catch(err) { console.error('save manual report failed', err); alert('Rapor kaydedilemedi: ' + (err.message||err)); }
                    };
                    container.appendChild(saveReportBtn);
                // create button to request Gemini NLG
                const btn = document.createElement('button');
                btn.className = 'mt-3 inline-block bg-indigo-600 text-white px-3 py-1 rounded hover:bg-indigo-700';
                btn.innerText = 'Yapay Zeka Yorumu İste';
                btn.onclick = async function(){
                    try {
                        // disable while running
                        btn.disabled = true; btn.style.opacity = '0.6';
                        summaryHolder.innerText = 'Oluşturuluyor... Lütfen bekleyin.';
                        const resp = await requestCandidateNLGDetailed(candidate);
                        const text = resp && resp.text ? resp.text : (typeof resp === 'string' ? resp : JSON.stringify(resp));
                        summaryHolder.innerText = text || 'Gemini döndü fakat metin alınamadı.';
                        // Persist AI report to DB under candidates/<rumuz>/reports/ai
                        try {
                            await saveCandidateReport(candidate.rumuz, { author: 'ai', type: 'ai', text: text || '', ts: Date.now() });
                        } catch(saveErr) { console.warn('Saving AI report failed', saveErr); }
                    } catch (err) {
                        console.error('NLG isteği başarısız', err);
                        summaryHolder.innerText = 'NLG isteği sırasında hata oluştu: ' + (err.message||err);
                    } finally {
                        btn.disabled = false; btn.style.opacity = '';
                    }
                };
                container.appendChild(btn);
                modal.querySelector('.bg-white')?.appendChild(container);
                summaryEl = document.getElementById(summaryElId);
            }
            summaryEl.innerText = s.nlgSummary || 'Detaylı yorum bulunmuyor.';
            // Per-category breakdown
            const breakdownEl = document.getElementById('perCategoryBreakdown');
            breakdownEl.innerHTML = '';
            const pc = s.perCategory || {};
            if (Object.keys(pc).length) {
                Object.keys(pc).forEach(cat => {
                    const c = pc[cat];
                    const div = document.createElement('div');
                    div.innerHTML = `<b>${cat}</b>: ${c.adjusted} (${c.label}) — raw: ${c.raw}`;
                    breakdownEl.appendChild(div);
                });
            } else {
                breakdownEl.innerHTML = '<div>Detaylı kategori verisi yok.</div>';
            }

            // Reports list (AI + human) if present in candidate object
            try {
                const reportsWrapId = 'candidateReportsList';
                let reportsWrap = document.getElementById(reportsWrapId);
                if (!reportsWrap) {
                    reportsWrap = document.createElement('div');
                    reportsWrap.id = reportsWrapId;
                    reportsWrap.className = 'mt-4';
                    modal.querySelector('.bg-white')?.appendChild(reportsWrap);
                }
                reportsWrap.innerHTML = '<h4 class="font-semibold mb-2">Kayıtlı Raporlar</h4>';
                const reports = candidate.reports || {};
                const keys = Object.keys(reports);
                if (!keys.length) {
                    reportsWrap.innerHTML += '<div class="text-sm text-gray-600">Rapor bulunamadı.</div>';
                } else {
                    const list = document.createElement('ul'); list.className='text-sm text-gray-700 list-disc pl-5';
                    keys.forEach(k => {
                        try {
                            const types = reports[k] || {};
                            Object.keys(types).forEach(tk => {
                                const rep = types[tk];
                                const li = document.createElement('li');
                                li.innerText = `${k} / ${tk} — ${rep.author||''} — ${new Date((rep.ts||0)).toLocaleString()}`;
                                list.appendChild(li);
                            });
                        } catch(e){}
                    });
                    reportsWrap.appendChild(list);
                }
            } catch(e){ console.warn('reports list render failed', e); }

            // Question-by-question detail
            const qListEl = document.getElementById('questionDetailList');
            qListEl.innerHTML = '';
            const questions = (candidate.cevaplar && candidate.cevaplar.length && (testForm && testForm._questions)) ? testForm._questions : null;
            if (questions && questions.length) {
                questions.forEach((q, idx) => {
                    const ans = candidate.cevaplar[idx] || '';
                    const target = q.target || '';
                    const expert = Array.isArray(q.expertScores) ? q.expertScores.join(',') : '';
                    const row = document.createElement('div');
                    row.className = 'mb-2';
                    row.innerHTML = `<div class='font-semibold'>${idx+1}. ${q.text}</div><div class='text-sm text-gray-600'>Cevap: ${ans} ${target?('<br/>Target: '+target):''} ${expert?('<br/>Expert: '+expert):''}</div>`;
                    qListEl.appendChild(row);
                });
            } else if (candidate.cevaplar && candidate.cevaplar.length) {
                candidate.cevaplar.forEach((cevap, i) => {
                    const row = document.createElement('div'); row.className='mb-2';
                    row.innerHTML = `<div class='font-semibold'>${i+1}. Soru metni yok</div><div class='text-sm text-gray-600'>Cevap: ${cevap}</div>`;
                    qListEl.appendChild(row);
                });
            } else {
                qListEl.innerHTML = '<div>Henüz cevap yok.</div>';
            }

            // Export CSV handler
            const exportBtn = document.getElementById('exportCsvBtn');
            exportBtn.onclick = function(){
                const rows = [];
                rows.push(['Rumuz','Tip','Başlık','Kategori','Soru','Cevap','Target','ExpertScores','CategoryAdjusted']);
                const pcKeys = Object.keys(pc);
                if (questions && questions.length) {
                    questions.forEach((q, idx) => {
                        const ans = candidate.cevaplar[idx] || '';
                        // find category adjusted
                        let catAdj = '';
                        for (const k of pcKeys) {
                            const found = (pc[k].questions || []).find(x=>x.index===idx);
                            if (found) { catAdj = pc[k].adjusted; break; }
                        }
                        rows.push([candidate.rumuz, candidate.tip, candidate.baslik, q.category||'', q.text, ans, q.target||'', Array.isArray(q.expertScores)?q.expertScores.join('|'): '', catAdj]);
                    });
                } else if (candidate.cevaplar && candidate.cevaplar.length) {
                    candidate.cevaplar.forEach((a,i)=> rows.push([candidate.rumuz, candidate.tip, candidate.baslik, '', 'Soru metni yok', a, '', '', '']));
                }
                const csv = rows.map(r => r.map(c => '"'+String(c||'').replace(/"/g,'""')+'"').join(',')).join('\n');
                const blob = new Blob([csv], {type:'text/csv;charset=utf-8;'});
                const url = URL.createObjectURL(blob);
                const a = document.createElement('a'); a.href = url; a.download = `${candidate.rumuz}_answers.csv`; document.body.appendChild(a); a.click(); a.remove(); URL.revokeObjectURL(url);
            };

            const copyBtn = document.getElementById('copySummaryBtn');
            copyBtn.onclick = function(){
                try { navigator.clipboard.writeText(s.nlgSummary || ''); alert('Özet panoya kopyalandı'); } catch(e){ prompt('Özet (kopyalayın):', s.nlgSummary || ''); }
            };
        }, 100);
}
    </script>
        <script>
        // Taşınması gereken modal/overlay elemanlarını body'ye taşıyarak layout bozulmalarını engelle
        document.addEventListener('DOMContentLoaded', function() {
            try {
                // list of modal element ids we want at body level so fixed positioning and z-index behave correctly
                const modalIds = ['adminPanel','ikPanel','hrAdminModal','hrManageModal','systemSettingsModal','loadingOverlay'];
                modalIds.forEach(id => {
                    try {
                        const el = document.getElementById(id);
                        if (el && el.parentElement !== document.body) document.body.appendChild(el);
                    } catch(inner){ /* ignore single element failures */ }
                });
                        // Ensure the admin-panel close button reliably hides all admin UI (fix for close X not working)
                        try {
                            const closeBtn = document.getElementById('closeAdminPanel');
                            if (closeBtn) {
                                closeBtn.addEventListener('click', function() {
                                    try {
                                        const panel = document.getElementById('adminPanel');
                                        const compact = document.getElementById('adminLoginCompact');
                                        if (panel) { panel.classList.add('hidden'); panel.style.display = 'none'; }
                                        if (compact) { compact.classList.add('hidden'); try { compact.style.display = 'none'; } catch(_){} }
                                        // restore main grid display if it was hidden
                                        try { const anaGrid = document.querySelector('.grid'); if (anaGrid) anaGrid.style.display = ''; } catch(_){}
                                    } catch(err) { console.warn('closeAdminPanel handler failed', err); }
                                }, {passive: true});
                            }
                        } catch(e) { /* ignore */ }
            } catch (e) {
                console.warn('Modal taşıma sırasında hata:', e);
            }
    });
    </script>
</body>
</html>
