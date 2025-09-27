<!DOCTYPE html>
<html lang="tr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Giriş - Analiz Pro X</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0/css/all.min.css" rel="stylesheet">
    <style>
    /* Küçük tema destekleri */
    .apx-dark { background: linear-gradient(180deg,#0f172a,#07132a) !important; color: #e6eef8; }
    .apx-dark .bg-white { background-color: #0b1220 !important; }
    .apx-dark .text-gray-700, .apx-dark .text-gray-900 { color: #dbeafe !important; }
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
                            <label class="block text-sm font-medium text-gray-700 mb-1">Aday Tipi</label>
                            <select id="candidateType" class="w-full px-3 py-2 border rounded-lg">
                                <option value="">Seçiniz</option>
                                <option value="mavi yaka">Mavi Yaka</option>
                                <option value="beyaz yaka">Beyaz Yaka</option>
                                <option value="yonetici">Yönetici</option>
                            </select>
                        </div>
                        <div>
                            <label class="block text-sm font-medium text-gray-700 mb-1">Soru Başlığı</label>
                            <select id="questionCategory" class="w-full px-3 py-2 border rounded-lg">
                                <option value="">Seçiniz</option>
                                <option value="iletişim">İletişim</option>
                                <option value="liderlik">Liderlik</option>
                                <option value="teknik">Teknik</option>
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
                    <h3 class="text-lg font-bold text-red-700 mb-3">Yönetici Girişi</h3>
                    <form id="adminLoginFormCompact" class="space-y-3">
                        <div>
                            <label class="block text-sm text-gray-700 mb-1">E-posta</label>
                            <input type="email" id="adminEmailCompact" class="w-full px-3 py-2 border rounded" placeholder="admin@firma.com" required />
                        </div>
                        <div>
                            <label class="block text-sm text-gray-700 mb-1">Şifre</label>
                            <input type="password" id="adminPasswordCompact" class="w-full px-3 py-2 border rounded" placeholder="Yönetici şifresi" required />
                        </div>
                        <div>
                            <button type="submit" class="w-full bg-red-600 text-white py-2 rounded">Giriş Yap</button>
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
  import { initializeApp } from "https://www.gstatic.com/firebasejs/12.3.0/firebase-app.js";
    import { getDatabase, ref, set, push, onValue, get, update, query, orderByChild, equalTo } from "https://www.gstatic.com/firebasejs/12.3.0/firebase-database.js";
    import { getAuth, createUserWithEmailAndPassword, signInWithEmailAndPassword, signOut } from "https://www.gstatic.com/firebasejs/12.3.0/firebase-auth.js";

  const firebaseConfig = {
    apiKey: "AIzaSyC-ZvTo79-xDc9Uw2IMOZMwK9Egm9qODrU",
    authDomain: "ikpaneli.firebaseapp.com",
    projectId: "ikpaneli",
    storageBucket: "ikpaneli.firebasestorage.app",
    messagingSenderId: "645340845423",
    appId: "1:645340845423:web:435b57f7093782422e449a",
    measurementId: "G-6NBTKSBVYL",
    databaseURL: "https://ikpaneli-default-rtdb.europe-west1.firebasedatabase.app/"
  };

    const app = initializeApp(firebaseConfig);
    const db = getDatabase(app);
    const auth = getAuth(app);

    // Expose auth and helpers to global window for legacy non-module handlers
    // Note: avoid keeping any admin password on client in production
    window.firebaseAuth = auth;
    window.signInWithEmailAndPassword = signInWithEmailAndPassword;
    window.signOutFirebase = signOut;
    window.createUserWithEmailAndPassword = createUserWithEmailAndPassword;
    // Expose realtime-database helpers so non-module scripts (older inline code) can call set/update/get
    window.db = db;
    window.ref = ref;
    window.set = set;
    window.push = push;
    window.onValue = onValue;
    window.get = get;
    window.update = update;
    window.query = query;
    window.orderByChild = orderByChild;
    window.equalTo = equalTo;

  // ADAY TEST SONUÇLARINI KAYDETME
    window.saveCandidateTest = async function(rumuz, tip, baslik, cevaplar, skorlar) {
        // skorlar: {radar: [...], sjt: [...], bias: ...}
        // Compute deterministic scored values + NLG summary on client side as fallback
        try {
            const computed = computeScoresAndNLG(cevaplar, { tip, baslik });
            // merge provided skorlar (if any) with computed, but prefer computed values
            const finalSkorlar = Object.assign({}, skorlar || {}, computed);
            await update(ref(db, 'candidates/' + rumuz), {
                rumuz,
                tip,
                baslik,
                cevaplar,
                skorlar: finalSkorlar,
                timestamp: Date.now()
            });
            return { ok: true };
        } catch (err) {
            console.error('saveCandidateTest: primary save failed', err);
            // Try a raw save as a fallback but capture any permission errors
            try {
                await update(ref(db, 'candidates/' + rumuz), {
                    rumuz,
                    tip,
                    baslik,
                    cevaplar,
                    skorlar: skorlar || {},
                    timestamp: Date.now()
                });
                return { ok: true, fallback: true };
            } catch (err2) {
                console.error('saveCandidateTest: fallback save failed', err2);
                // Surface helpful message to the user
                const code = (err2 && err2.code) ? err2.code : null;
                const msg = (err2 && err2.message) ? err2.message : String(err2);
                alert('Kaydetme başarısız oldu: ' + (code || msg));
                // If permission denied, give explicit guidance
                if ((code && code.toLowerCase().includes('permission')) || (msg && msg.toLowerCase().includes('permission'))) {
                    alert('Görünüşe göre Realtime Database kuralları yazmaya izin vermiyor (permission_denied).\n\nÇözüm önerileri:\n- Firebase Console -> Realtime Database -> Rules bölümünden geçici olarak test kuralları uygulayarak deneyin.\n- Ya da adayları Auth ile giriş yaptırıp UID temelli yazma izni sağlayın.\n- Üretimde kesinlikle kuralları herkese açık yapmayın.');
                }
                throw err2;
            }
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
        // cevaplar: array of answers (strings or numbers)
        const nums = (cevaplar || []).map(mapAnswerToNumber).filter(n => typeof n === 'number');
        const n = nums.length || 1;
        const mean = nums.reduce((a,b)=>a+b,0)/n;
        const variance = nums.reduce((a,b)=>a + Math.pow(b-mean,2),0)/n;
        const std = Math.sqrt(variance);
        // Response bias: scale std to 0-100
        const bias = Math.round(Math.min(100, std * 25 * 10)/10 * 10)/10 || 0;

        // Radar: split answers into 5 buckets (simple heuristic) and normalize to 0-100
        const buckets = [0,0,0,0,0];
        const counts = [0,0,0,0,0];
        for (let i=0;i<nums.length;i++){
            const bi = i % 5;
            buckets[bi] += nums[i]; counts[bi]++;
        }
        const radar = buckets.map((sum,idx) => {
            const cnt = counts[idx]||1;
            const avg = sum / cnt; // 1..5
            return Math.round(((avg - 1) / 4) * 100); // normalize to 0-100
        });

        // SJT average heuristic: if meta.baslik contains SJT-like keywords, treat as sjt
        const sjtKeywords = ['sjt','durumsal','problem','senaryo'];
        const isSjt = (meta.baslik||'').toLowerCase().split(' ').some(w=>sjtKeywords.includes(w)) || false;
        const sjtAvg = isSjt ? Math.round((mean/5)*100) : null;

        // overall (weighted) score - simple average of radar
        const genelSkor = Math.round((radar.reduce((a,b)=>a+b,0)/radar.length) * 10) / 10;

        const labels = ['İletişim','Liderlik','Teknik','Vicdanlılık','Adaptasyon'];
        const labeled = radar.map((val,i)=>({k: labels[i]||('K'+i), v: val}));
        const sorted = labeled.slice().sort((a,b)=>b.v-a.v);
        const top3Strong = sorted.slice(0,3).map(x=>x.k);
        const top3Weak = sorted.slice(-3).map(x=>x.k);

        // NLG - rule based
        const nlgParts = [];
        if (labeled[1].v > 85 && (sjtAvg===null || sjtAvg > 80)) {
            nlgParts.push('Adayın liderlik ve karar verme yetkinliği oldukça yüksektir. Ekip lideri rolü için potansiyel taşımaktadır.');
        }
        if (labeled[0].v < 60 && labeled[4].v < 50) {
            nlgParts.push('Aday, iletişim ve adaptasyon alanlarında gelişime ihtiyaç göstermektedir; müşteri temelli roller için dikkatli değerlendirme önerilir.');
        }
        if (bias > 30) nlgParts.push('Güvenilirlik puanı yüksek; sonuçlar manipülasyon riski taşıyabilir. Mülakat sırasında tutarlılık sorgulanmalıdır.');
        if (!nlgParts.length) nlgParts.push('Adayın profili dengeli; dikkat edilmesi gereken belirgin bir risk tespit edilmedi.');

        return {
            radar,
            sjt: sjtAvg ? [sjtAvg] : [],
            bias,
            genelSkor,
            top3Strong,
            top3Weak,
            nlgSummary: nlgParts.join(' ')
        };
    }

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
        // user: {username, fullName, phone, email, password, active}
        if (!user || !user.username) throw new Error('username required');
        await set(ref(db, 'hrUsers/' + user.username), {
            username: user.username,
            fullName: user.fullName || '',
            phone: user.phone || '',
            email: user.email || '',
            password: user.password || '',
            active: user.active === undefined ? true : !!user.active,
            createdAt: Date.now()
        });
    };

    window.listHRUsers = function(callback) {
        const hrRef = ref(db, 'hrUsers');
        onValue(hrRef, (snapshot) => {
            const data = snapshot.val() || {};
            const arr = Object.keys(data).map(k => data[k]);
            callback(arr);
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
                // Try to create Auth user if available
                let createdAuth = false;
                try {
                    const userCred = await window.createUserWithEmailAndPassword(window.firebaseAuth, email, password);
                    createdAuth = !!(userCred && userCred.user && userCred.user.uid);
                } catch(authErr) {
                    console.warn('Auth createUser failed (will fallback to DB-only).', authErr);
                    // If auth not configured (operation-not-allowed / configuration-not-found), we'll fallback
                }

                // Write hrUsers entry in DB (keep username as local part before @ or uid)
                const username = email.split('@')[0];
                await set(ref(db, 'hrUsers/' + username), {
                    username,
                    fullName: '',
                    phone: '',
                    email,
                    // Do NOT store plain password in production. For now keep null to avoid leaks.
                    password: null,
                    active: true,
                    createdAt: Date.now(),
                    authCreated: createdAuth
                });

                if (createdAuth) {
                    alert('Kayıt başarılı! İK hesabınız oluşturuldu. Yönetici onayı sonrası giriş yapabilirsiniz.');
                } else {
                    alert('Kayıt başarıyla veritabanına yazıldı, ancak Firebase Auth oluşturulamadı (Auth yapılandırması eksik).\nLütfen Firebase Console -> Authentication -> Email/Password etkinleştir veya yöneticiden Auth hesabı oluşturmasını isteyin.');
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
                await update(ref(db, 'candidates/' + (it.rumuz || ('pending_' + it.ts))), {
                    rumuz: it.rumuz || null,
                    tip: it.tip || null,
                    baslik: it.baslik || null,
                    cevaplar: it.cevaplar || [],
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
                if (!idToken || !idToken.claims || !idToken.claims.hr) {
                    await window.signOutFirebase(window.firebaseAuth);
                    alert('Bu hesap İK yetkisine sahip değil.');
                    return;
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
            const emailEl = document.getElementById('adminUsername');
            const passEl = document.getElementById('adminPassword');
            const email = emailEl ? emailEl.value.trim() : '';
            const password = passEl ? passEl.value : '';
            const overlay = document.getElementById('loadingOverlay'); if (overlay) overlay.classList.remove('hidden');
            try {
                if (!email || !password) { alert('E-posta ve şifre girin'); return; }
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
                // render questions according to stored tip/baslik
                renderTestQuestions(cand.tip, cand.baslik);
                testSection.classList.remove('hidden');
            } catch (err) {
                console.error(err);
                alert('Giriş sırasında hata: ' + (err.message||err));
            }
        });
    }

    // Test sorularını dinamik oluştur
    function renderTestQuestions(tip, baslik) {
        const testForm = document.getElementById('testForm');
        if (!testForm) return;
        testForm.innerHTML = '';
        const questions = (questionPool[tip] && questionPool[tip][baslik]) || [];
        questions.forEach((q, i) => {
            testForm.innerHTML += `
                <div>
                    <label class='block text-gray-700 font-medium mb-2'>${i+1}. ${q}</label>
                    <select class='w-full px-4 py-2 border rounded-lg' required>
                        <option value=''>Seçiniz</option>
                        <option value='evet'>Evet</option>
                        <option value='hayir'>Hayır</option>
                        <option value='bazen'>Bazen</option>
                    </select>
                </div>
            `;
        });
        testForm.innerHTML += `<button type='submit' class='w-full bg-blue-700 text-white py-2 rounded-lg mt-4 hover:bg-blue-800'>Cevapları Gönder</button>`;
    }

    // Test cevaplarını kaydet ve İK paneline yansıt (Firebase ile tam entegrasyon)
    const testForm = document.getElementById('testForm');
    if (testForm) {
        testForm.onsubmit = async function(e) {
            e.preventDefault();
            if (!activeCandidate) return;
            const selects = testForm.querySelectorAll('select');
            const cevaplar = Array.from(selects).map(s => s.value);
            // Demo skorlar (gerçek hesaplama ile değiştirilebilir)
            const skorlar = {
                radar: [Math.random()*100, Math.random()*100, Math.random()*100, Math.random()*100, Math.random()*100],
                sjt: [Math.random()*100, Math.random()*100, Math.random()*100, Math.random()*100, Math.random()*100],
                bias: (80 + Math.random()*20).toFixed(1)
            };
            // Firebase'e kaydet
            await window.saveCandidateTest(activeCandidate.rumuz, activeCandidate.tip, activeCandidate.baslik, cevaplar, skorlar);
            alert('Cevaplarınız kaydedildi!');
            // İK panelindeki cevap listesine yansıt
            renderAnswers({cevaplar});
            renderRadar(skorlar.radar);
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
                candidateList.innerHTML += `
                    <tr>
                        <td class='p-2'>${c.rumuz}</td>
                        <td class='p-2'>${c.tip}</td>
                        <td class='p-2'>${c.baslik}</td>
                        <td class='p-2'>
                            <button class='px-2 py-1 border view-detail' data-r='${c.rumuz}'>Detay</button>
                            <button class='px-2 py-1 border ml-2 send-creds' data-r='${c.rumuz}' data-p='${c.password||''}'>Kimlik Gönder</button>
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
            const email = document.getElementById('adminEmailCompact').value.trim();
            const pw = document.getElementById('adminPasswordCompact').value;
            const overlay = document.getElementById('loadingOverlay'); if (overlay) overlay.classList.remove('hidden');
            try {
                if (!email || !pw) { alert('E-posta ve şifre girin'); return; }
                // Use exposed signInWithEmailAndPassword (from module scope)
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
    if (addCandidateForm) {
        addCandidateForm.onsubmit = async function(e) {
            e.preventDefault();
            const rumuz = document.getElementById('newCandidateNickname').value.trim();
            const sifre = document.getElementById('newCandidatePassword').value;
            const tip = document.getElementById('candidateType').value;
            const baslik = document.getElementById('questionCategory').value;
            if (!rumuz) { alert('Rumuz giriniz'); return; }
            try {
                await window.addCandidateToDB({ rumuz, password: sifre, tip, baslik, createdBy: window.currentHR || 'admin' });
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
            if (!username || !email || !password) {
                alert('Kullanıcı adı, e-posta ve şifre zorunludur.');
                return;
            }
            // Use Firebase helper
            addHRUser({username, fullName, phone, email, password, active: true}).then(() => {
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
                hrManageTableBody.innerHTML += `<tr>
                    <td class='p-2'>${u.username}</td>
                    <td class='p-2'>${u.fullName||''}</td>
                    <td class='p-2'>${u.phone||''}</td>
                    <td class='p-2'>${u.email||''}</td>
                    <td class='p-2'>${u.password||''}</td>
                    <td class='p-2'>${testCount}</td>
                    <td class='p-2'>${status}</td>
                    <td class='p-2'><button class='px-2 py-1 border toggle-hr' data-user='${u.username}'>${u.active? 'Pasife Al' : 'Aktifleştir'}</button></td>
                    <td class='p-2'><button class='px-2 py-1 bg-indigo-600 text-white rounded request-nlg' data-hr='${u.username}'>Gemini Özeti</button></td>
                </tr>`;
            }
            // attach toggles
            Array.from(document.querySelectorAll('.toggle-hr')).forEach(btn => {
                btn.onclick = async function(){
                    const username = this.dataset.user;
                    // flip
                    const current = this.textContent.includes('Pasife')? true : false;
                    const newActive = !current;
                    await setHRActive(username, newActive);
                    loadHRList();
                }
            });
        });
    }
    if (refreshHrListBtn) refreshHrListBtn.onclick = loadHRList;
    if (applyFilterBtn) applyFilterBtn.onclick = loadHRList;
    
    // Attach handler for Gemini NLG request buttons
    document.addEventListener('click', function(e){
        if (e.target && e.target.classList && e.target.classList.contains('request-nlg')){
            const username = e.target.dataset.hr;
            if (!username) return alert('Kullanıcı seçili değil');
            // ask confirmation
            if (!confirm(`'${username}' için Gemini ile özet oluşturulsun mu?`)) return;
            requestCandidateNLG(username).then(result => {
                alert('Gemini özeti oluşturuldu ve kaydedildi.');
                // refresh candidate list/details
                loadCandidatesFromFirebase();
                if (!document.getElementById('hrManageModal').classList.contains('hidden')) loadHRList();
            }).catch(err => {
                console.error(err);
                alert('Gemini özeti oluşturulurken hata: ' + (err.message||err));
            });
        }
    });

    // Call the generateCandidateNLG cloud function for a given candidate username
    async function requestCandidateNLG(rumuz){
        // function URL and secret can be configured in the global window for convenience
        const fnUrl = window.GENERATE_NLG_URL || prompt('Fonksiyon URL (ör: https://REGION-PROJECT.cloudfunctions.net/generateCandidateNLG):');
        if (!fnUrl) throw new Error('Fonksiyon URL girilmedi');
        const secret = window.APX_SECRET || prompt('APX secret (x-apx-secret başlığı):');
        if (!secret) throw new Error('APX secret girilmedi');

        // store for convenience in this session
        window.GENERATE_NLG_URL = fnUrl;
        window.APX_SECRET = secret;

        const res = await fetch(fnUrl, {
            method: 'POST',
            headers: {
                'Content-Type': 'application/json',
                'x-apx-secret': secret
            },
            body: JSON.stringify({ rumuz })
        });
        if (!res.ok) {
            const txt = await res.text();
            throw new Error('Fonksiyon hatası: ' + txt);
        }
        return res.json();
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
                candidateList.innerHTML += `<tr><td class='p-2'>${c.rumuz}</td><td class='p-2'>${c.tip}</td><td class='p-2'>${c.baslik}</td></tr>`;
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
  modal.className = 'fixed inset-0 bg-black bg-opacity-60 flex items-center justify-center z-[99999]';
  modal.innerHTML = `
    <div class='bg-white rounded-2xl shadow-2xl p-8 w-full max-w-2xl relative'>
      <button onclick='this.closest(".fixed").remove()' class='absolute top-4 right-4 text-gray-400 hover:text-red-500 text-2xl'>&times;</button>
      <h2 class='text-2xl font-bold text-blue-800 mb-4'>Aday Detay Analizi</h2>
      <div class='mb-4'><b>Rumuz:</b> ${candidate.rumuz} &nbsp; <b>Tip:</b> ${candidate.tip} &nbsp; <b>Başlık:</b> ${candidate.baslik}</div>
      <div class='mb-6 flex flex-col md:flex-row gap-6'>
        <div class='flex-1 bg-blue-50 rounded-lg p-4'>
          <canvas id='detailRadar' width='300' height='200'></canvas>
          <div class='text-xs text-gray-500 mt-2'>Big Five & Alt Yetkinlikler</div>
        </div>
        <div class='flex-1 bg-blue-50 rounded-lg p-4'>
          <h4 class='font-semibold mb-2'>SJT Performansı</h4>
          <canvas id='detailSJT' width='300' height='200'></canvas>
          <div class='text-xs text-gray-500 mt-2'>Durumsal Yargı Testi</div>
        </div>
      </div>
      <div class='mb-4'>
        <b>Güvenilirlik Puanı (Response Bias):</b> <span id='detailBias'></span>
      </div>
      <div>
        <h4 class='font-semibold mb-2'>Cevapladığı Sorular</h4>
        <ul id='detailAnswers' class='list-disc pl-5 text-gray-700'></ul>
      </div>
    </div>
  `;
  document.body.appendChild(modal);
  // Radar ve SJT grafikleri demo (örnek puanlar)
        setTimeout(() => {
            // Prefer database-provided skorlar if available
            const s = candidate.skorlar || {};
            const radarData = (s.radar && s.radar.length) ? s.radar : [Math.random()*100, Math.random()*100, Math.random()*100, Math.random()*100, Math.random()*100];
            const sjtData = (s.sjt && s.sjt.length) ? s.sjt : [Math.random()*100, Math.random()*100, Math.random()*100, Math.random()*100, Math.random()*100];

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
                new Chart(document.getElementById('detailSJT').getContext('2d'), {
                    type: 'bar',
                    data: {
                        labels: ['Senaryo 1','Senaryo 2','Senaryo 3','Senaryo 4','Senaryo 5'],
                        datasets: [{
                            label: 'SJT Skoru',
                            data: sjtData,
                            backgroundColor: 'rgba(16,185,129,0.5)'
                        }]
                    },
                    options: {responsive: false, plugins: {legend: {display: false}}}
                });
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
                // create button to request Gemini NLG
                const btn = document.createElement('button');
                btn.className = 'mt-3 inline-block bg-indigo-600 text-white px-3 py-1 rounded hover:bg-indigo-700';
                btn.innerText = 'Yapay Zeka Yorumu İste';
                btn.onclick = async function(){
                    try {
                        summaryHolder.innerText = 'Oluşturuluyor... Lütfen bekleyin.';
                        const resp = await requestCandidateNLG(candidate.rumuz);
                        // resp may be { rumuz, result }
                        const text = resp && resp.result ? resp.result : (typeof resp === 'string' ? resp : JSON.stringify(resp));
                        summaryHolder.innerText = text || 'Gemini döndü fakat metin alınamadı.';
                    } catch (err) {
                        console.error('NLG isteği başarısız', err);
                        summaryHolder.innerText = 'NLG isteği sırasında hata oluştu: ' + (err.message||err);
                    }
                };
                container.appendChild(btn);
                modal.querySelector('.bg-white')?.appendChild(container);
                summaryEl = document.getElementById(summaryElId);
            }
            summaryEl.innerText = s.nlgSummary || 'Detaylı yorum bulunmuyor.';

            // Cevap listesi
            const ans = document.getElementById('detailAnswers');
            ans.innerHTML = '';
            if (candidate.cevaplar && candidate.cevaplar.length) {
                candidate.cevaplar.forEach((cevap, i) => {
                    ans.innerHTML += `<li>${i+1}. Soru: ${cevap}</li>`;
                });
            } else {
                ans.innerHTML = '<li>Henüz cevap yok.</li>';
            }
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
            } catch (e) {
                console.warn('Modal taşıma sırasında hata:', e);
            }
        });
        </script>
</body>
</html>
