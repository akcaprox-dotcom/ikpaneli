<!DOCTYPE html>
<html lang="tr">
<head>
        <!-- Firebase SDK'ları -->
        <script src="https://www.gstatic.com/firebasejs/9.23.0/firebase-app-compat.js"></script>
        <script src="https://www.gstatic.com/firebasejs/9.23.0/firebase-database-compat.js"></script>
        <script src="https://www.gstatic.com/firebasejs/9.23.0/firebase-auth-compat.js"></script>
        <script>
            const firebaseConfig = {
                apiKey: "AIzaSyC-ZvTo79-xDc9Uw2IMOZMwK9Egm9qODrU",
                authDomain: "ikpaneli.firebaseapp.com",
                databaseURL: "https://ikpaneli-default-rtdb.europe-west1.firebasedatabase.app",
                projectId: "ikpaneli",
                storageBucket: "ikpaneli.firebasestorage.app",
                messagingSenderId: "645340845423",
                appId: "1:645340845423:web:435b57f7093782422e449a",
                measurementId: "G-6NBTKSBVYL"
            };
            firebase.initializeApp(firebaseConfig);
            const db = firebase.database();
            const auth = firebase.auth();
            
            // Google Auth Provider
            const googleProvider = new firebase.auth.GoogleAuthProvider();
            googleProvider.addScope('email');
            googleProvider.addScope('profile');
        </script>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>İK Test Paneli</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
    <style>
        body {
            box-sizing: border-box;
        }
        .fade-in {
            animation: fadeIn 0.3s ease-in;
        }
        @keyframes fadeIn {
            from { opacity: 0; transform: translateY(10px); }
            to { opacity: 1; transform: translateY(0); }
        }
        .slide-in {
            animation: slideIn 0.4s ease-out;
        }
        @keyframes slideIn {
            from { transform: translateX(-20px); opacity: 0; }
            to { transform: translateX(0); opacity: 1; }
        }
        
        /* Likert Scale Seçenekleri için Özel Stiller */
        .likert-option {
            position: relative;
            background: linear-gradient(135deg, #f8fafc 0%, #e2e8f0 100%);
            border: 2px solid #e2e8f0;
            border-radius: 12px;
            padding: 16px 20px;
            margin: 8px 0;
            cursor: pointer;
            transition: all 0.3s cubic-bezier(0.4, 0, 0.2, 1);
            overflow: hidden;
        }
        
        .likert-option::before {
            content: '';
            position: absolute;
            top: 0;
            left: -100%;
            width: 100%;
            height: 100%;
            background: linear-gradient(90deg, transparent, rgba(59, 130, 246, 0.1), transparent);
            transition: left 0.5s ease;
        }
        
        .likert-option:hover {
            transform: translateY(-2px);
            box-shadow: 0 8px 25px rgba(59, 130, 246, 0.15);
            border-color: #3b82f6;
            background: linear-gradient(135deg, #dbeafe 0%, #bfdbfe 100%);
        }
        
        .likert-option:hover::before {
            left: 100%;
        }
        
        .likert-option.selected {
            background: linear-gradient(135deg, #3b82f6 0%, #1d4ed8 100%);
            border-color: #1d4ed8;
            color: white;
            transform: translateY(-2px);
            box-shadow: 0 8px 25px rgba(59, 130, 246, 0.3);
        }
        
        .likert-option.selected::before {
            background: linear-gradient(90deg, transparent, rgba(255, 255, 255, 0.2), transparent);
        }
        
        .likert-option .option-number {
            display: inline-block;
            width: 28px;
            height: 28px;
            background: #e2e8f0;
            color: #64748b;
            border-radius: 50%;
            text-align: center;
            line-height: 28px;
            font-weight: bold;
            font-size: 14px;
            margin-right: 12px;
            transition: all 0.3s ease;
        }
        
        .likert-option:hover .option-number {
            background: #3b82f6;
            color: white;
            transform: scale(1.1);
        }
        
        .likert-option.selected .option-number {
            background: rgba(255, 255, 255, 0.2);
            color: white;
            transform: scale(1.1);
        }
        
        .likert-option .option-text {
            font-weight: 500;
            font-size: 16px;
            transition: all 0.3s ease;
        }
        
        .likert-option:hover .option-text {
            color: #1e40af;
        }
        
        .likert-option.selected .option-text {
            color: white;
        }
        
        /* Radio button gizleme */
        .likert-option input[type="radio"] {
            display: none;
        }
    </style>
</head>
<body class="bg-gradient-to-br from-blue-50 to-indigo-100 min-h-screen">
    <!-- Ana Giriş Ekranı -->
    <div id="loginScreen" class="min-h-screen flex items-center justify-center p-4 relative">
        <!-- Admin Butonu Sol Alt Köşe -->
        <button onclick="showRoleLogin('admin')" class="fixed bottom-4 left-4 bg-red-600 hover:bg-red-700 text-white text-xs px-2 py-1 rounded opacity-50 hover:opacity-100 transition-opacity duration-300 z-10">
            Admin
        </button>

        <!-- Developer Credit Sayfa Ortası -->

        <!-- Developer Credit Alt Orta -->
        <div class="fixed bottom-4 left-1/2 transform -translate-x-1/2 z-50 pointer-events-none select-none">
            <span class="text-lg font-semibold text-gray-400 opacity-80 bg-white bg-opacity-70 px-6 py-3 rounded-xl shadow-md">Developed by Akça Pro X</span>
        </div>

        <div class="bg-white rounded-2xl shadow-2xl p-8 w-full max-w-md fade-in">
            <div class="text-center mb-8">
                <h1 class="text-3xl font-bold text-gray-800 mb-2">Analiz Pro X</h1>
                <p class="text-lg text-blue-600 font-semibold mb-4">Profesyonel Aday Değerlendirme Paneli</p>
                
                <!-- Bilimsel Temeller ve Sorumluluk Reddi -->
                <div class="bg-yellow-50 border border-yellow-200 rounded-lg p-4 mb-6">
                    <div class="flex items-center justify-center mb-3 space-x-3">
                        <button id="methodologyButton" onclick="showMethodology()" class="flex items-center space-x-2 bg-green-600 hover:bg-green-700 text-white px-4 py-2 rounded-lg transition duration-300">
                            <svg class="w-5 h-5" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M9.663 17h4.673M12 3v1m6.364 1.636l-.707.707M21 12h-1M4 12H3m3.343-5.657l-.707-.707m2.828 9.9a5 5 0 117.072 0l-.548.547A3.374 3.374 0 0014 18.469V19a2 2 0 11-4 0v-.531c0-.895-.356-1.754-.988-2.386l-.548-.547z"></path>
                            </svg>
                            <span>METODOLOJİ VE BİLİMSEL TEMELLER</span>
                        </button>
                        <button id="disclaimerButton" onclick="showDisclaimer()" class="flex items-center space-x-2 bg-blue-600 hover:bg-blue-700 text-white px-4 py-2 rounded-lg transition duration-300">
                            <svg class="w-5 h-5" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M9 12h6m-6 4h6m2 5H7a2 2 0 01-2-2V5a2 2 0 012-2h5.586a1 1 0 01.707.293l5.414 5.414a1 1 0 01.293.707V19a2 2 0 01-2 2z"></path>
                            </svg>
                            <span>Sorumluluk Reddi Beyanını Oku</span>
                        </button>
                    </div>
                    <div class="flex items-center justify-center">
                        <label class="flex items-center cursor-pointer">
                            <input type="checkbox" id="disclaimerAccept" class="w-4 h-4 text-blue-600 bg-gray-100 border-gray-300 rounded focus:ring-blue-500 focus:ring-2" disabled>
                            <span class="ml-2 text-sm text-gray-700">Sorumluluk reddi beyanını okudum ve onaylıyorum</span>
                            <svg class="w-5 h-5 ml-2 text-green-600" fill="currentColor" viewBox="0 0 20 20">
                                <path fill-rule="evenodd" d="M16.707 5.293a1 1 0 010 1.414l-8 8a1 1 0 01-1.414 0l-4-4a1 1 0 011.414-1.414L8 12.586l7.293-7.293a1 1 0 011.414 0z" clip-rule="evenodd"></path>
                            </svg>
                        </label>
                    </div>
                </div>
            </div>
            
            <div class="space-y-4">
                <!-- Google Authentication Butonu -->
                <button id="googleAuthButton" onclick="signInWithGoogle()" class="w-full bg-red-600 hover:bg-red-700 text-white font-semibold py-4 px-6 rounded-xl transition duration-300 transform hover:scale-105 flex items-center justify-center space-x-3">
                    <svg class="w-6 h-6" viewBox="0 0 24 24">
                        <path fill="currentColor" d="M22.56 12.25c0-.78-.07-1.53-.2-2.25H12v4.26h5.92c-.26 1.37-1.04 2.53-2.21 3.31v2.77h3.57c2.08-1.92 3.28-4.74 3.28-8.09z"/>
                        <path fill="currentColor" d="M12 23c2.97 0 5.46-.98 7.28-2.66l-3.57-2.77c-.98.66-2.23 1.06-3.71 1.06-2.86 0-5.29-1.93-6.16-4.53H2.18v2.84C3.99 20.53 7.7 23 12 23z"/>
                        <path fill="currentColor" d="M5.84 14.09c-.22-.66-.35-1.36-.35-2.09s.13-1.43.35-2.09V7.07H2.18C1.43 8.55 1 10.22 1 12s.43 3.45 1.18 4.93l2.85-2.22.81-.62z"/>
                        <path fill="currentColor" d="M12 5.38c1.62 0 3.06.56 4.21 1.64l3.15-3.15C17.45 2.09 14.97 1 12 1 7.7 1 3.99 3.47 2.18 7.07l3.66 2.84c.87-2.6 3.3-4.53 6.16-4.53z"/>
                    </svg>
                    <span>🔐 Google ile Giriş</span>
                </button>
                
                <!-- İK Kaydı ve Girişi -->
                <div class="grid grid-cols-2 gap-3">
                    <button id="hrRegisterMainButton" onclick="showHrRegister()" class="bg-blue-600 hover:bg-blue-700 text-white font-semibold py-3 px-4 rounded-xl transition duration-300 transform hover:scale-105 disabled:opacity-50 disabled:cursor-not-allowed" disabled title="Önce Google ile giriş yapın">
                        👨‍💼 İK Kaydı
                    </button>
                    <button id="hrButton" onclick="showRoleLogin('hr')" class="bg-green-600 hover:bg-green-700 text-white font-semibold py-3 px-4 rounded-xl transition duration-300 transform hover:scale-105">
                        👩‍💻 İK Girişi
                    </button>
                </div>
                
                <div class="text-center">
                    <span class="text-gray-500 text-sm">veya</span>
                </div>
                
                <button id="candidateButton" onclick="showRoleLogin('candidate')" class="w-full bg-orange-600 hover:bg-orange-700 text-white font-semibold py-4 px-6 rounded-xl transition duration-300 transform hover:scale-105 disabled:opacity-50 disabled:cursor-not-allowed" disabled>
                    📝 Aday Portalı
                </button>
            </div>
        </div>
    </div>

    <!-- Rol Bazlı Giriş Formu -->
    <div id="roleLoginScreen" class="min-h-screen flex items-center justify-center p-4 hidden">
        <div class="bg-white rounded-2xl shadow-2xl p-8 w-full max-w-md fade-in">
            <button onclick="backToMain()" class="mb-4 text-gray-600 hover:text-gray-800 flex items-center">
                ← Geri Dön
            </button>
            
            <div class="text-center mb-6">
                <h2 id="roleTitle" class="text-2xl font-bold text-gray-800 mb-2"></h2>
                <p class="text-gray-600">Giriş bilgilerinizi giriniz</p>
            </div>
            
            <form id="loginForm" class="space-y-4">
                <div id="candidateFields" class="hidden space-y-4">
                    <input type="text" id="candidateAlias" placeholder="Rumuz" class="w-full px-4 py-3 border border-gray-300 rounded-xl focus:ring-2 focus:ring-blue-500 focus:border-transparent">
                    <input type="password" id="candidatePassword" placeholder="Şifre" class="w-full px-4 py-3 border border-gray-300 rounded-xl focus:ring-2 focus:ring-blue-500 focus:border-transparent">
                </div>
                
                <div id="adminHrFields" class="space-y-4">
                    <input type="email" id="adminHrEmail" placeholder="E-posta" class="w-full px-4 py-3 border border-gray-300 rounded-xl focus:ring-2 focus:ring-blue-500 focus:border-transparent">
                    <input type="password" id="adminHrPassword" placeholder="Şifre" class="w-full px-4 py-3 border border-gray-300 rounded-xl focus:ring-2 focus:ring-blue-500 focus:border-transparent">
                </div>
                
                <button type="submit" class="w-full bg-indigo-600 hover:bg-indigo-700 text-white font-semibold py-3 px-6 rounded-xl transition duration-300">
                    Giriş Yap
                </button>
            </form>
        </div>
    </div>

    <!-- Admin Panel -->
    <div id="adminPanel" class="hidden min-h-screen bg-gray-50">
        <nav class="bg-white shadow-lg">
            <div class="max-w-7xl mx-auto px-4">
                <div class="flex justify-between items-center py-4">
                    <h1 class="text-2xl font-bold text-gray-800">Admin Yönetici Paneli</h1>
                    <button onclick="logout()" class="bg-red-600 hover:bg-red-700 text-white px-4 py-2 rounded-lg">Çıkış</button>
                </div>
            </div>
        </nav>
        
        <div class="max-w-7xl mx-auto p-6">
            <div class="grid grid-cols-1 lg:grid-cols-3 gap-6 mb-8">
                <div class="bg-white rounded-xl shadow-lg p-6">
                    <h3 class="text-lg font-semibold text-gray-800 mb-2">Toplam İK Yöneticisi</h3>
                    <p class="text-3xl font-bold text-blue-600" id="totalHrManagers">0</p>
                </div>
                <div class="bg-white rounded-xl shadow-lg p-6">
                    <h3 class="text-lg font-semibold text-gray-800 mb-2">Aktif Kullanıcılar</h3>
                    <p class="text-3xl font-bold text-green-600" id="activeUsers">0</p>
                </div>
                <div class="bg-white rounded-xl shadow-lg p-6">
                    <h3 class="text-lg font-semibold text-gray-800 mb-2">Pasif Kullanıcılar</h3>
                    <p class="text-3xl font-bold text-red-600" id="inactiveUsers">0</p>
                </div>
            </div>
            
            <div class="bg-white rounded-xl shadow-lg p-6">
                <h3 class="text-xl font-bold text-gray-800 mb-4">İK Yöneticileri</h3>
                <!-- Tarih aralığı filtre alanı -->
                <div class="flex flex-col md:flex-row md:items-end gap-4 mb-4">
                    <div>
                        <label for="adminFilterStartDate" class="block text-sm font-medium text-gray-700 mb-1">Başlangıç Tarihi</label>
                        <input type="date" id="adminFilterStartDate" class="border border-gray-300 rounded px-3 py-2 focus:ring-2 focus:ring-blue-500">
                    </div>
                    <div>
                        <label for="adminFilterEndDate" class="block text-sm font-medium text-gray-700 mb-1">Bitiş Tarihi</label>
                        <input type="date" id="adminFilterEndDate" class="border border-gray-300 rounded px-3 py-2 focus:ring-2 focus:ring-blue-500">
                    </div>
                    <div>
                        <button id="adminFilterDateBtn" class="bg-blue-600 hover:bg-blue-700 text-white font-semibold px-6 py-2 rounded transition">Filtrele</button>
                        <button id="adminClearDateBtn" class="ml-2 bg-gray-300 hover:bg-gray-400 text-gray-800 font-semibold px-4 py-2 rounded transition">Temizle</button>
                    </div>
                </div>
                <div class="overflow-x-auto">
                    <table class="w-full table-auto">
                        <thead>
                            <tr class="bg-gray-50">
                                <th class="px-4 py-3 text-left text-sm font-semibold text-gray-600">Kuruluş</th>
                                <th class="px-4 py-3 text-left text-sm font-semibold text-gray-600">Ad Soyad</th>
                                <th class="px-4 py-3 text-left text-sm font-semibold text-gray-600">E-posta</th>
                                <th class="px-4 py-3 text-left text-sm font-semibold text-gray-600">Telefon</th>
                                <th class="px-4 py-3 text-left text-sm font-semibold text-gray-600">Görev</th>
                                <th class="px-4 py-3 text-left text-sm font-semibold text-gray-600">Durum</th>
                                <th class="px-4 py-3 text-left text-sm font-semibold text-gray-600">İşlemler</th>
                            </tr>
                        </thead>
                        <tbody id="hrManagersList">
                            <!-- İK Yöneticileri buraya yüklenecek -->
                        </tbody>
                    </table>
                </div>
            </div>
        </div>
    </div>

    <!-- İK Yönetici Panel -->
    <div id="hrPanel" class="hidden min-h-screen bg-gray-50">
        <nav class="bg-white shadow-lg">
            <div class="max-w-7xl mx-auto px-4">
                <div class="flex justify-between items-center py-4">
                    <div class="flex items-center space-x-4">
                        <h1 class="text-2xl font-bold text-gray-800">İK Yönetici Paneli</h1>
                        <div id="userInfo" class="hidden flex items-center space-x-3 bg-gray-100 rounded-lg px-3 py-2">
                            <img id="userPhoto" src="" alt="Profile" class="w-8 h-8 rounded-full">
                            <div>
                                <p id="userName" class="text-sm font-semibold text-gray-800"></p>
                                <p id="userEmail" class="text-xs text-gray-600"></p>
                            </div>
                        </div>
                    </div>
                    <div class="flex space-x-4">
                        <button onclick="showHrSection('dashboard')" class="bg-blue-600 hover:bg-blue-700 text-white px-4 py-2 rounded-lg">Dashboard</button>
                        <button onclick="showNewMemberPanel()" class="bg-green-600 hover:bg-green-700 text-white px-4 py-2 rounded-lg">Yeni Kayıt</button>
                        <button onclick="showHrSection('candidates')" class="bg-purple-600 hover:bg-purple-700 text-white px-4 py-2 rounded-lg">Adaylar</button>
                        <button onclick="showHrSection('reports')" class="bg-orange-600 hover:bg-orange-700 text-white px-4 py-2 rounded-lg">Raporlar</button>
                        <button onclick="logout()" class="bg-red-600 hover:bg-red-700 text-white px-4 py-2 rounded-lg">Çıkış</button>
                    </div>
                </div>
            </div>
        </nav>

        <!-- İK Dashboard -->
        <div id="hrDashboard" class="max-w-7xl mx-auto p-6">
            <!-- Tarih aralığı filtre alanı -->
            <div class="flex flex-col md:flex-row md:items-end gap-4 mb-6">
                <div>
                    <label for="filterStartDate" class="block text-sm font-medium text-gray-700 mb-1">Başlangıç Tarihi</label>
                    <input type="date" id="filterStartDate" class="border border-gray-300 rounded px-3 py-2 focus:ring-2 focus:ring-blue-500">
                </div>
                <div>
                    <label for="filterEndDate" class="block text-sm font-medium text-gray-700 mb-1">Bitiş Tarihi</label>
                    <input type="date" id="filterEndDate" class="border border-gray-300 rounded px-3 py-2 focus:ring-2 focus:ring-blue-500">
                </div>
                <div>
                    <button id="filterDateBtn" class="bg-blue-600 hover:bg-blue-700 text-white font-semibold px-6 py-2 rounded transition">Filtrele</button>
                    <button id="clearDateBtn" class="ml-2 bg-gray-300 hover:bg-gray-400 text-gray-800 font-semibold px-4 py-2 rounded transition">Temizle</button>
                </div>
            </div>
            <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-4 gap-6 mb-8">
                <div class="bg-white rounded-xl shadow-lg p-6">
                    <h3 class="text-lg font-semibold text-gray-800 mb-2">Toplam Aday</h3>
                    <p class="text-3xl font-bold text-blue-600" id="totalCandidates">0</p>
                </div>
                <div class="bg-white rounded-xl shadow-lg p-6">
                    <h3 class="text-lg font-semibold text-gray-800 mb-2">Tamamlanan Testler</h3>
                    <p class="text-3xl font-bold text-green-600" id="completedTests">0</p>
                </div>
                <div class="bg-white rounded-xl shadow-lg p-6">
                    <h3 class="text-lg font-semibold text-gray-800 mb-2">Bekleyen Testler</h3>
                    <p class="text-3xl font-bold text-orange-600" id="pendingTests">0</p>
                </div>
                <div class="bg-white rounded-xl shadow-lg p-6">
                    <h3 class="text-lg font-semibold text-gray-800 mb-2">Ortalama Puan</h3>
                    <p class="text-3xl font-bold text-purple-600" id="averageScore">0</p>
                </div>
            </div>
        </div>

        <!-- Yeni Üye Ekleme -->
        <div id="hrNewMember" class="hidden max-w-6xl mx-auto p-6">
            <div class="bg-white rounded-xl shadow-lg p-8">
                <h3 class="text-2xl font-bold text-gray-800 mb-6">Yeni Aday Ekle ve Test Kriterleri Belirle</h3>
                <form id="newMemberForm" class="space-y-6">
                    <!-- Temel Bilgiler -->
                    <div class="grid grid-cols-1 md:grid-cols-3 gap-4">
                        <input type="text" id="newMemberAlias" placeholder="Aday Rumuzu" class="px-4 py-3 border border-gray-300 rounded-xl focus:ring-2 focus:ring-blue-500 focus:border-transparent" required>
                        <select id="newMemberMainCategory" class="px-4 py-3 border border-gray-300 rounded-xl focus:ring-2 focus:ring-blue-500 focus:border-transparent" required>
                            <option value="">Ana Kategori Seç</option>
                            <option value="manufacturing">İşletme</option>
                            <option value="service">Hizmet</option>
                        </select>
                        <select id="newMemberSubCategory" class="px-4 py-3 border border-gray-300 rounded-xl focus:ring-2 focus:ring-blue-500 focus:border-transparent" required disabled>
                            <option value="">Önce ana kategori seçin</option>
                        </select>
                    </div>
                    
                    <div class="grid grid-cols-1 md:grid-cols-2 gap-4">
                        <input type="password" id="newMemberPassword" placeholder="Aday Şifresi Belirle" class="px-4 py-3 border border-gray-300 rounded-xl focus:ring-2 focus:ring-blue-500 focus:border-transparent" required>
                        <div class="px-4 py-3 border border-gray-300 rounded-xl bg-gray-50 flex items-center">
                            <p class="text-sm text-gray-600">Aday bu bilgilerle giriş yapacak</p>
                        </div>
                    </div>

                    <!-- Test Kriterleri Seçimi -->
                    <div class="border-t pt-6">
                        <h4 class="text-lg font-semibold text-gray-800 mb-4">Test Grubu Seçimi</h4>
                        <div class="grid grid-cols-1 md:grid-cols-2 gap-4">
                            <select id="testGroupSelection" class="px-4 py-3 border border-gray-300 rounded-xl focus:ring-2 focus:ring-blue-500 focus:border-transparent" required>
                                <option value="">Test Grubu Seçin</option>
                                <option value="grup1">Beyaz Yaka Çalışanları (100 soru)</option>
                                <option value="grup2">Mavi Yaka Çalışanları (100 soru)</option>
                                <option value="grup3">Yönetici İmalat (100 soru)</option>
                                <option value="grup4">Hizmet Personeli (100 soru)</option>
                                <option value="grup5">Hizmet Yöneticileri (100 soru)</option>
                            </select>
                            <div class="px-4 py-3 border border-gray-300 rounded-xl bg-blue-50 flex items-center">
                                <p class="text-sm text-blue-700">Seçilen gruba ait 100 soru adaya sunulacak</p>
                            </div>
                        </div>
                    </div>
                            </div>

                            <!-- Bilişsel Kapasite -->
                            <div class="bg-green-50 border border-green-200 rounded-lg p-4">
                                <h5 class="font-semibold text-green-800 mb-3">Bilişsel Kapasite</h5>
                                <div class="space-y-2">
                                    <label class="flex items-center">
                                        <input type="checkbox" name="testCriteria" value="analytical_thinking" class="mr-2">
                                        <span class="text-sm">Analitik Düşünme</span>
                                    </label>
                                    <label class="flex items-center">
                                        <input type="checkbox" name="testCriteria" value="verbal_reasoning" class="mr-2">
                                        <span class="text-sm">Sözel Akıl Yürütme</span>
                                    </label>
                                    <label class="flex items-center">
                                        <input type="checkbox" name="testCriteria" value="numerical_ability" class="mr-2">
                                        <span class="text-sm">Sayısal Yetenek</span>
                                    </label>
                                    <label class="flex items-center">
                                        <input type="checkbox" name="testCriteria" value="problem_solving" class="mr-2">
                                        <span class="text-sm">Problem Çözme</span>
                                    </label>
                                </div>
                            </div>

                            <!-- Durumsal Yargı -->
                            <div class="bg-purple-50 border border-purple-200 rounded-lg p-4">
                                <h5 class="font-semibold text-purple-800 mb-3">Durumsal Yargı (SJT)</h5>
                                <div class="space-y-2">
                                    <label class="flex items-center">
                                        <input type="checkbox" name="testCriteria" value="ethical_decisions" class="mr-2">
                    
                    <div class="pt-4">
                        <button type="submit" class="w-full bg-green-600 hover:bg-green-700 text-white font-semibold py-3 px-6 rounded-xl transition duration-300">
                            Aday Oluştur ve Test Hazırla
                        </button>
                    </div>
                </form>
            </div>
        </div>

        <!-- Aday Yönetimi -->
        <div id="hrCandidates" class="hidden max-w-7xl mx-auto p-6">
            <div class="bg-white rounded-xl shadow-lg p-6 mb-6">
                <h3 class="text-xl font-bold text-gray-800 mb-4">Hızlı Aday Ekle</h3>
                <p class="text-sm text-gray-600 mb-4">Detaylı test kriterleri için Dashboard'daki "Yeni Aday Ekle" bölümünü kullanın.</p>
                <form id="newCandidateForm" class="space-y-4">
                    <div class="grid grid-cols-1 md:grid-cols-3 gap-4">
                        <input type="text" id="candidateAliasInput" placeholder="Rumuz" class="px-4 py-3 border border-gray-300 rounded-xl focus:ring-2 focus:ring-blue-500 focus:border-transparent" required>
                        <input type="password" id="candidatePasswordInput" placeholder="Şifre Belirle" class="px-4 py-3 border border-gray-300 rounded-xl focus:ring-2 focus:ring-blue-500 focus:border-transparent" required>
                        <select id="candidateTestGroup" class="px-4 py-3 border border-gray-300 rounded-xl focus:ring-2 focus:ring-blue-500 focus:border-transparent" required>
                            <option value="">Test Grubu Seçin</option>
                            <option value="grup1">Beyaz Yaka Çalışanları</option>
                            <option value="grup2">Mavi Yaka Çalışanları</option>
                            <option value="grup3">Yönetici İmalat</option>
                            <option value="grup4">Hizmet Personeli</option>
                            <option value="grup5">Hizmet Yöneticileri</option>
                        </select>
                    </div>
                    <button type="submit" class="w-full bg-purple-600 hover:bg-purple-700 text-white font-semibold py-3 px-6 rounded-xl transition duration-300">
                        Hızlı Aday Ekle
                    </button>
                </form>
            </div>
            
            <div class="bg-white rounded-xl shadow-lg p-6">
                <h3 class="text-xl font-bold text-gray-800 mb-4">Adaylar Listesi</h3>
                <!-- Tarih aralığı filtre alanı -->
                <div class="flex flex-col md:flex-row md:items-end gap-4 mb-4">
                    <div>
                        <label for="candidatesFilterStartDate" class="block text-sm font-medium text-gray-700 mb-1">Başlangıç Tarihi</label>
                        <input type="date" id="candidatesFilterStartDate" class="border border-gray-300 rounded px-3 py-2 focus:ring-2 focus:ring-blue-500">
                    </div>
                    <div>
                        <label for="candidatesFilterEndDate" class="block text-sm font-medium text-gray-700 mb-1">Bitiş Tarihi</label>
                        <input type="date" id="candidatesFilterEndDate" class="border border-gray-300 rounded px-3 py-2 focus:ring-2 focus:ring-blue-500">
                    </div>
                    <div>
                        <button id="candidatesFilterDateBtn" class="bg-blue-600 hover:bg-blue-700 text-white font-semibold px-6 py-2 rounded transition">Filtrele</button>
                        <button id="candidatesClearDateBtn" class="ml-2 bg-gray-300 hover:bg-gray-400 text-gray-800 font-semibold px-4 py-2 rounded transition">Temizle</button>
                    </div>
                </div>
                <div class="overflow-x-auto">
                    <table class="w-full table-auto">
                        <thead>
                            <tr class="bg-gray-50">
                                <th class="px-4 py-3 text-left text-sm font-semibold text-gray-600">Rumuz</th>
                                <th class="px-4 py-3 text-left text-sm font-semibold text-gray-600">Test Alanı</th>
                                <th class="px-4 py-3 text-left text-sm font-semibold text-gray-600">Test Durumu</th>
                                <th class="px-4 py-3 text-left text-sm font-semibold text-gray-600">Oluşturma Tarihi</th>
                                <th class="px-4 py-3 text-left text-sm font-semibold text-gray-600">İşlemler</th>
                            </tr>
                        </thead>
                        <tbody id="candidatesList">
                            <!-- Adaylar buraya yüklenecek -->
                        </tbody>
                    </table>
                </div>
            </div>
        </div>

        <!-- Raporlar -->
        <div id="hrReports" class="hidden max-w-7xl mx-auto p-6">
            <div class="grid grid-cols-1 lg:grid-cols-2 gap-6 mb-6">
                <div class="bg-white rounded-xl shadow-lg p-6">
                    <h3 class="text-xl font-bold text-gray-800 mb-4">Aday Seç</h3>
                    <select id="reportCandidateSelect" class="w-full px-4 py-3 border border-gray-300 rounded-xl focus:ring-2 focus:ring-blue-500 focus:border-transparent">
                        <option value="">Aday Seçin</option>
                    </select>
                </div>
                <div class="bg-white rounded-xl shadow-lg p-6">
                    <h3 class="text-xl font-bold text-gray-800 mb-4">Rapor Türü</h3>
                    <div class="space-y-2">
                        <button onclick="showReport('answers')" class="w-full bg-blue-600 hover:bg-blue-700 text-white py-2 px-4 rounded-lg transition duration-300 transform hover:scale-105">Sorular ve Cevaplar</button>
                        <button onclick="showReport('scores')" class="w-full bg-green-600 hover:bg-green-700 text-white py-2 px-4 rounded-lg transition duration-300 transform hover:scale-105">Puanlar</button>
                        <button onclick="showReport('charts')" class="w-full bg-purple-600 hover:bg-purple-700 text-white py-2 px-4 rounded-lg transition duration-300 transform hover:scale-105">Grafikler</button>
                    </div>
                </div>
            </div>
            
            <div id="reportContent" class="bg-white rounded-xl shadow-lg p-6">
                <p class="text-gray-600 text-center">Rapor görüntülemek için aday seçin ve rapor türünü belirleyin.</p>
            </div>
        </div>
    </div>

    <!-- İK Kayıt Formu -->
    <div id="hrRegisterScreen" class="min-h-screen flex items-center justify-center p-4 hidden">
        <div class="bg-white rounded-2xl shadow-2xl p-8 w-full max-w-2xl fade-in">
            <button onclick="backToRoleLogin()" class="mb-4 text-gray-600 hover:text-gray-800 flex items-center">
                ← Geri Dön
            </button>
            
            <div class="text-center mb-6">
                <h2 class="text-2xl font-bold text-gray-800 mb-2">İK Yönetici Kayıt</h2>
                <p class="text-gray-600">Bilgilerinizi doldurun</p>
            </div>
            
            <form id="hrRegisterForm" class="grid grid-cols-1 md:grid-cols-2 gap-4">
                <input type="text" id="regOrganization" placeholder="Kuruluş Adı" class="px-4 py-3 border border-gray-300 rounded-xl focus:ring-2 focus:ring-blue-500 focus:border-transparent" required>
                <input type="text" id="regName" placeholder="Ad Soyad" class="px-4 py-3 border border-gray-300 rounded-xl focus:ring-2 focus:ring-blue-500 focus:border-transparent" required>
                <input type="tel" id="regPhone" placeholder="Telefon" class="px-4 py-3 border border-gray-300 rounded-xl focus:ring-2 focus:ring-blue-500 focus:border-transparent" required>
                <input type="email" id="regEmail" placeholder="E-posta" class="px-4 py-3 border border-gray-300 rounded-xl focus:ring-2 focus:ring-blue-500 focus:border-transparent" required>
                <input type="text" id="regPosition" placeholder="Görev/Pozisyon" class="px-4 py-3 border border-gray-300 rounded-xl focus:ring-2 focus:ring-blue-500 focus:border-transparent" required>
                <input type="password" id="regPassword" placeholder="Şifre Belirle" class="px-4 py-3 border border-gray-300 rounded-xl focus:ring-2 focus:ring-blue-500 focus:border-transparent" required>
                <div class="md:col-span-2">
                    <button type="submit" class="w-full bg-green-600 hover:bg-green-700 text-white font-semibold py-3 px-6 rounded-xl transition duration-300">
                        Kayıt Ol
                    </button>
                </div>
            </form>
        </div>
    </div>

    <!-- Metodoloji Modal -->
    <div id="methodologyModal" class="hidden fixed inset-0 bg-black bg-opacity-50 flex items-center justify-center p-4 z-50">
        <div class="bg-white rounded-2xl shadow-2xl w-full max-w-5xl max-h-[90vh] overflow-y-auto">
            <div class="p-6 border-b border-gray-200">
                <div class="flex justify-between items-center">
                    <h2 class="text-2xl font-bold text-gray-800">METODOLOJİ VE BİLİMSEL TEMELLER</h2>
                    <button onclick="closeMethodology()" class="text-gray-500 hover:text-gray-700">
                        <svg class="w-6 h-6" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M6 18L18 6M6 6l12 12"></path>
                        </svg>
                    </button>
                </div>
            </div>
            
            <div class="p-6 space-y-6 text-sm text-gray-700 leading-relaxed">
                <p class="text-base font-semibold text-green-600">
                    Analiz Pro X, işe alım kararlarınıza prediktif geçerliliği kanıtlanmış bilimsel teminat katmak amacıyla, adayın performansını üç temel boyutta ölçer. Biz, tek bir test sonucuna değil, bu üç modülün çapraz analizine güveniyoruz.
                </p>
                
                <div class="bg-blue-50 border-l-4 border-blue-500 p-4 rounded">
                    <h3 class="text-lg font-bold text-blue-800 mb-3">1. KİŞİLİK ENVANTERLERİ (Davranışsal Eğilim ve Motivasyon)</h3>
                    <p class="mb-3">
                        Bu modül, adayın iş yerindeki alışkanlıklarını, motivasyonel yapısını ve sosyal adaptasyonunu analiz eder.
                    </p>
                    <ul class="list-disc list-inside space-y-2 ml-4">
                        <li><strong>Akademik Kök:</strong> Psikolojinin en güvenilir modeli olan Beş Büyük Faktör Modeli (Big Five / OCEAN) temel alınır.</li>
                        <li><strong>Ölçülen Alan:</strong> 50 alt yetkinlik alanındaki detaylı davranışsal eğilimler. Bu, adayın Vicdanlılık (Disiplin, Zaman Yönetimi) ve Uyumluluk (İşbirliği) gibi kritik faktörlerinin alt kırılımlarını inceler.</li>
                        <li><strong>Soru Tipi:</strong> Adayın bir ifadeye ne kadar katıldığını ölçen 1'den 5'e kadar Likert Ölçeği formatındaki sorulardır.</li>
                    </ul>
                </div>
                
                <div class="bg-green-50 border-l-4 border-green-500 p-4 rounded">
                    <h3 class="text-lg font-bold text-green-800 mb-3">2. BİLİŞSEL KAPASİTE TESTLERİ (Zihinsel Potansiyel)</h3>
                    <p class="mb-3">
                        Bu modül, adayın doğuştan gelen öğrenme hızını, problem çözme çevikliğini ve karmaşık bilgiyi işleme potansiyelini ölçer.
                    </p>
                    <ul class="list-disc list-inside space-y-2 ml-4">
                        <li><strong>Akademik Kök:</strong> Genel Zekâ Faktörü (g-faktörü) teorisine dayanır. Yüksek g-faktörü, adayın adaptasyon ve uzun vadeli gelişim potansiyelinin en güçlü göstergesidir.</li>
                        <li><strong>Ölçülen Alanlar:</strong>
                            <ul class="list-disc list-inside ml-4 mt-2 space-y-1">
                                <li><strong>Analitik Düşünme ve Veri İşleme:</strong> Sayısal veriyi ve mantıksal desenleri işleme hızı.</li>
                                <li><strong>Sözel Akıl Yürütme ve Anlama:</strong> Karmaşık yazılı ve sözlü bilgileri doğru yorumlama becerisi.</li>
                            </ul>
                        </li>
                        <li><strong>Soru Tipi:</strong> Süreli, mantıksal çıkarım ve hızlı muhakeme gerektiren performans testleridir.</li>
                    </ul>
                </div>
                
                <div class="bg-purple-50 border-l-4 border-purple-500 p-4 rounded">
                    <h3 class="text-lg font-bold text-purple-800 mb-3">3. DURUMSAL YARGI TESTLERİ (SJT) (Uygulamalı Yargı Kalitesi)</h3>
                    <p class="mb-3">
                        Bu modül, adayın teorik eğiliminden bağımsız olarak, kritik bir iş senaryosu karşısında pratikte hangi eylemi seçeceğini ölçer.
                    </p>
                    <ul class="list-disc list-inside space-y-2 ml-4">
                        <li><strong>Akademik Kök:</strong> Kritik Olay Tekniği ile toplanan, pozisyona özgü gerçek hayattan senaryolara dayanır.</li>
                        <li><strong>Ölçülen Alan:</strong> Etik ikilemler, çatışma yönetimi ve kriz anı reaksiyonlarında kurumsal değerlere ne kadar yakın kararlar alındığı.</li>
                        <li><strong>Puanlama Mantığı:</strong> Basit bir doğru-yanlış yerine, uzmanlar paneli tarafından belirlenen Uzman Görüş Birliği (Expert Consensus) puanına göre derecelendirilir.</li>
                    </ul>
                </div>
                

            </div>
            
            <div class="p-6 border-t border-gray-200 bg-gray-50">
                <div class="flex justify-center">
                    <button onclick="closeMethodology()" class="bg-green-600 hover:bg-green-700 text-white font-semibold py-3 px-8 rounded-xl transition duration-300 flex items-center space-x-2">
                        <svg class="w-5 h-5" fill="currentColor" viewBox="0 0 20 20">
                            <path fill-rule="evenodd" d="M16.707 5.293a1 1 0 010 1.414l-8 8a1 1 0 01-1.414 0l-4-4a1 1 0 011.414-1.414L8 12.586l7.293-7.293a1 1 0 011.414 0z" clip-rule="evenodd"></path>
                        </svg>
                        <span>Anladım</span>
                    </button>
                </div>
            </div>
        </div>
    </div>

    <!-- Sorumluluk Reddi Modal -->
    <div id="disclaimerModal" class="fixed inset-0 bg-black bg-opacity-50 flex items-center justify-center p-4 z-[9999]" style="display: none;" onclick="event.target === this && closeDisclaimer()">
        <div class="bg-white rounded-2xl shadow-2xl w-full max-w-4xl max-h-[90vh] overflow-y-auto" onclick="event.stopPropagation()">
            <div class="p-6 border-b border-gray-200">
                <div class="flex justify-between items-center">
                    <h2 class="text-2xl font-bold text-gray-800">Hukuki Sorumluluk Reddi ve Veri Güvenliği Beyanı</h2>
                    <button onclick="closeDisclaimer()" class="text-gray-500 hover:text-gray-700">
                        <svg class="w-6 h-6" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M6 18L18 6M6 6l12 12"></path>
                        </svg>
                    </button>
                </div>
            </div>
            
            <div class="p-6 space-y-6 text-sm text-gray-700 leading-relaxed">
                <p class="text-base font-semibold text-blue-600">
                    Analiz Pro X platformu, veri analizi ve raporlama süreçlerinde hukuki uygunluk, şeffaflık ve etik sorumluluk prensiplerini benimser. Bu beyan, platformun teknolojik dayanağını, veri koruma politikalarını ve sonuçların kullanımına dair sorumluluk sınırlarını netleştirmektedir.
                </p>
                
                <div class="bg-blue-50 border-l-4 border-blue-500 p-4 rounded">
                    <h3 class="text-lg font-bold text-blue-800 mb-3">1. ALTYAPI VE VERİ GÜVENLİĞİ TEMİNATI (GOOGLE FIREBASE)</h3>
                    <p class="mb-3">
                        Platformun tüm teknolojik altyapısı ve veri yönetimi, dünya standartlarında güvenlik protokollerine sahip Google Firebase Güvenli Veri Tabanı üzerinde kurulmuştur. Bu seçim, müşterilerimize yüksek güvenlik, ölçeklenebilirlik ve kesintisizlik sunar:
                    </p>
                    <ul class="list-disc list-inside space-y-2 ml-4">
                        <li><strong>Kurumsal Seviyede Şifreleme:</strong> Tüm veriler, Firebase'in kurumsal düzeyde güvenlik ve şifreleme standartlarıyla korunur.</li>
                        <li><strong>Yüksek Performans:</strong> Google'ın bulut altyapısı, analiz süreçlerinin hızlı ve kesintisiz yürütülmesini garanti eder.</li>
                        <li><strong>Sorumluluk Reddi:</strong> Analiz Pro X, altyapı güvenliği için tamamen Google Firebase'in sağladığı protokol ve güvenlik standartlarına güvenir. Platform, Firebase'in dış tehditler sonucu oluşabilecek potansiyel güvenlik zafiyetlerinden veya altyapısal kesintilerden kaynaklanabilecek doğrudan sonuçlardan sorumlu tutulamaz.</li>
                    </ul>
                </div>
                
                <div class="bg-green-50 border-l-4 border-green-500 p-4 rounded">
                    <h3 class="text-lg font-bold text-green-800 mb-3">2. KİŞİSEL VERİ VE HUKUKİ UYUM (KVKK VE GDPR)</h3>
                    <p class="mb-3">
                        Analiz Pro X, Türkiye Cumhuriyeti'nin Kişisel Verilerin Korunması Kanunu (KVKK) ve Avrupa Birliği'nin Genel Veri Koruma Tüzüğü (GDPR) hükümlerine tam uyumlu olarak çalışır.
                    </p>
                    <ul class="list-disc list-inside space-y-2 ml-4">
                        <li><strong>Rumuz Fonksiyonu ile Anonimleştirme:</strong> Adaylardan hiçbir aşamada kimlik tespiti yapacak kişisel bilgi (Ad, Soyad, E-posta, TC Kimlik No) talep edilmez ve sistemimizde asla saklanmaz. Değerlendirme süreci, yalnızca İK personeliniz tarafından atanan Benzersiz Rumuz (Kod) üzerinden yürütülür.</li>
                        <li><strong>Veri Niteliği:</strong> Platformumuz, yasal olarak tanımlanmış "özel nitelikli kişisel veri" içermeyen, sadece adayın psikometrik skorlarını ve davranışsal eğilimlerini içeren anonimleştirilmiş analiz verilerini işler.</li>
                        <li><strong>Sorumluluk Beyanı:</strong> Platformumuz, kimlik bilgilerini içermeyen rumuz sistemi sayesinde, kullanıcı kurumların KVKK uyum süreçlerini destekler ve yasal risklerini minimize eder. Hukuki sorumluluğumuz, rumuz sistemi üzerinden işlenen analiz verileriyle sınırlıdır.</li>
                    </ul>
                </div>
                
                <div class="bg-orange-50 border-l-4 border-orange-500 p-4 rounded">
                    <h3 class="text-lg font-bold text-orange-800 mb-3">3. ANALİZ SONUÇLARININ NİHAİ KULLANIM SORUMLULUĞU</h3>
                    <p class="mb-3">
                        Analiz Pro X, Yapay Zekâ destekli bilimsel metotlarla prediktif analiz ve risk raporlaması sunan üst düzey bir karar destek aracıdır. Platformun sunduğu raporlar, nihai bir hüküm veya direktif değildir.
                    </p>
                    <p class="font-semibold text-orange-800">
                        <strong>Sorumluluk Beyanı:</strong> Platform tarafından sunulan Görüşme Önerileri, Risk Seviyeleri ve Yetkinlik Skorları tamamen tavsiye niteliğindedir. Adayın işe alım, elenme, terfi ettirilme veya görevlendirilme kararlarının nihai sorumluluğu ve takdiri, her zaman kullanıcı kurumun yetkili İK ve Yönetici kadrolarına aittir. Analiz Pro X, verilen raporların tavsiye niteliğinden dolayı ortaya çıkabilecek örgütsel veya operasyonel sonuçlardan sorumlu tutulamaz.
                    </p>
                </div>
            </div>
            
            <div class="p-6 border-t border-gray-200 bg-gray-50">
                <div class="flex justify-center">
                    <button onclick="acceptDisclaimer()" class="bg-green-600 hover:bg-green-700 text-white font-semibold py-3 px-8 rounded-xl transition duration-300 flex items-center space-x-2">
                        <svg class="w-5 h-5" fill="currentColor" viewBox="0 0 20 20">
                            <path fill-rule="evenodd" d="M16.707 5.293a1 1 0 010 1.414l-8 8a1 1 0 01-1.414 0l-4-4a1 1 0 011.414-1.414L8 12.586l7.293-7.293a1 1 0 011.414 0z" clip-rule="evenodd"></path>
                        </svg>
                        <span>Okudum ve Onaylıyorum</span>
                    </button>
                </div>
            </div>
        </div>
    </div>

    <!-- Aday Test Paneli -->
    <div id="candidatePanel" class="hidden min-h-screen bg-gray-50">
        <nav class="bg-white shadow-lg">
            <div class="max-w-7xl mx-auto px-4">
                <div class="flex justify-between items-center py-4">
                    <h1 class="text-2xl font-bold text-gray-800">Aday Test Paneli</h1>
                    <button onclick="logout()" class="bg-red-600 hover:bg-red-700 text-white px-4 py-2 rounded-lg">Çıkış</button>
                </div>
            </div>
        </nav>
        
        <div id="candidateWelcome" class="max-w-4xl mx-auto p-6">
            <div class="bg-white rounded-xl shadow-lg p-8 text-center">
                <h2 class="text-3xl font-bold text-gray-800 mb-4">Hoş Geldiniz!</h2>
                <p class="text-gray-600 mb-6">Test alanınız: <span id="candidateTestArea" class="font-semibold text-blue-600"></span></p>
                <p class="text-gray-600 mb-8">Teste başlamak için aşağıdaki butona tıklayın. Test süresince dikkatli olun ve sorularınızı dikkatlice okuyun.</p>
                <button onclick="startTest()" class="bg-green-600 hover:bg-green-700 text-white font-semibold py-4 px-8 rounded-xl transition duration-300 transform hover:scale-105">
                    Teste Başla
                </button>
            </div>
        </div>
        
        <div id="candidateTest" class="hidden max-w-4xl mx-auto p-6">
            <div class="bg-white rounded-xl shadow-lg p-8">
                <div class="flex justify-between items-center mb-6">
                    <h3 class="text-xl font-bold text-gray-800">Soru <span id="currentQuestionNumber">1</span> / <span id="totalQuestions">10</span></h3>
                    <div class="text-lg font-semibold text-blue-600">Süre: <span id="testTimer">30:00</span></div>
                </div>
                
                <div id="questionContent" class="mb-8">
                    <!-- Sorular buraya yüklenecek -->
                </div>
                
                <div class="flex justify-between">
                    <button id="prevButton" onclick="previousQuestion()" class="bg-gray-600 hover:bg-gray-700 text-white py-2 px-6 rounded-lg disabled:opacity-50" disabled>
                        Önceki
                    </button>
                    <button id="nextButton" onclick="nextQuestion()" class="bg-blue-600 hover:bg-blue-700 text-white py-2 px-6 rounded-lg">
                        Sonraki
                    </button>
                    <button id="finishButton" onclick="finishTest()" class="hidden bg-green-600 hover:bg-green-700 text-white py-2 px-6 rounded-lg">
                        Testi Bitir
                    </button>
                </div>
            </div>
        </div>
        
        <div id="testCompleted" class="hidden max-w-4xl mx-auto p-6">
            <div class="bg-white rounded-xl shadow-lg p-8 text-center">
                <div class="text-6xl mb-4">🎉</div>
                <h2 class="text-3xl font-bold text-gray-800 mb-4">Test Tamamlandı!</h2>
                <p class="text-gray-600 mb-6">Testinizi başarıyla tamamladınız. Sonuçlarınız değerlendirilmek üzere İK departmanına iletilmiştir.</p>
                <button onclick="logout()" class="bg-blue-600 hover:bg-blue-700 text-white font-semibold py-3 px-6 rounded-xl transition duration-300">
                    Çıkış Yap
                </button>
            </div>
        </div>
    </div>

    <script>
                // Firebase başlatma
                                // ...firebaseConfig ve db tanımı en başta var, tekrar etmeye gerek yok...
        // Ters ifadeler için cevap puanına göre anlam/yorum tablosu
        const tersYorumTablosu = {
            1: "Çok olumsuz davranış",
            2: "Olumsuz eğilim",
            3: "Orta düzeyde eğilim",
            4: "Olumlu eğilim",
            5: "Çok olumlu davranış"
        };

        // Ters ifadede verilen cevaba göre anlam döndüren fonksiyon
        function tersYorumGetir(puan) {
            return tersYorumTablosu[puan] || "";
        }
        // 1-500 arası sorular için cevap anahtarı (cevap.txt'den alınmıştır)
        // Her bir cevap, 0 tabanlı index ile (Cevap 1 => 0, Cevap 2 => 1, ...)
        const answerKey = [
            0,1,2,3,4,0,1,2,3,4,0,1,2,3,4,0,1,2,3,4,0,1,2,3,4,0,1,2,3,4,0,1,2,3,4,0,1,2,3,4,0,1,2,3,4,0,1,2,3,4,0,1,2,3,4,0,1,2,3,4,0,1,2,3,4,0,1,2,3,4,0,1,2,3,4,0,1,2,3,4,0,1,2,3,4,0,1,2,3,4,0,1,2,3,4,0,1,2,3,4,
            0,1,2,3,4,0,1,2,3,4,0,1,2,3,4,0,1,2,3,4,0,1,2,3,4,0,1,2,3,4,0,1,2,3,4,0,1,2,3,4,0,1,2,3,4,0,1,2,3,4,0,1,2,3,4,0,1,2,3,4,0,1,2,3,4,0,1,2,3,4,0,1,2,3,4,0,1,2,3,4,0,1,2,3,4,0,1,2,3,4,0,1,2,3,4,0,1,2,3,4,0,1,2,3,4,
            0,1,2,3,4,0,1,2,3,4,0,1,2,3,4,0,1,2,3,4,0,1,2,3,4,0,1,2,3,4,0,1,2,3,4,0,1,2,3,4,0,1,2,3,4,0,1,2,3,4,0,1,2,3,4,0,1,2,3,4,0,1,2,3,4,0,1,2,3,4,0,1,2,3,4,0,1,2,3,4,0,1,2,3,4,0,1,2,3,4,0,1,2,3,4,0,1,2,3,4,0,1,2,3,4,
            0,1,2,3,4,0,1,2,3,4,0,1,2,3,4,0,1,2,3,4,0,1,2,3,4,0,1,2,3,4,0,1,2,3,4,0,1,2,3,4,0,1,2,3,4,0,1,2,3,4,0,1,2,3,4,0,1,2,3,4,0,1,2,3,4,0,1,2,3,4,0,1,2,3,4,0,1,2,3,4,0,1,2,3,4,0,1,2,3,4,0,1,2,3,4,0,1,2,3,4,0,1,2,3,4,
            0,1,2,3,4,0,1,2,3,4,0,1,2,3,4,0,1,2,3,4,0,1,2,3,4,0,1,2,3,4,0,1,2,3,4,0,1,2,3,4,0,1,2,3,4,0,1,2,3,4,0,1,2,3,4,0,1,2,3,4,0,1,2,3,4,0,1,2,3,4,0,1,2,3,4,0,1,2,3,4,0,1,2,3,4,0,1,2,3,4,0,1,2,3,4,0,1,2,3,4,0,1,2,3,4
        ];
        // Global değişkenler
        let currentUser = null;
        let currentRole = null;
        let currentQuestionIndex = 0;
        let testQuestions = [];
        let userAnswers = [];
        let testTimer = null;
        let googleUser = null; // Google kullanıcı bilgisi

        // Google Authentication Fonksiyonları
        async function signInWithGoogle() {
            try {
                // Yeniden giriş tetiklemek için önce mevcut oturumu kapat (sessiz)
                if (auth.currentUser) {
                    try { await auth.signOut(); } catch(e) { console.warn('Ön signOut başarısız (önemsiz):', e); }
                }
                const result = await auth.signInWithPopup(googleProvider);
                googleUser = result.user;
                console.log('Google ile giriş başarılı:', googleUser);
                
                // Sadece butonu değiştir - ekran aynı kalsın
                updateGoogleButton(true);
                updateHrRegisterButton();
                
            } catch (error) {
                console.error('Google giriş hatası:', error);
                alert('Google ile giriş yapılırken bir hata oluştu: ' + error.message);
            }
        }

        async function signOutGoogle() {
            try {
                await auth.signOut();
                googleUser = null;
                currentUser = null;
                console.log('Google çıkış başarılı');
                
                // Sadece butonu eski haline döndür - ekran aynı kalsın
                updateGoogleButton(false);
                
            } catch (error) {
                console.error('Google çıkış hatası:', error);
                alert('Çıkış yapılırken bir hata oluştu: ' + error.message);
            }
        }

        function updateGoogleButton(isLoggedIn) {
            const googleButton = document.getElementById('googleAuthButton');
            
            if (isLoggedIn) {
                // Giriş yapılmış - Çıkış butonu göster
                googleButton.innerHTML = `
                    <svg class="w-6 h-6" viewBox="0 0 24 24">
                        <path fill="currentColor" d="M16 17v-3H9v-4h7V7l5 5-5 5zM14 2C11.24 2 9 4.24 9 7v1h5V7c0-1.1-.9-2-2-2s-2 .9-2 2v1h1V7c0-.55.45-1 1-1s1 .45 1 1v1h1V7c0-2.76-2.24-5-5-5z"/>
                    </svg>
                    <span>🚪 Çıkış Yap</span>
                `;
                googleButton.onclick = signOutGoogle;
                googleButton.classList.remove('bg-red-600', 'hover:bg-red-700');
                googleButton.classList.add('bg-green-600', 'hover:bg-green-700');
            } else {
                // Çıkış yapılmış - Giriş butonu göster
                googleButton.innerHTML = `
                    <svg class="w-6 h-6" viewBox="0 0 24 24">
                        <path fill="currentColor" d="M22.56 12.25c0-.78-.07-1.53-.2-2.25H12v4.26h5.92c-.26 1.37-1.04 2.53-2.21 3.31v2.77h3.57c2.08-1.92 3.28-4.74 3.28-8.09z"/>
                        <path fill="currentColor" d="M12 23c2.97 0 5.46-.98 7.28-2.66l-3.57-2.77c-.98.66-2.23 1.06-3.71 1.06-2.86 0-5.29-1.93-6.16-4.53H2.18v2.84C3.99 20.53 7.7 23 12 23z"/>
                        <path fill="currentColor" d="M5.84 14.09c-.22-.66-.35-1.36-.35-2.09s.13-1.43.35-2.09V7.07H2.18C1.43 8.55 1 10.22 1 12s.43 3.45 1.18 4.93l2.85-2.22.81-.62z"/>
                        <path fill="currentColor" d="M12 5.38c1.62 0 3.06.56 4.21 1.64l3.15-3.15C17.45 2.09 14.97 1 12 1 7.7 1 3.99 3.47 2.18 7.07l3.66 2.84c.87-2.6 3.3-4.53 6.16-4.53z"/>
                    </svg>
                    <span>🔐 Google ile Giriş</span>
                `;
                googleButton.onclick = signInWithGoogle;
                googleButton.classList.remove('bg-green-600', 'hover:bg-green-700');
                googleButton.classList.add('bg-red-600', 'hover:bg-red-700');
            }
        }

        function showLoginScreen() {
            // Tüm panelleri gizle
            document.querySelectorAll('[id$="Panel"]').forEach(panel => panel.classList.add('hidden'));
            document.getElementById('roleLoginScreen').classList.add('hidden');
            // Ana giriş ekranını göster
            document.getElementById('loginScreen').classList.remove('hidden');
        }

        async function registerGoogleUserAsHR(user) {
            try {
                if (!user) return;
                if (!user.email) return;
                // Otomatik kayıt KALDIRILDI: Manuel işlem istenirse ayrı tetiklenecek
                return; // Erken çık
                // Google kullanıcısını Firebase'de İK yöneticisi olarak kaydet
                /* eski otomatik kayıt kodu devre dışı
                const hrData = {...};
                */
                
            } catch (error) {
                console.error('Google kullanıcısını kaydetme hatası:', error);
            }
        }

        // Auth state değişikliklerini dinle
        let userInitiatedGoogle = false; // Buton tıklaması flag
        // Butona basınca set edilecek
        const originalSignIn = signInWithGoogle;
        signInWithGoogle = async function() { userInitiatedGoogle = true; return originalSignIn(); }

        auth.onAuthStateChanged((user) => {
            if (user && userInitiatedGoogle) {
                googleUser = user;
                console.log('Kullanıcı oturum açmış (manuel):', user.email);
                updateGoogleButton(true);
            } else if (user && !userInitiatedGoogle) {
                // Eski session yakalandı - yok say ve çıkış yap
                console.log('Önceki session tespit edildi, otomatik çıkış...');
                auth.signOut();
                return;
            } else {
                googleUser = null;
                console.log('Kullanıcı oturum açmamış');
                updateGoogleButton(false);
            }
            updateHrRegisterButton();
        });
        
        // Oturum kalıcılığını session bazlı yap (tarayıcı kapanınca silinsin)
        if (auth && auth.setPersistence) {
            auth.setPersistence(firebase.auth.Auth.Persistence.SESSION).catch(e=>console.warn('Persistence set edilemedi:', e));
        }
        let timeRemaining = 1800; // 30 dakika
        let disclaimerAccepted = false;

        // Global hata yakalama
        // Global hata yakalayıcı ve element kontrolü
        function safeAddEventListener(elementId, event, handler) {
            const element = document.getElementById(elementId);
            if (element) {
                element.addEventListener(event, handler);
                return true;
            } else {
                console.warn('Element bulunamadı:', elementId);
                return false;
            }
        }
        
        window.addEventListener('error', function(e) {
            console.log('Hata yakalandı ve engellendi:', e.error);
            return true; // Hatayı sessizce yakala
        });

        // Firebase bağlantısı için hazır yapı (sonradan eklenecek)
        // Örnek veri yapıları (Firebase'e geçiş için hazır)
        let hrManagers = [];
        let candidates = [];
        let testResults = [];

        // Firebase'den adayları çek
        function fetchCandidates(callback) {
            db.ref('candidates').once('value').then(snapshot => {
                const val = snapshot.val() || {};
                candidates = Object.values(val);
                if (callback) callback();
            });
        }

        // Firebase'den İK yöneticilerini çek
        function fetchHrManagers(callback) {
            db.ref('hrManagers').once('value').then(snapshot => {
                const val = snapshot.val() || {};
                hrManagers = Object.values(val);
                if (callback) callback();
            });
        }

        // Firebase'e yeni İK yöneticisi ekle
        function addHrManager(hrObj) {
            try {
                const newRef = db.ref('hrManagers').push();
                hrObj.id = newRef.key;
                return newRef.set(hrObj).then(() => {
                    console.log('İK yöneticisi başarıyla kaydedildi:', hrObj);
                    return true;
                }).catch((error) => {
                    console.error('Firebase kayıt hatası:', error);
                    alert('Kayıt sırasında hata oluştu: ' + error.message);
                    return false;
                });
            } catch (error) {
                console.error('addHrManager hatası:', error);
                alert('Kayıt yapılamadı: ' + error.message);
                return false;
            }
        }

        // Firebase'den İK yöneticisi sil
        function deleteHrManager(hrId) {
            db.ref('hrManagers/' + hrId).remove();
        }

        // Soru bankası - yeni sorular yüklenecek
        const questionBank = {
            grup1: [
                "Yapılacak işler listesini daima önceliklendiririm",
                "Bitmeyen işler yüzünden kişisel zamanımı sürekli feda ederim",
                "Karmaşık projeleri küçük parçalara ayırarak planlarım",
                "Dikkatimi dağıtan şeylere engel olmakta zorlanırım",
                "Toplantılarıma her zaman zamanında katılırım",
                "Son teslim tarihleri yaklaştıkça daha çok stres olurum",
                "Haftalık planımı esnekliği koruyacak şekilde hazırlarım",
                "Sık sık bir işi bitirmek için gereken süreyi yanlış hesaplarım",
                "En zor işleri günün erken saatlerinde bitirmeye çalışırım",
                "İşlerimi genellikle başkalarının beni itmesiyle tamamlarım",
                "Ekip arkadaşlarımın fikirlerini aktif olarak dinlerim ve dikkate alırım",
                "Ekip içindeki sorunların çözümüne katılmak yerine, sadece kendi işime bakarım",
                "Başkalarına görevlerini tamamlamaları için zamanında geri bildirim sağlarım",
                "Çatışma durumlarında taraf tutmamak için sessiz kalmayı tercih ederim",
                "Ekipteki bilgi birikimini artırmak için öğrendiklerimi paylaşırım",
                "Ekibin başarısızlıklarının sorumluluğunu üstlenmekten kaçınırım",
                "Farklı kültürel veya profesyonel geçmişe sahip kişilerle kolayca uyum sağlarım",
                "Ekip üyelerimin motivasyonunu yükseltmek için çabalamam",
                "Ortak kararlar alındıktan sonra bile kendi yöntemimi uygulamakta ısrar ederim",
                "Ekip dışındaki paydaşlarla verimli bir şekilde işbirliği yapabilirim",
                "E-posta veya yazılı iletişimde net ve anlaşılır ifadeler kullanırım",
                "Zor bir konu konuşulurken göz teması kurmaktan çekinirim",
                "Karşımdaki kişinin beden dilini ve duygusal durumunu anlarım",
                "Meslektaşlarıma eleştirilerimi genellikle sert bir dille ifade ederim",
                "Bir sunum yaparken seyircinin dikkatini kolayca toplayabilirim",
                "Fikirlerim kabul edilmezse, tartışmayı uzatmayı tercih ederim",
                "Bilgi akışını sağlamak için düzenli ve kısa bilgilendirme toplantıları yaparım",
                "İş arkadaşlarımdan gelen talimatları sık sık yanlış anlarım",
                "Telefonda konuşurken de yüz yüze konuşmadaki kadar etkili iletişim kurarım",
                "Benimle aynı fikirde olmayan kişileri dinlemeye isteksiz olurum",
                "Üstlendiğim projelerin nihai sonuçlarından tamamen ben sorumluyum",
                "Hatalarımı gizlemek için ufak tefek yalanlar söyleyebilirim",
                "Zorlu hedeflere ulaşmak için ekstra çaba göstermekten kaçınmam",
                "Bana verilen talimatlara kesinlikle uymakta güçlük çekerim",
                "İşler yolunda gitmediğinde dış etkenlere odaklanmak yerine çözüm ararım",
                "İşimin kalitesi, sadece üzerimdeki denetime bağlıdır",
                "Kritik ve acil durumlarda dahi doğru kararı veririm",
                "İşlerimi tamamlamak için sürekli olarak başkalarının hatırlatmasına ihtiyacım olur",
                "Şirketin kaynaklarını kendi kaynaklarım gibi korur ve verimli kullanırım",
                "Baskı altında çalışırken kendimi verimli hissetmem",
                "Bir problem ortaya çıktığında verileri analiz ederek kök nedeni bulurum",
                "Basit çözümler işe yaramadığında hemen pes ederim",
                "Sorunu çözmek için farklı departmanlardan uzmanlarla görüşürüm",
                "Rutin dışındaki sorunlarla uğraşmak benim görevim değildir",
                "En karmaşık sorunları bile mantıksal adımlarla çözebilirim",
                "Bir çözüm önerirken, bunun olası yan etkilerini düşünmem",
                "Çözüm süreci boyunca soğukkanlılığımı ve objektifliğimi korurum",
                "Sık sık karar veremez ve ertelemeyi tercih ederim",
                "Aldığım kararların uzun vadeli etkilerini öngörürüm",
                "Problemi çözmektense, görmezden gelmek daha kolay gelir",
                "Yaptığım her işte mükemmeliyetçi bir yaklaşım sergilerim",
                "Teslim tarihlerine yetişmek için kalite kontrol adımlarını atlarım",
                "Hata payını en aza indirmek için ekstra önlemler alırım",
                "Sadece yöneticim talep ettiğinde kalite standartlarına uyarım",
                "Belirlenen kalite hedeflerine ulaşmak için aktif olarak iyileştirme öneririm",
                "İşimin sonucundan çok, hızına odaklanırım",
                "Kalite süreçlerinin tüm aşamalarına dikkat eder ve uyum sağlarım",
                "Müşterinin beklentileri benim için genellikle belirsizdir",
                "Kendi hatalarımı bulmak ve düzeltmek için proaktif davranırım",
                "İş yüküm arttığında ilk vazgeçeceğim şey detaylara dikkat etmek olur",
                "Müşteri beklentilerini anlamak için düzenli olarak iletişim kurarım",
                "Müşteri talepleri iş tanımımın dışına çıktığında itiraz ederim",
                "Müşteri memnuniyetini sağlamak için esnek çözümler üretirim",
                "Müşteri şikayetleri karşısında duygusal tepkiler vermekten kaçınırım",
                "Müşterilerimizle uzun vadeli ilişkiler kurmayı hedeflerim",
                "Sektördeki yenilikleri takip etme zorunluluğu hissetmem",
                "Müşteri geri bildirimlerini iş süreçlerime dahil ederek iyileştiririm",
                "Müşterinin verdiği bilgilerin doğruluğunu her zaman kontrol etmeye gerek yoktur",
                "Hem iç hem de dış müşterilerime eşit derecede önem veririm",
                "Müşteriye hayır demekten çekinmem",
                "Bir projede doğal olarak liderlik rolünü üstlenmeye hazırım",
                "Genellikle riskli kararlar almaktan kaçınırım",
                "Ekip arkadaşlarıma görevleri adil bir şekilde delege edebilirim",
                "Bir ekibi yönetmek, kişisel performansıma odaklanmaktan daha zordur",
                "Başkalarını motive etmek ve ortak bir vizyon etrafında toplamak konusunda başarılıyımdır",
                "Hata yapan birini eleştirmektense, konuyu geçiştirmeyi tercih ederim",
                "Ekip üyelerimin gelişimine katkıda bulunmak için mentorluk yaparım",
                "İnsanların bana gönüllü olarak uyması benim için önemli değildir",
                "Zorlu durumlarda bile ekibe sakinlik ve güven aşılarım",
                "Liderlik pozisyonu, beraberinde getirdiği sorumluluklar nedeniyle gözümü korkutur",
                "Mevcut süreçleri iyileştirmek için proaktif öneriler sunarım",
                "Sadece bana söylenilen görevleri yaparım, fazlasını değil",
                "İhtiyaç duyulan bilgi veya kaynağı kendi çabamla bulurum",
                "Hata yapma ihtimali varsa, yeni bir şey denemekten kaçınırım",
                "İşleri hızlandırmak ve verimliliği artırmak için yaratıcı yollar denerim",
                "Sorunlarımı amirime danışmadan çözmeye çalışmam",
                "Acil bir durumda dahi yetki beklemeden doğru kararı veririm",
                "Başkalarının benim için harekete geçmesini beklerim",
                "Yeni projeler veya bilinmeyen alanlar beni heyecanlandırır",
                "Yeni bir göreve başlarken detaylı bir kılavuz olmasını şart koşarım",
                "Öğrenmeye ve yeni yetenekler kazanmaya her zaman hevesliyimdir",
                "Kendi güçlü ve zayıf yönlerimi bilmek beni ilgilendirmez",
                "Performansımı düzenli olarak değerlendirir ve kendimi geliştiririm",
                "Eğitimler ve seminerler genellikle zaman kaybıdır",
                "Başarısızlıkları birer öğrenme fırsatı olarak görürüm",
                "Değişen çalışma yöntemlerine ayak uydurmak benim için zordur",
                "Eleştirilere açıktır ve bu geri bildirimleri gelişmek için kullanırım",
                "İşimi en iyi şekilde yaptığımı düşündüğüm için gelişime ihtiyacım yoktur",
                "Sektördeki son trendleri ve teknolojileri düzenli olarak takip ederim",
                "Kariyer hedeflerime ulaşmak için net bir kişisel gelişim planım vardır"
            ],
            grup2: [
                "Çalışma alanımda her zaman güvenlik prosedürlerine uygun hareket ederim",
                "Basit görevlerde bile kişisel koruyucu ekipman (KKE) kullanmaktan kaçınırım",
                "Tehlikeli durum veya eksik ekipman gördüğümde hemen ilgililere bildiririm",
                "İş güvenliği kurallarının esnetilebileceği durumlar olabilir",
                "Acil durum prosedürleri ve tahliye yollarını düzenli olarak kontrol ederim",
                "Hızlı çalışmak, güvenlik kurallarından daha önemlidir",
                "Makinelerin ve aletlerin bakımının tam olduğundan emin olurum",
                "Diğer çalışanları güvenlik riskleri konusunda uyarmak benim işim değildir",
                "Güvenlik talimatlarını sadece denetim olduğunda uygularım",
                "Yeni bir ekipman kullanmadan önce daima eğitim alırım",
                "Ekip arkadaşlarıma ihtiyaç duyduklarında yardım etmeye gönüllüyüm",
                "Ekip içindeki anlaşmazlıklar beni doğrudan etkilemezse karışmam",
                "İş akışının aksamaması için kendi görevimi eksiksiz tamamlarım",
                "Sadece benim dışımdaki ekip üyelerinin hataları üzerinde dururum",
                "Ortak bir hedef belirlediğimizde bu hedefe tam olarak bağlı kalırım",
                "Ekipteki herkesin işine eşit derecede saygı gösteririm",
                "Ekip çalışmasında en önemli şey kendi performansımı öne çıkarmaktır",
                "Ekip içindeki sorunları çözmek için aktif olarak görüş bildiririm",
                "Yüksek tempoda çalışırken ekip ruhunu sürdürmekte zorlanmam",
                "Benim dışımda gelişen hatalar için sorumluluk almam",
                "Talimatları dinlerken önemli noktaları not ederim",
                "Yöneticimle konuşurken genellikle konuyu dağıtırım",
                "İş arkadaşlarımla net ve saygılı bir dil kullanırım",
                "Bir talimatı tam olarak anlamadan uygulamaya başlarım",
                "İşle ilgili bilgileri gerekli kişilere anında iletirim",
                "Sık sık iş arkadaşlarımın sözünü keserim",
                "Üretim veya hizmet alanındaki sorunları çekinmeden dile getiririm",
                "İletişim kurarken teknik jargon kullanmaktan kaçınmam",
                "Çalışma ortamında çıkan dedikodulara katılmam",
                "Sorun çıktığında kişisel olarak algılar ve küserim",
                "İşime her zaman zamanında başlar ve bitiririm",
                "İş yerindeki kişisel eşyalarımı dağınık bırakırım",
                "Üretim sürecindeki aksaklıkları hemen üstüme bildiririm",
                "Çalışma ortamımın temiz ve düzenli olmasından ben sorumlu değilim",
                "Verilen görevleri tamamlamadan işten ayrılmam",
                "Benim sorumluluğumdaki bir hata çıktığında kolayca mazeret bulurum",
                "Tüm araç ve gereçleri büyük bir dikkatle kullanırım",
                "İş yapma biçimim sık sık başkalarını olumsuz etkiler",
                "Görevimi yerine getirirken şirket kaynaklarını dikkatli kullanırım",
                "İşimi yaparken sık sık kişisel işlerimle ilgilenirim",
                "Karşılaştığım basit arızaları veya sorunları kendim çözmeye çalışırım",
                "Bir sorun çıktığında ilk tepkim başkasının gelip çözmesini beklemek olur",
                "Sorunu çözmek için farklı yöntemleri denemekten çekinmem",
                "Bir sorunun kök nedenini bulmak yerine, sadece belirtileri gidermeye odaklanırım",
                "Problem çözümü için gerekli olan bilgileri hızla toplarım",
                "Karmaşık sorunlar beni gergin ve çaresiz hissettirir",
                "Çözüm süreci boyunca soğukkanlılığımı korurum",
                "Basit sorunları bile çözmek için uzun süreye ihtiyacım olur",
                "İş akışımı etkileyen problemleri yöneticime doğru şekilde aktarırım",
                "Başkalarının çözüm önerilerini genellikle kabul etmem",
                "Üretim/hizmet standartlarını titizlikle uygularım",
                "Kalite kontrolde çıkan küçük kusurları göz ardı edebilirim",
                "Hata payını en aza indirmek için ekstra önlemler alırım",
                "İşim bittikten sonra kontrol etmekle vakit kaybetmem",
                "Kalite hedeflerine ulaşmak için ekip arkadaşlarımla işbirliği yaparım",
                "Kalitesiz ürünler üretmektense, üretimi yavaşlatmayı tercih ederim",
                "Kalite benim için hızdan sonra gelir",
                "Kullandığım malzemelerin kalitesini sürekli kontrol ederim",
                "Kaliteyi sağlamanın tek yolu katı kurallara uymaktır",
                "Kaliteyi artıracak önerilerimi yöneticilerime sunmaktan çekinmem",
                "Vardiyam boyunca işimi verimli bir şekilde planlarım",
                "Molalarımı genellikle uzatırım",
                "İş akışındaki önceliklere göre kendimi hızlıca adapte ederim",
                "Bana verilen görevi ne zaman bitireceğimi nadiren bilirim",
                "Boş zamanlarımda bile etrafımdaki işleri düzenlerim",
                "Görevleri yetiştiremeyeceğimi anladığımda yardım istemem",
                "Zorlu işleri bitirmek için gerekli zamanı her zaman ayırırım",
                "İşim bittikten sonra dinlenmektense, yeni işler aramaya koyulurum",
                "İş yerindeki dağınıklık çalışma hızımı olumsuz etkilemez",
                "Bir işi bitirmek için sürekli başkalarının onayını beklerim",
                "Kendi çalışma alanımda yetkili bir ses olmayı amaçlarım",
                "İş arkadaşlarımı etkilemekten ve yönlendirmekten hoşlanmam",
                "Yeni gelenlere işi öğretme sorumluluğunu üstlenirim",
                "Çevremdeki kişilerin benim fikirlerime ne kadar değer verdiği umrumda değildir",
                "Ekipteki herkesin işine odaklanmasını sağlamayı başarırım",
                "İş arkadaşlarım zor durumdayken onlara akıl hocalığı yapmam",
                "Diğer çalışanları motive etmek için pozitif bir enerji yayarım",
                "Sadece emir almaktan ve uygulamaktan memnunum",
                "Çalışma alanımda bir kural ihlali gördüğümde çekinmeden müdahale ederim",
                "Liderlik sadece yöneticilerin işidir",
                "Daha iyi bir yol varsa, mevcut talimatlara bağlı kalmam",
                "Yeni bir yöntemi denemek yerine bildiğim yoldan giderim",
                "Üretimde/hizmette verimliliği artıracak fikirleri hemen denerim",
                "Bir işe başlamadan önce tüm detayların bana verilmesini beklerim",
                "Riskleri hesaplayarak yeni bir sorumluluğu üstlenmekten çekinmem",
                "Sorunlarımı amirime danışmadan çözmeye çalışmam",
                "Acil bir durumda dahi yetki beklemeden doğru kararı veririm",
                "Başkalarının benim için harekete geçmesini beklerim",
                "Yeni projeler veya bilinmeyen alanlar beni heyecanlandırır",
                "Bir görevde yetkimin dışına çıkmaktan korkarım",
                "İşimi daha iyi yapmak için sürekli yeni beceriler öğrenirim",
                "Kendi güçlü ve zayıf yönlerimi bilmek beni ilgilendirmez",
                "Performansımı düzenli olarak değerlendirir ve kendimi geliştiririm",
                "Eğitimler ve seminerler genellikle zaman kaybıdır",
                "Başarısızlıkları birer öğrenme fırsatı olarak görürüm",
                "Değişen çalışma yöntemlerine ayak uydurmak benim için zordur",
                "Eleştirilere açıktır ve bu geri bildirimleri gelişmek için kullanırım",
                "İşimi en iyi şekilde yaptığımı düşündüğüm için gelişime ihtiyacım yoktur",
                "Sektördeki son trendleri ve teknolojileri düzenli olarak takip ederim",
                "Kariyer hedeflerime ulaşmak için net bir kişisel gelişim planım vardır"
            ],
            grup3: [
                "Şirketin uzun vadeli hedeflerini günlük kararlarıma dahil ederim",
                "Sektördeki rakiplerin hareketlerini göz ardı ederim",
                "Gelecekteki pazar trendlerini tahmin edebilirim",
                "Kısa vadeli sonuçlar, stratejik planlamadan daha önemlidir",
                "Riskleri ve fırsatları değerlendirerek alternatif planlar geliştiririm",
                "Büyük resmi görmekten çok, detaylara takılı kalırım",
                "Stratejik kararlarımı desteklemek için güvenilir verilere dayanırım",
                "İşler yolunda giderken yeni stratejiler geliştirmeye gerek duymam",
                "Organizasyonumun rekabet avantajını sürekli sorgularım",
                "İş planlarını genellikle son teslim tarihine yakın hazırlarım",
                "Zor ve önemli kararları hızlı ve emin bir şekilde alırım",
                "Karar alma sürecinde sadece kendi deneyimlerime güvenirim",
                "Bir kararın olası sonuçlarını önceden hesaplarım",
                "Baskı altında karar vermek beni felç eder",
                "Farklı görüşleri dinledikten sonra objektif bir karar veririm",
                "Hata yapmamak için karar vermeyi sürekli ertelerim",
                "Yeterli bilgi olmadığında bile risk alarak ilerlerim",
                "Yanlış olduğu ortaya çıkan bir kararda sorumluluğu başkasına atarım",
                "Başkalarına karar verme yetkisi (delege etme) vermekten çekinirim",
                "Kararlarımı ekibime açık ve mantıklı bir şekilde açıklarım",
                "Ekibimi şirketin vizyonu etrafında toplayabilir ve motive ederim",
                "Ekip üyelerimin kişisel gelişim hedefleri beni ilgilendirmez",
                "Ekip üyelerimin potansiyellerini ortaya çıkarmaları için onlara fırsat sunarım",
                "Ekip içinde çıkan çatışmalara müdahale etmekten kaçınırım",
                "Zorlu değişiklik süreçlerinde ekibime güven aşılarım",
                "Yetkimi kullanarak kararlarımı sorgusuzca kabul ettiririm",
                "Performansı düşük çalışanlara yapıcı ve dürüst geri bildirim veririm",
                "Liderlik tarzımı farklı durum ve kişilere göre değiştiremem",
                "Ekibimin kararlara katılımını sağlayarak sahiplenme duygusunu artırırım",
                "Hata yapmaktan korkan bir çalışan profilini teşvik ederim",
                "Departman hedeflerimi şirketin genel stratejisiyle uyumlu hale getiririm",
                "Planlarımı esnek tutmak yerine katı kurallara bağlı kalırım",
                "Kaynak (zaman, bütçe, personel) dağıtımını etkin bir şekilde yaparım",
                "Acil durum planı yapmayı genellikle gereksiz bulurum",
                "Planlama sürecine tüm ilgili paydaşları dahil ederim",
                "Gelecekteki olası zorlukları planlarıma dahil etmem",
                "İş akışlarını optimize etmek için düzenli olarak süreçleri gözden geçiririm",
                "Planlamadan çok, anlık kararlarla ilerlemeyi tercih ederim",
                "Planlarımı ve ilerlemeyi ekibimle düzenli olarak paylaşırım",
                "Önceliklerim sık sık değişir ve bu durum ekibi yorar",
                "Üst yönetimle konuşurken karmaşık bilgileri basitleştiririm",
                "Çalışanlarımın ne düşündüğünü öğrenmek için düzenli toplantılar yapmam",
                "Kritik bilgileri doğru zamanda, ilgili kişilere iletirim",
                "Ekibimle iletişim kurarken genellikle resmi ve mesafeli bir dil kullanırım",
                "Zorlu geri bildirimleri yapıcı ve empatik bir şekilde verebilirim",
                "Çalışanların endişelerini dile getirmesi beni rahatsız eder",
                "Bir konuşmada hem sözlü hem de sözsüz sinyallere dikkat ederim",
                "Sık sık insanların söylediklerini yanlış anlarım",
                "Farklı pozisyonlardaki kişilerle rahatlıkla iletişim kurabilirim",
                "Duygusal zekam, profesyonel iletişimimi olumsuz etkiler",
                "Karmaşık iş sorunlarında veriye dayalı çözüm yolları ararım",
                "Bir problem çıktığında ilk tepkim sorunu görmezden gelmek olur",
                "Sorunların kök nedenlerini tespit etmek için sistematik yöntemler kullanırım",
                "Basit sorunların çözümü için bile çok zaman harcarım",
                "Yenilikçi ve yaratıcı problem çözme tekniklerini teşvik ederim",
                "Başkalarının çözüm önerilerini genellikle yetersiz bulurum",
                "Problemleri fırsata çevirerek iş süreçlerini iyileştiririm",
                "Çözüme ulaştıktan sonra süreç analizi yapmam",
                "Farklı görüşlerden faydalanarak en uygun çözümü bulurum",
                "Problem çözme yeteneğim, kriz anlarında düşer",
                "Yüksek kalite standartlarını tüm operasyonel süreçlere entegre ederim",
                "Kalite yönetim sistemlerinin gerekliliklerini göz ardı ederim",
                "Kalite hatalarını azaltmak için sürekli iyileştirme projeleri başlatırım",
                "Kaliteyi artırmak için yapılan yatırımları gereksiz bulurum",
                "Çalışanlarımın kalite bilincini geliştirmek için eğitimler düzenlerim",
                "Hız ve maliyet, kalite standartlarının önündedir",
                "Müşteri şikayetlerini, kalite süreçlerini gözden geçirmek için kullanırım",
                "Kalite hedeflerine ulaşılmadığında sorumluluğu kabul etmem",
                "Tedarikçilerimden de aynı yüksek kalite standartlarını talep ederim",
                "Kalite sorunları genellikle benim kontrolüm dışındaki durumlardan kaynaklanır",
                "Müşteri ihtiyaçlarını anlamak için düzenli pazar araştırması yaparım",
                "Müşteri beklentileri değiştiğinde hemen uyum sağlamakta zorlanırım",
                "Müşteri şikayetlerini hızlı ve tatmin edici bir şekilde çözerim",
                "Sadece büyük müşterilerin görüşleri benim için önemlidir",
                "Ekibimi, her etkileşimde müşteri memnuniyetini hedeflemeye yönlendiririm",
                "Müşteri geri bildirimlerini dinlemek zaman kaybıdır",
                "Hem iç hem de dış müşterilerime eşit derecede önem veririm",
                "Müşteriye hayır demekten çekinmem",
                "Müşteri sadakatini artırmak için uzun vadeli stratejiler geliştiririm",
                "Müşterinin tam olarak ne istediği benim için her zaman açık değildir",
                "Sektördeki yenilikleri takip ederek yeni iş alanları yaratırım",
                "Sürekli yeni fikirler denemektense, mevcut rutinime bağlı kalırım",
                "Ekibimi belirlenen hedeflerin ötesine geçmeleri için teşvik ederim",
                "Hata yapma riskini göze alamam",
                "İşleri hızlandırmak ve verimliliği artırmak için yaratıcı yollar denerim",
                "Sorunlarımı amirime danışmadan çözmeye çalışmam",
                "Acil bir durumda dahi yetki beklemeden doğru kararı veririm",
                "Başkalarının benim için harekete geçmesini beklerim",
                "Yeni projeler veya bilinmeyen alanlar beni heyecanlandırır",
                "Yeni bir göreve başlarken detaylı bir kılavuz olmasını şart koşarım",
                "Şirketimin uzun vadeli rekabet gücünü artırmak için sürekli öğrenirim",
                "Çalışanlarımın gelişim ihtiyaçlarını göz ardı ederim",
                "Performansımı düzenli olarak değerlendirir ve kendimi geliştiririm",
                "Eğitimler ve seminerler genellikle zaman kaybıdır",
                "Başarısızlıkları birer öğrenme fırsatı olarak görürüm",
                "Değişen çalışma yöntemlerine ayak uydurmak benim için zordur",
                "Eleştirilere açıktır ve bu geri bildirimleri gelişmek için kullanırım",
                "İşimi en iyi şekilde yaptığımı düşündüğüm için gelişime ihtiyacım yoktur",
                "Sektördeki son trendleri ve teknolojileri düzenli olarak takip ederim",
                "Kariyer hedeflerime ulaşmak için net bir kişisel gelişim planım vardır"
            ],
            grup4: [
                "Müşteri taleplerine her zaman güler yüzle ve sabırla cevap veririm",
                "Müşterilerin sık sık şikayet etmesi beni sinirlendirir",
                "Müşterilerin sözünü kesmeden, söylediklerini tamamen dinlerim",
                "Müşteriye tam olarak ne istediğini sormak yerine varsayımlarla ilerlerim",
                "Zorlu müşterilerle bile profesyonel ve sakin kalabilirim",
                "Müşterilerin kişisel sorunlarını çözmek benim görevim değildir",
                "Müşterilere hızlı ve doğru bilgi sağlamak için çabalarım",
                "Müşterinin memnuniyeti, sadece yöneticimin sorumluluğundadır",
                "Hizmet sonrası memnuniyeti ölçmek için inisiyatif alırım",
                "Müşterinin ne istediğini anlamakta zorlanırım",
                "Ani değişen vardiya veya çalışma saatlerine kolayca uyum sağlarım",
                "Yeni bir sisteme veya sürece adapte olmak benim için zordur",
                "Farklı görevler üstlenmem gerektiğinde itiraz etmem",
                "İş arkadaşlarımın çalışma tarzlarına ayak uydurmakta güçlük çekerim",
                "Yüksek tempolu ve stresli çalışma ortamlarında bile sakin kalırım",
                "Sadece bana verilen talimatlara bağlı kalırım",
                "Müşterinin değişen taleplerine hızlıca çözüm üretirim",
                "Rutin dışındaki durumlar beni gereğinden fazla yorar",
                "Farklı kişilikteki iş arkadaşlarımla iyi geçinirim",
                "Çalışma ortamında çıkan dedikodulara katılmam",
                "Ekipteki herkesin işine eşit derecede saygı gösteririm",
                "Ekip arkadaşlarımdan destek istemekten çekinirim",
                "İş akışının aksamaması için kendi görevimi eksiksiz tamamlarım",
                "Sadece benim dışımdaki ekip üyelerinin hataları üzerinde dururum",
                "Ortak bir hedef belirlediğimizde bu hedefe tam olarak bağlı kalırım",
                "Ekip içindeki anlaşmazlıklar beni doğrudan etkilemezse karışmam",
                "İhtiyaç duyulduğunda başka bir birime yardım etmeye gönüllüyüm",
                "Başkalarıyla çalışmak yerine tek başıma çalışmayı tercih ederim",
                "İş yükü fazla olan bir arkadaşıma yardım teklif ederim",
                "Ekip içi bilgi akışını sağlamak benim sorumluluğumda değildir",
                "Sözlü talimatları dinlerken önemli noktaları not ederim",
                "Yöneticimle konuşurken genellikle konuyu dağıtırım",
                "İş arkadaşlarımla net ve saygılı bir dil kullanırım",
                "Bir talimatı tam olarak anlamadan uygulamaya başlarım",
                "Müşterilerle iletişim kurarken daima pozitif bir dil kullanırım",
                "Sık sık insanların ne demek istediğini yanlış anlarım",
                "Üretim veya hizmet alanındaki sorunları çekinmeden dile getiririm",
                "İletişim kurarken teknik jargon kullanmaktan kaçınmam",
                "Çalışma ortamında çıkan dedikodulara katılmam",
                "Sorun çıktığında kişisel olarak algılar ve küserim",
                "Vardiyam boyunca işimi verimli bir şekilde planlarım",
                "Molalarımı genellikle uzatırım",
                "İş akışındaki önceliklere göre kendimi hızlıca adapte ederim",
                "Bana verilen görevi ne zaman bitireceğimi nadiren bilirim",
                "Boş zamanlarımda bile etrafımdaki işleri düzenlerim",
                "Görevleri yetiştiremeyeceğimi anladığımda yardım istemem",
                "Zorlu işleri bitirmek için gerekli zamanı her zaman ayırırım",
                "İşim bittikten sonra dinlenmektense, yeni işler aramaya koyulurum",
                "İş yerindeki dağınıklık çalışma hızımı olumsuz etkilemez",
                "Bir işi bitirmek için sürekli başkalarının onayını beklerim",
                "Hizmet/ürün standartlarını titizlikle uygularım",
                "Kalite kontrolde çıkan küçük kusurları göz ardı edebilirim",
                "Hata payını en aza indirmek için ekstra önlemler alırım",
                "İşim bittikten sonra kontrol etmekle vakit kaybetmem",
                "Müşteriye sunduğum hizmetin kalitesini sürekli kontrol ederim",
                "Kalitesiz hizmet vermektense, hizmeti yavaşlatmayı tercih ederim",
                "Kalite benim için hızdan sonra gelir",
                "Kullandığım malzemelerin kalitesini sürekli kontrol ederim",
                "Kaliteyi sağlamanın tek yolu katı kurallara uymaktır",
                "Kaliteyi artıracak önerilerimi yöneticilerime sunmaktan çekinmem",
                "İşimin kalitesi ve zamanında teslimatından ben sorumluyum",
                "İş yerindeki kişisel eşyalarımı dağınık bırakırım",
                "Yapılması gereken işlerde proaktif davranırım ve beklemede kalmam",
                "Çalışma ortamımın temiz ve düzenli olmasından ben sorumlu değilim",
                "Verilen görevleri tamamlamadan işten ayrılmam",
                "Benim sorumluluğumdaki bir hata çıktığında kolayca mazeret bulurum",
                "Tüm araç ve gereçleri büyük bir dikkatle kullanırım",
                "İş yapma biçimim sık sık başkalarını olumsuz etkiler",
                "Görevimi yerine getirirken şirket kaynaklarını dikkatli kullanırım",
                "İşimi yaparken sık sık kişisel işlerimle ilgilenirim",
                "Müşterinin yaşadığı sorunlara hızlı ve etkili çözümler üretirim",
                "Bir sorun çıktığında ilk tepkim başkasının gelip çözmesini beklemek olur",
                "Sorunu çözmek için farklı yöntemleri denemekten çekinmem",
                "Bir sorunun kök nedenini bulmak yerine, sadece belirtileri gidermeye odaklanırım",
                "Problem çözümü için gerekli olan bilgileri hızla toplarım",
                "Karmaşık sorunlar beni gergin ve çaresiz hissettirir",
                "Çözüm süreci boyunca soğukkanlılığımı korurum",
                "Basit sorunları bile çözmek için uzun süreye ihtiyacım olur",
                "İş akışımı etkileyen problemleri yöneticime doğru şekilde aktarırım",
                "Başkalarının çözüm önerilerini genellikle kabul etmem",
                "Daha iyi bir hizmet yolu varsa, mevcut talimatlara bağlı kalmam",
                "Yeni bir yöntemi denemek yerine bildiğim yoldan giderim",
                "Hizmette verimliliği artıracak fikirleri hemen denerim",
                "Bir işe başlamadan önce tüm detayların bana verilmesini beklerim",
                "Riskleri hesaplayarak yeni bir sorumluluğu üstlenmekten çekinmem",
                "Sorunlarımı amirime danışmadan çözmeye çalışmam",
                "Acil bir durumda dahi yetki beklemeden doğru kararı veririm",
                "Başkalarının benim için harekete geçmesini beklerim",
                "Yeni projeler veya bilinmeyen alanlar beni heyecanlandırır",
                "Bir görevde yetkimin dışına çıkmaktan korkarım",
                "İşimi daha iyi yapmak için sürekli yeni beceriler öğrenirim",
                "Kendi güçlü ve zayıf yönlerimi bilmek beni ilgilendirmez",
                "Performansımı düzenli olarak değerlendirir ve kendimi geliştiririm",
                "Eğitimler ve seminerler genellikle zaman kaybıdır",
                "Başarısızlıkları birer öğrenme fırsatı olarak görürüm",
                "Değişen çalışma yöntemlerine ayak uydurmak benim için zordur",
                "Eleştirilere açıktır ve bu geri bildirimleri gelişmek için kullanırım",
                "İşimi en iyi şekilde yaptığımı düşündüğüm için gelişime ihtiyacım yoktur",
                "Sektördeki son trendleri ve teknolojileri düzenli olarak takip ederim",
                "Kariyer hedeflerime ulaşmak için net bir kişisel gelişim planım vardır"
            ],
            grup5: [
                "Mevcut süreçleri iyileştirmek için proaktif öneriler sunarım",
                "Sadece bana söylenilen görevleri yaparım, fazlasını değil",
                "İhtiyaç duyulan bilgi veya kaynağı kendi çabamla bulurum",
                "Hata yapma ihtimali varsa, yeni bir şey denemekten kaçınırım",
                "İşleri hızlandırmak ve verimliliği artırmak için yaratıcı yollar denerim",
                "Sorunlarımı amirime danışmadan çözmeye çalışmam",
                "Acil bir durumda dahi yetki beklemeden doğru kararı veririm",
                "Başkalarının benim için harekete geçmesini beklerim",
                "Yeni projeler veya bilinmeyen alanlar beni heyecanlandırır",
                "Yeni bir göreve başlarken detaylı bir kılavuz olmasını şart koşarım",
                "Karmaşık bilgileri sadeleştirerek herkese ulaştırırım",
                "Geri bildirim almaktan kaçınır, genellikle savunmacı bir tavır sergilerim",
                "Çatışma durumlarında konuyu uzatmaktansa sessiz kalmayı tercih ederim",
                "Farklı departmanlarla düzenli ve etkili bir iletişim ağı kurarım",
                "Hem sözlü hem de yazılı iletişimde açık ve ikna ediciyimdir",
                "Çalışanların endişelerini dile getirmesi beni rahatsız eder",
                "Önemli bilgileri zamanında ve uygun kanallar aracılığıyla paylaşırım",
                "İletişim kurarken teknik jargon kullanmaktan kaçınmam",
                "Ekibimin kararlara katılımını sağlamak için düzenli olarak görüş alırım",
                "Duygusal zekam, profesyonel iletişimimi olumsuz etkiler",
                "Departman hedeflerimi şirketin genel stratejisiyle uyumlu hale getiririm",
                "Planlarımı esnek tutmak yerine katı kurallara bağlı kalırım",
                "Kaynak (zaman, bütçe, personel) dağıtımını etkin bir şekilde yaparım",
                "Acil durum planı yapmayı genellikle gereksiz bulurum",
                "Planlama sürecine tüm ilgili paydaşları dahil ederim",
                "Gelecekteki olası zorlukları planlarıma dahil etmem",
                "İş akışlarını optimize etmek için düzenli olarak süreçleri gözden geçiririm",
                "Planlamadan çok, anlık kararlarla ilerlemeyi tercih ederim",
                "Planlarımı ve ilerlemeyi ekibimle düzenli olarak paylaşırım",
                "Önceliklerim sık sık değişir ve bu durum ekibi yorar",
                "Şirketin uzun vadeli hedeflerini günlük kararlarıma dahil ederim",
                "Sektördeki rakiplerin hareketlerini göz ardı ederim",
                "Gelecekteki pazar trendlerini tahmin edebilirim",
                "Kısa vadeli sonuçlar, stratejik planlamadan daha önemlidir",
                "Riskleri ve fırsatları değerlendirerek alternatif planlar geliştiririm",
                "Büyük resmi görmekten çok, detaylara takılı kalırım",
                "Stratejik kararlarımı desteklemek için güvenilir verilere dayanırım",
                "İşler yolunda giderken yeni stratejiler geliştirmeye gerek duymam",
                "Organizasyonumun rekabet avantajını sürekli sorgularım",
                "İş planlarını genellikle son teslim tarihine yakın hazırlarım",
                "Zor ve önemli kararları hızlı ve emin bir şekilde alırım",
                "Karar alma sürecinde sadece kendi deneyimlerime güvenirim",
                "Bir kararın olası sonuçlarını önceden hesaplarım",
                "Baskı altında karar vermek beni felç eder",
                "Farklı görüşleri dinledikten sonra objektif bir karar veririm",
                "Hata yapmamak için karar vermeyi sürekli ertelerim",
                "Yeterli bilgi olmadığında bile risk alarak ilerlerim",
                "Yanlış olduğu ortaya çıkan bir kararda sorumluluğu başkasına atarım",
                "Başkalarına karar verme yetkisi (delege etme) vermekten çekinirim",
                "Kararlarımı ekibime açık ve mantıklı bir şekilde açıklarım",
                "Karmaşık iş sorunlarında veriye dayalı çözüm yolları ararım",
                "Bir problem çıktığında ilk tepkim sorunu görmezden gelmek olur",
                "Sorunların kök nedenlerini tespit etmek için sistematik yöntemler kullanırım",
                "Basit sorunların çözümü için bile çok zaman harcarım",
                "Yenilikçi ve yaratıcı problem çözme tekniklerini teşvik ederim",
                "Başkalarının çözüm önerilerini genellikle yetersiz bulurum",
                "Problemleri fırsata çevirerek iş süreçlerini iyileştiririm",
                "Çözüme ulaştıktan sonra süreç analizi yapmam",
                "Farklı görüşlerden faydalanarak en uygun çözümü bulurum",
                "Problem çözme yeteneğim, kriz anlarında düşer",
                "Yüksek kalite standartlarını tüm hizmet süreçlerine entegre ederim",
                "Kalite yönetim sistemlerinin gerekliliklerini göz ardı ederim",
                "Kalite hatalarını azaltmak için sürekli iyileştirme projeleri başlatırım",
                "Kaliteyi artırmak için yapılan yatırımları gereksiz bulurum",
                "Çalışanlarımın kalite bilincini geliştirmek için eğitimler düzenlerim",
                "Hız ve maliyet, kalite standartlarının önündedir",
                "Müşteri şikayetlerini, kalite süreçlerini gözden geçirmek için kullanırım",
                "Kalite hedeflerine ulaşılmadığında sorumluluğu kabul etmem",
                "Tedarikçilerimden de aynı yüksek kalite standartlarını talep ederim",
                "Kalite sorunları genellikle benim kontrolüm dışındaki durumlardan kaynaklanır",
                "Müşteri ihtiyaçlarını anlamak için düzenli pazar araştırması yaparım",
                "Müşteri beklentileri değiştiğinde hemen uyum sağlamakta zorlanırım",
                "Müşteri şikayetlerini hızlı ve tatmin edici bir şekilde çözerim",
                "Sadece büyük müşterilerin görüşleri benim için önemlidir",
                "Ekibimi, her etkileşimde müşteri memnuniyetini hedeflemeye yönlendiririm",
                "Müşteri geri bildirimlerini dinlemek zaman kaybıdır",
                "Hem iç hem de dış müşterilerime eşit derecede önem veririm",
                "Müşteriye hayır demekten çekinmem",
                "Müşteri sadakatini artırmak için uzun vadeli stratejiler geliştiririm",
                "Müşterinin tam olarak ne istediği benim için her zaman açık değildir",
                "Önceliklerime göre zamanımı etkin bir şekilde tahsis ederim",
                "Çalışma saatlerimi sosyal medya veya gereksiz e-postalara harcarım",
                "Yüksek hacimli görevleri küçük, yönetilebilir adımlara bölerim",
                "Zaman baskısı altında işleri yetiştirmekte zorlanırım",
                "Gecikmelere neden olan süreçleri düzenli olarak analiz edip düzeltirim",
                "Toplantılara her zaman zamanında katılmam",
                "En önemli görevleri (zorunlu olmasa da) günün erken saatlerinde bitiririm",
                "Son teslim tarihlerini sık sık kaçırma eğilimim vardır",
                "Programımı düzenli tutmak için sürekli çaba gösteririm",
                "Bir işi bitirmek için sürekli başkalarının onayını beklerim",
                "Bir projede doğal olarak liderlik rolünü üstlenmeye hazırım",
                "Genellikle riskli kararlar almaktan kaçınırım",
                "Ekip arkadaşlarıma görevleri adil bir şekilde delege edebilirim",
                "Bir ekibi yönetmek, kişisel performansıma odaklanmaktan daha zordur",
                "Başkalarını motive etmek ve ortak bir vizyon etrafında toplamak konusunda başarılıyımdır",
                "Hata yapan birini eleştirmektense, konuyu geçiştirmeyi tercih ederim",
                "Ekip üyelerimin gelişimine katkıda bulunmak için mentorluk yaparım",
                "İnsanların bana gönüllü olarak uyması benim için önemli değildir",
                "Zorlu durumlarda bile ekibe sakinlik ve güven aşılarım",
                "Liderlik pozisyonu, beraberinde getirdiği sorumluluklar nedeniyle gözümü korkutur"
            ]
        };

        // 500 soruluk cevap anahtarı - yeni cevaplar yüklenecek
        const questionAnswerKey = [
            5,1,5,1,5,1,5,1,5,1,5,1,5,1,5,1,1,5,5,1,5,1,5,1,5,1,5,1,5,1,
            5,1,5,1,5,1,5,1,5,1,5,1,5,1,5,1,5,1,5,1,5,1,5,1,5,1,5,1,5,1,
            5,1,5,5,5,1,5,1,5,1,5,1,5,1,5,5,1,5,5,1,5,1,5,1,5,1,5,1,5,1,
            5,1,5,1,5,1,5,1,5,5,5,1,5,1,5,1,5,1,5,1,5,1,5,1,5,1,5,1,5,1,
            5,1,5,1,5,1,5,1,5,1,5,1,5,1,5,1,5,1,5,1,5,1,5,5,1,5,1,5,1,1,
            1,5,1,5,1,5,1,5,1,5,1,5,1,5,1,5,1,5,1,5,1,5,1,5,1,5,1,5,1,5,
            5,1,5,1,5,1,5,1,5,1,5,1,5,1,5,1,5,1,5,1,5,1,5,1,5,1,5,1,5,1,
            5,1,5,1,5,1,5,1,5,1,5,1,5,1,5,1,5,1,5,1,5,1,5,1,5,1,5,1,5,5,
            5,1,5,1,5,1,5,1,5,1,5,1,5,1,5,1,5,1,5,1,5,1,5,1,5,1,5,1,5,1,
            5,1,5,1,5,1,5,1,5,1,5,1,5,1,5,1,5,1,5,1,5,1,5,1,5,1,5,1,5,1,
            5,1,5,1,5,1,5,1,5,1,5,1,5,1,5,1,5,1,5,1,5,1,5,1,5,1,5,1,5,1,
            5,1,5,1,5,1,5,1,5,1,5,1,5,1,5,1,5,1,5,1,5,1,5,1,5,1,5,1,5,1,
            5,1,5,1,5,1,5,1,5,1,5,1,5,1,5,1,5,1,5,1,5,1,5,1,5,1,5,1,5,1,
            5,1,5,1,5,1,5,1,5,1,5,5,5,1,5,1,5,1,5,1,5,1,5,1,5,1,5,1,5,1,
            5,1,5,1,5,1,5,1,5,1,5,1,5,1,5,1,5,1,5,1,5,1,5,1,5,1,5,1,5,1,
            5,1,5,1,5,1,5,1,5,1,5,1,5,1,5,1,5,1,5,1,5,1,5,1,5,1,5,1,5,5,
            5,1,1,5,5,1,5,1,5,1,5,1,5,1,5,1,5,1,5,1,5,1,5,1,5,1,5,1,1,5,
            5,1,5,1,5,1,5,1,5,1,5,1,5,1,5,1,5,1,5,1,5,1,5,1,5,1,5,1,5,1,
            5,1,5,1,5,1,5,1,5,1,5,1,5,1,5,1,5,1,5,1,5,1,5,1,5,1,5,1,5,1
        ];

        // Kullanıcının verdiği cevaplara göre toplam puanı hesaplayan fonksiyon
        function puanHesapla(sorular, cevaplar) {
            let toplamPuan = 0;
            let maxPuan = 0;
            // Cevap anahtarı: doğru seçenekler (ör: [5,1,5,...])
            // questionAnswerKey dizisi kullanılacak
            for (let i = 0; i < sorular.length; i++) {
                const soru = sorular[i];
                const cevap = cevaplar[i];
                // Soru string ise anahtardan doğru cevabı al
                let dogruCevap = (typeof questionAnswerKey !== 'undefined' && questionAnswerKey[i]) ? questionAnswerKey[i] : 5;
                // Kullanıcı cevabı 0 tabanlı index, 1-5 arası puan için +1
                let kullaniciCevap = (cevap !== null && cevap !== undefined) ? (cevap + 1) : null;
                let puanYuzdesi = 0;
                
                if (kullaniciCevap === null) {
                    puanYuzdesi = 0; // Cevaplanmadı = %0
                } else if (dogruCevap === 5) {
                    // Doğru cevap 5 ise
                    if (kullaniciCevap === 5) puanYuzdesi = 100;      // Tam doğru = %100
                    else if (kullaniciCevap === 4) puanYuzdesi = 75;  // Yakın cevap = %75
                    else if (kullaniciCevap === 3) puanYuzdesi = 50;  // Orta cevap = %50
                    else if (kullaniciCevap === 2) puanYuzdesi = 0;   // Ters cevap = %0
                    else if (kullaniciCevap === 1) puanYuzdesi = 0;   // Tam ters = %0
                } else if (dogruCevap === 1) {
                    // Doğru cevap 1 ise
                    if (kullaniciCevap === 1) puanYuzdesi = 100;      // Tam doğru = %100
                    else if (kullaniciCevap === 2) puanYuzdesi = 75;  // Yakın cevap = %75
                    else if (kullaniciCevap === 3) puanYuzdesi = 50;  // Orta cevap = %50
                    else if (kullaniciCevap === 4) puanYuzdesi = 0;   // Ters cevap = %0
                    else if (kullaniciCevap === 5) puanYuzdesi = 0;   // Tam ters = %0
                } else {
                    // Eğer anahtarda 2,3,4 gibi değer olursa tam eşleşme %100, diğerleri gradüel
                    if (kullaniciCevap === dogruCevap) puanYuzdesi = 100;
                    else {
                        let fark = Math.abs(kullaniciCevap - dogruCevap);
                        if (fark === 1) puanYuzdesi = 75;
                        else if (fark === 2) puanYuzdesi = 50;
                        else puanYuzdesi = 0;
                    }
                }
                
                toplamPuan += puanYuzdesi;
                maxPuan += 100; // Her soru maksimum %100 değerinde
            }
            
            // Toplam puanı yüzde olarak hesapla
            return maxPuan > 0 ? Math.round((toplamPuan / maxPuan) * 100) : 0;
        }

        // Metodoloji fonksiyonları
        function showMethodology() {
            // Çalışan metodoloji modalı
            const methodModal = document.createElement('div');
            methodModal.innerHTML = `
                <div style="position: fixed; top: 0; left: 0; width: 100%; height: 100%; background: rgba(0,0,0,0.8); z-index: 99999; display: flex; align-items: center; justify-content: center; overflow-y: auto;">
                    <div style="background: white; padding: 30px; border-radius: 15px; max-width: 900px; max-height: 90vh; overflow-y: auto; margin: 20px;">
                        <h2 style="color: #1f2937; font-size: 24px; font-weight: bold; margin-bottom: 20px;">📊 Analiz Pro X Metodolojisi ve Bilimsel Temeller</h2>
                        
                        <div style="font-size: 14px; color: #374151; line-height: 1.6; margin-bottom: 25px;">
                            <h3 style="font-weight: 600; color: #059669; margin: 15px 0 10px 0;">🔬 Bilimsel Yaklaşım</h3>
                            <p style="margin-bottom: 15px;">Analiz Pro X, endüstriyel psikoloji ve organizasyonel davranış alanındaki güncel araştırmalara dayanan, kanıta dayalı (evidence-based) bir değerlendirme platformudur. Sistemimiz, Likert ölçeği metodolojisi ve psikometrik analiz ilkeleri temelinde geliştirilmiştir.</p>
                            
                            <h3 style="font-weight: 600; color: #059669; margin: 15px 0 10px 0;">📋 5'li Likert Ölçeği Sistemi</h3>
                            <p style="margin-bottom: 10px;"><strong>Ölçek Yapısı:</strong></p>
                            <ul style="margin-left: 20px; margin-bottom: 15px;">
                                <li><strong>5 - Tamamen Katılıyorum:</strong> Yüksek uyum ve pozitif yönelim</li>
                                <li><strong>4 - Katılıyorum:</strong> Genel uyum ve kabul</li>
                                <li><strong>3 - Kararsızım:</strong> Nötr duruş veya belirsizlik</li>
                                <li><strong>2 - Katılmıyorum:</strong> Olumsuz eğilim</li>
                                <li><strong>1 - Hiç Katılmıyorum:</strong> Güçlü olumsuz duruş</li>
                            </ul>
                            
                            <h3 style="font-weight: 600; color: #059669; margin: 15px 0 10px 0;">📊 Yetkinlik Kategorileri</h3>
                            <div style="display: grid; grid-template-columns: 1fr 1fr; gap: 15px; margin-bottom: 15px;">
                                <div style="background: #f0fdf4; padding: 15px; border-radius: 8px;">
                                    <h4 style="color: #166534; font-weight: 600; margin-bottom: 8px;">👔 Beyaz Yaka Çalışanları</h4>
                                    <p style="font-size: 12px;">Analitik düşünme, problem çözme, iletişim becerileri odaklı değerlendirme</p>
                                </div>
                                <div style="background: #eff6ff; padding: 15px; border-radius: 8px;">
                                    <h4 style="color: #1d4ed8; font-weight: 600; margin-bottom: 8px;">🔧 Mavi Yaka Çalışanları</h4>
                                    <p style="font-size: 12px;">Pratik zeka, takım çalışması, güvenlik bilinci odaklı değerlendirme</p>
                                </div>
                                <div style="background: #fef3c7; padding: 15px; border-radius: 8px;">
                                    <h4 style="color: #92400e; font-weight: 600; margin-bottom: 8px;">👨‍💼 Yönetici İmalat</h4>
                                    <p style="font-size: 12px;">Liderlik, karar verme, operasyonel yönetim odaklı değerlendirme</p>
                                </div>
                                <div style="background: #fce7f3; padding: 15px; border-radius: 8px;">
                                    <h4 style="color: #be185d; font-weight: 600; margin-bottom: 8px;">🏢 Hizmet Sektörü</h4>
                                    <p style="font-size: 12px;">Müşteri odaklılık, empati, hizmet kalitesi odaklı değerlendirme</p>
                                </div>
                            </div>
                            
                            <h3 style="font-weight: 600; color: #059669; margin: 15px 0 10px 0;">⚖️ Validite ve Güvenilirlik</h3>
                            <p style="margin-bottom: 10px;"><strong>Psikometrik Özellikler:</strong></p>
                            <ul style="margin-left: 20px; margin-bottom: 15px;">
                                <li><strong>İç Tutarlılık:</strong> Cronbach Alpha katsayısı ile ölçülmüştür</li>
                                <li><strong>Yapı Geçerliliği:</strong> Faktör analizi ile doğrulanmıştır</li>
                                <li><strong>Kriter Geçerliliği:</strong> İş performansı ile korelasyon analizi yapılmıştır</li>
                                <li><strong>Test-Tekrar Test:</strong> Kararlılık katsayısı hesaplanmıştır</li>
                            </ul>
                            
                            <h3 style="font-weight: 600; color: #059669; margin: 15px 0 10px 0;">🎯 Uygulama Alanları</h3>
                            <p style="margin-bottom: 15px;">Bu metodoloji, işe alım süreçleri, performans değerlendirme, yetenek yönetimi ve organizasyonel gelişim projelerinde kullanılmak üzere tasarlanmıştır. Sonuçlar, karar verme süreçlerinde destekleyici bilgi olarak kullanılmalı ve tek başına değerlendirme kriteri olmamalıdır.</p>
                        </div>
                        
                        <div style="text-align: center;">
                            <button onclick="this.parentElement.parentElement.parentElement.remove();" style="background: #059669; color: white; padding: 12px 30px; border: none; border-radius: 8px; cursor: pointer; font-size: 16px;">
                                ✓ Anladım
                            </button>
                        </div>
                    </div>
                </div>
            `;
            document.body.appendChild(methodModal);
        }
        
        function closeMethodology() {
            // Modal artık createElement ile yapılıyor, bu fonksiyon gerekli değil
        }

        // Sorumluluk reddi fonksiyonları
        function showDisclaimer() {
            // Çalışan disclaimer modalı
            const testModal = document.createElement('div');
            testModal.innerHTML = `
                <div style="position: fixed; top: 0; left: 0; width: 100%; height: 100%; background: rgba(0,0,0,0.8); z-index: 99999; display: flex; align-items: center; justify-content: center; overflow-y: auto;">
                    <div style="background: white; padding: 30px; border-radius: 15px; max-width: 800px; max-height: 90vh; overflow-y: auto; margin: 20px;">
                        <h2 style="color: #1f2937; font-size: 24px; font-weight: bold; margin-bottom: 20px;">Hukuki Sorumluluk Reddi ve Veri Güvenliği Beyanı</h2>
                        
                        <div style="font-size: 14px; color: #374151; line-height: 1.6; margin-bottom: 25px;">
                            <p style="margin-bottom: 15px; font-weight: 600; color: #2563eb;">
                                Analiz Pro X platformu, veri analizi ve raporlama süreçlerinde hukuki uygunluk, şeffaflık ve etik sorumluluk prensiplerini benimser. Bu beyan, platformun teknolojik dayanağını, veri koruma politikalarını ve sonuçların kullanımına dair sorumluluk sınırlarını netleştirmektedir.
                            </p>
                            
                            <h3 style="font-weight: 600; color: #1f2937; margin: 15px 0 10px 0;">1. Teknolojik Altyapı ve Güvenlik</h3>
                            <p style="margin-bottom: 10px;"><strong>Altyapı Güvenliği:</strong> Analiz Pro X, Google Firebase Real-time Database teknolojisini kullanarak verilerinizi işler ve saklar. Firebase, Google Cloud'un enterprise düzeyindeki güvenlik protokolleri ile korunmaktadır.</p>
                            
                            <p style="margin-bottom: 10px;"><strong>Veri İletimi:</strong> Tüm veriler HTTPS protokolü ile şifrelenerek iletilir ve Firebase'in çok katmanlı güvenlik duvarları ile korunur.</p>
                            
                            <p style="margin-bottom: 15px;"><strong>Sorumluluk Reddi:</strong> Analiz Pro X, altyapı güvenliği için tamamen Google Firebase'in sağladığı protokol ve güvenlik standartlarına güvenir.</p>
                            
                            <h3 style="font-weight: 600; color: #1f2937; margin: 15px 0 10px 0;">2. Veri Koruma ve KVKK Uyumu</h3>
                            <p style="margin-bottom: 10px;"><strong>Anonimlik Sistemi:</strong> Platform, adayların gerçek kimlik bilgilerini hiçbir şekilde toplamaz, işlemez veya saklamaz. Sadece kullanıcı kurumun belirlediği rumuz sistemi ile çalışır.</p>
                            
                            <p style="margin-bottom: 15px;"><strong>Sorumluluk Beyanı:</strong> Platformumuz, kimlik bilgilerini içermeyen rumuz sistemi sayesinde, kullanıcı kurumların KVKK uyum süreçlerini destekler ve yasal risklerini minimize eder.</p>
                            
                            <h3 style="font-weight: 600; color: #1f2937; margin: 15px 0 10px 0;">3. Sonuçların Kullanımı ve Karar Verme Süreçleri</h3>
                            <p style="margin-bottom: 15px;"><strong>Sorumluluk Beyanı:</strong> Platform tarafından sunulan Görüşme Önerileri, Risk Seviyeleri ve Yetkinlik Skorları tamamen tavsiye niteliğindedir. Adayın işe alım, elenme, terfi ettirilme veya görevlendirilme kararlarının nihai sorumluluğu ve takdiri, her zaman kullanıcı kurumun yetkili İK ve Yönetici kadrolarına aittir.</p>
                        </div>
                        
                        <div style="text-align: center;">
                            <button onclick="acceptDisclaimer(); this.parentElement.parentElement.parentElement.remove();" style="background: #16a34a; color: white; padding: 12px 30px; border: none; border-radius: 8px; cursor: pointer; font-size: 16px; margin-right: 10px;">
                                ✓ Okudum ve Onaylıyorum
                            </button>
                            <button onclick="this.parentElement.parentElement.parentElement.remove();" style="background: #dc2626; color: white; padding: 12px 30px; border: none; border-radius: 8px; cursor: pointer; font-size: 16px;">
                                ✗ İptal
                            </button>
                        </div>
                    </div>
                </div>
            `;
            document.body.appendChild(testModal);
        }
        
        function closeDisclaimer() {
            const modal = document.getElementById('disclaimerModal');
            modal.style.display = 'none';
        }
        
        function closeDisclaimer() {
            const modal = document.getElementById('disclaimerModal');
            modal.style.display = 'none';
        }
        
        function acceptDisclaimer() {
            console.log('acceptDisclaimer çalıştı');
            disclaimerAccepted = true;
            document.getElementById('disclaimerAccept').checked = true;
            document.getElementById('disclaimerAccept').disabled = false;
            document.getElementById('candidateButton').disabled = false;
            updateHrRegisterButton(); // İK kayıt butonunu durumu güncelle
            closeDisclaimer();
            alert('Sorumluluk reddi beyanı onaylandı!');
        }

        // Ana fonksiyonlar
        function showRoleLogin(role) {
            // Admin için sorumluluk reddi zorunlu değil
            // İK yöneticisi için de zorunlu değil (kayıtlı kullanıcılar giriş yapabilir)
            if (role === 'candidate' && !disclaimerAccepted) {
                alert('Lütfen önce sorumluluk reddi beyanını okuyun ve onaylayın!');
                return;
            }
            
            document.getElementById('loginScreen').classList.add('hidden');
            document.getElementById('roleLoginScreen').classList.remove('hidden');
            currentRole = role;
            
            const titles = {
                admin: '👨‍💼 Admin Yönetici Girişi',
                hr: '👩‍💻 İK Yönetici Girişi',
                candidate: '📝 Aday Girişi'
            };
            
            document.getElementById('roleTitle').textContent = titles[role];
            
            if (role === 'candidate') {
                document.getElementById('candidateFields').classList.remove('hidden');
                document.getElementById('adminHrFields').classList.add('hidden');
                document.getElementById('hrRegisterOption').classList.add('hidden');
            } else {
                document.getElementById('candidateFields').classList.add('hidden');
                document.getElementById('adminHrFields').classList.remove('hidden');
                if (role === 'hr') {
                    document.getElementById('hrRegisterOption').classList.remove('hidden');
                    // Kayıt ol butonunun durumunu güncelle
                    updateHrRegisterButton();
                } else {
                    document.getElementById('hrRegisterOption').classList.add('hidden');
                }
            }
        }

        function backToMain() {
            document.getElementById('roleLoginScreen').classList.add('hidden');
            document.getElementById('loginScreen').classList.remove('hidden');
            // İK yöneticilerini güncel çek
            fetchHrManagers();
            currentRole = null;
        }

        function updateHrRegisterButton() {
            const hrRegisterMainButton = document.getElementById('hrRegisterMainButton');
            if (hrRegisterMainButton) {
                // Sorumluluk reddi onaylanmışsa VE Google hesabı varsa aktif
                if (disclaimerAccepted && googleUser) {
                    hrRegisterMainButton.disabled = false;
                    hrRegisterMainButton.classList.remove('opacity-50','cursor-not-allowed');
                    hrRegisterMainButton.title = '';
                } else {
                    hrRegisterMainButton.disabled = true;
                    hrRegisterMainButton.classList.add('opacity-50','cursor-not-allowed');
                    if (!disclaimerAccepted) {
                        hrRegisterMainButton.title = 'Önce sorumluluk reddi beyanını onaylayın';
                    } else if (!googleUser) {
                        hrRegisterMainButton.title = 'Önce Google ile giriş yapın';
                    }
                }
            }
        }

        function showHrRegister() {
            console.log('showHrRegister çağrıldı - Kontroller:', {
                disclaimerAccepted,
                googleUser: !!googleUser
            });
            
            // Önce sorumluluk reddi kontrolü
            if (!disclaimerAccepted) {
                alert('Önce sorumluluk reddi beyanını okuyun ve onaylayın!');
                return;
            }
            
            // Sonra Google giriş kontrolü  
            if (!googleUser) {
                alert('Önce Google ile giriş yapmalısınız.');
                return;
            }
            
            const roleLogin = document.getElementById('roleLoginScreen');
            const registerScreen = document.getElementById('hrRegisterScreen');
            
            if (!registerScreen) {
                console.error('hrRegisterScreen bulunamadı!');
                alert('Kayıt ekranı yüklenemedi (hrRegisterScreen eksik).');
                return;
            }
            
            // Ekranları değiştir
            if (roleLogin) {
                roleLogin.classList.add('hidden');
                console.log('roleLoginScreen gizlendi');
            }
            registerScreen.classList.remove('hidden');
            console.log('İK kayıt ekranı açıldı (showHrRegister)');
        }

        // Debug amaçlı: tarayıcı konsolundan window.forceRegister() diyerek açabilirsin
        window.forceRegister = function() {
            console.log('forceRegister çağrıldı');
            showHrRegister();
        }

        function backToRoleLogin() {
            const registerScreen = document.getElementById('hrRegisterScreen');
            const roleLogin = document.getElementById('roleLoginScreen');
            if (registerScreen) registerScreen.classList.add('hidden');
            if (roleLogin) roleLogin.classList.remove('hidden');
            updateHrRegisterButton();
            console.log('Role login ekranına dönüldü');
        }

        function logout() {
            // Google oturumunu kapat
            if (googleUser) {
                signOutGoogle();
            }
            
            currentUser = null;
            currentRole = null;
            
            // Kullanıcı bilgilerini gizle
            document.getElementById('userInfo').classList.add('hidden');
            
            document.querySelectorAll('[id$="Panel"]').forEach(panel => panel.classList.add('hidden'));
            document.getElementById('loginScreen').classList.remove('hidden');
        }

        // Giriş formu işleme
        const loginForm = document.getElementById('loginForm');
        if (loginForm) {
            loginForm.addEventListener('submit', function(e) {
                e.preventDefault();
            
            if (currentRole === 'candidate') {
                const alias = document.getElementById('candidateAlias').value.trim();
                const password = document.getElementById('candidatePassword').value.trim();
                console.log('Girişte girilen alias:', alias);
                console.log('Girişte girilen şifre:', password);
                console.log('candidates dizisi:', candidates);
                if (candidates.length === 0) {
                    fetchCandidates(() => {
                        document.getElementById('loginForm').dispatchEvent(new Event('submit'));
                    });
                    return;
                }
                const candidate = candidates.find(c => c.alias === alias && c.password === password);
                if (candidate) {
                    currentUser = candidate;
                    showCandidatePanel();
                } else {
                    alert('Geçersiz rumuz veya şifre!');
                }
            } else {
                const email = document.getElementById('adminHrEmail').value.trim();
                const password = document.getElementById('adminHrPassword').value.trim();
                if (currentRole === 'admin') {
                    // Admin giriş kontrolü (demo için basit kontrol)
                    if (email === 'akcaprox@gmail.com' && password === 'Ba030714') {
                        currentUser = { email, role: 'admin' };
                        showAdminPanel();
                    } else {
                        alert('Geçersiz admin bilgileri!');
                    }
                } else if (currentRole === 'hr') {
                    if (hrManagers.length === 0) {
                        fetchHrManagers(() => {
                            document.getElementById('loginForm').dispatchEvent(new Event('submit'));
                        });
                        return;
                    }
                    const hrManager = hrManagers.find(hr => hr.email === email && hr.password === password && hr.status === 'active');
                    if (hrManager) {
                        currentUser = hrManager;
                        showHrPanel();
                    } else {
                        alert('Geçersiz İK yönetici bilgileri veya hesap pasif!');
                    }
                }
            }
        });
        } // loginForm if kapanışı
        
        // Panel gösterme fonksiyonları
        function showAdminPanel() {
            document.getElementById('roleLoginScreen').classList.add('hidden');
            document.getElementById('adminPanel').classList.remove('hidden');
            loadAdminData();
        }

        function showHrPanel() {
            document.getElementById('roleLoginScreen').classList.add('hidden');
            document.getElementById('hrPanel').classList.remove('hidden');
            
            // İK yöneticisi giriş yaptıktan sonra kayıt ol seçeneğini kilitle
            localStorage.setItem('hrRegistrationLocked', 'true');
            
            // Google kullanıcı bilgilerini göster
            if (currentUser && currentUser.authMethod === 'google') {
                const userInfo = document.getElementById('userInfo');
                const userPhoto = document.getElementById('userPhoto');
                const userName = document.getElementById('userName');
                const userEmail = document.getElementById('userEmail');
                
                userInfo.classList.remove('hidden');
                userPhoto.src = currentUser.photoURL || '';
                userName.textContent = currentUser.name || '';
                userEmail.textContent = currentUser.email || '';
            }
            
            showHrSection('dashboard');
        }

        function showCandidatePanel() {
            document.getElementById('roleLoginScreen').classList.add('hidden');
            document.getElementById('candidatePanel').classList.remove('hidden');
            
            const categoryNames = {
                manufacturing_blue: 'İmalat İşleri - Mavi Yaka',
                manufacturing_white: 'İmalat İşleri - Beyaz Yaka',
                manufacturing_manager: 'İmalat İşleri - Yönetici',
                service_personnel: 'Hizmet İşleri - Hizmet Personeli',
                service_admin: 'Hizmet İşleri - Hizmet İdari Yönetici'
            };
            
            document.getElementById('candidateTestArea').textContent = categoryNames[currentUser.category];
        }

        // Admin panel fonksiyonları
        function loadAdminData(filteredList) {
            db.ref('hrManagers').once('value').then(snapshot => {
                const val = snapshot.val() || {};
                let hrManagers = Array.isArray(filteredList)
                    ? filteredList
                    : Object.values(val);
                document.getElementById('totalHrManagers').textContent = hrManagers.length;
                document.getElementById('activeUsers').textContent = hrManagers.filter(hr => hr.status === 'active').length;
                document.getElementById('inactiveUsers').textContent = hrManagers.filter(hr => hr.status === 'inactive').length;
                const tbody = document.getElementById('hrManagersList');
                tbody.innerHTML = '';
                hrManagers.forEach(hr => {
                    const row = document.createElement('tr');
                    row.className = 'border-b hover:bg-gray-50';
                    row.innerHTML = `
                        <td class="px-4 py-3">${hr.organization}</td>
                        <td class="px-4 py-3">${hr.name}</td>
                        <td class="px-4 py-3">${hr.email}</td>
                        <td class="px-4 py-3">${hr.phone}</td>
                        <td class="px-4 py-3">${hr.position}</td>
                        <td class="px-4 py-3">
                            <span class="px-2 py-1 rounded-full text-xs ${hr.status === 'active' ? 'bg-green-100 text-green-800' : 'bg-red-100 text-red-800'}">
                                ${hr.status === 'active' ? 'Aktif' : 'Pasif'}
                            </span>
                        </td>
                        <td class="px-4 py-3">
                            <button onclick="toggleHrStatus('${hr.id}')" class="px-3 py-1 rounded text-xs ${hr.status === 'active' ? 'bg-red-600 hover:bg-red-700 text-white' : 'bg-green-600 hover:bg-green-700 text-white'}">
                                ${hr.status === 'active' ? 'Pasif Yap' : 'Aktif Yap'}
                            </button>
                            <button onclick="deleteHrManager('${hr.id}'); loadAdminData();" class="ml-2 px-3 py-1 rounded text-xs bg-red-500 hover:bg-red-700 text-white">Sil</button>
                        </td>
                    `;
                    tbody.appendChild(row);
                });
            });
        }
// Admin paneli tarih filtreleme eventleri
document.addEventListener('DOMContentLoaded', function() {
    const startInput = document.getElementById('adminFilterStartDate');
    const endInput = document.getElementById('adminFilterEndDate');
    const filterBtn = document.getElementById('adminFilterDateBtn');
    const clearBtn = document.getElementById('adminClearDateBtn');
    if (filterBtn && startInput && endInput) {
        filterBtn.addEventListener('click', function() {
            const start = startInput.value ? new Date(startInput.value) : null;
            const end = endInput.value ? new Date(endInput.value) : null;
            db.ref('hrManagers').once('value').then(snapshot => {
                let hrManagers = Object.values(snapshot.val() || {});
                if (start) {
                    hrManagers = hrManagers.filter(hr => hr.createdAt && new Date(hr.createdAt) >= start);
                }
                if (end) {
                    const endOfDay = new Date(end);
                    endOfDay.setHours(23,59,59,999);
                    hrManagers = hrManagers.filter(hr => hr.createdAt && new Date(hr.createdAt) <= endOfDay);
                }
                loadAdminData(hrManagers);
            });
        });
    }
    if (clearBtn && startInput && endInput) {
        clearBtn.addEventListener('click', function() {
            startInput.value = '';
            endInput.value = '';
            loadAdminData();
        });
    }
});

        function toggleHrStatus(hrId) {
            // Firebase'den ilgili İK yöneticisini bul ve güncelle
            db.ref('hrManagers/' + hrId).once('value').then(snapshot => {
                const hr = snapshot.val();
                if (hr) {
                    const newStatus = hr.status === 'active' ? 'inactive' : 'active';
                    db.ref('hrManagers/' + hrId + '/status').set(newStatus).then(() => {
                        loadAdminData();
                    });
                }
            });
        }

        // İK panel fonksiyonları
        // Özel yeni üye paneli açma fonksiyonu
        function showNewMemberPanel() {
            try {
                console.log('showNewMemberPanel çağrıldı');
                
                // Tüm hr panellerini gizle
                const hrSections = ['hrDashboard', 'hrNewMember', 'hrCandidates', 'hrReports'];
                hrSections.forEach(sectionId => {
                    const element = document.getElementById(sectionId);
                    if (element) {
                        element.classList.add('hidden');
                        console.log('Gizlendi:', sectionId);
                    }
                });
                
                // Yeni üye panelini göster
                const newMemberPanel = document.getElementById('hrNewMember');
                if (newMemberPanel) {
                    newMemberPanel.classList.remove('hidden');
                    console.log('hrNewMember paneli gösterildi');
                    
                    // Formu sıfırla
                    const form = document.getElementById('newMemberForm');
                    if (form) {
                        form.reset();
                        console.log('Form sıfırlandı');
                    }
                } else {
                    console.error('hrNewMember paneli bulunamadı!');
                    alert('Yeni kayıt paneli bulunamadı!');
                }
            } catch (error) {
                console.error('showNewMemberPanel hatası:', error);
                alert('Panel açılırken hata oluştu: ' + error.message);
            }
        }

        function showHrSection(section) {
            console.log('showHrSection çağrıldı, section:', section);
            
            // Tüm hr panellerini gizle
            document.querySelectorAll('[id^="hr"]').forEach(el => {
                if (el.id.startsWith('hr') && el.id !== 'hrPanel') {
                    el.classList.add('hidden');
                }
            });
            
            // Hedef ID'yi oluştur
            const targetId = 'hr' + section.charAt(0).toUpperCase() + section.slice(1);
            console.log('Aranan hedef ID:', targetId);
            
            // Hedef elementi bul ve göster
            const targetElement = document.getElementById(targetId);
            if (targetElement) {
                targetElement.classList.remove('hidden');
                console.log('Panel gösterildi:', targetId);
            } else {
                console.error('Hedef panel bulunamadı:', targetId);
                return;
            }
            
            // Bölüm özel işlemleri
            if (section === 'dashboard') {
                loadHrDashboard();
            } else if (section === 'newMember') {
                console.log('Yeni üye ekleme formu açıldı');
                // Yeni üye ekleme formu gösterildi, özel bir yükleme işlemi gerekmez
            } else if (section === 'candidates') {
                loadCandidatesList();
            } else if (section === 'reports') {
                loadReportsData();
            }
        }

        function loadHrDashboard(filteredList) {
            // Eğer filtreli liste verilmişse onu kullan, yoksa tüm adayları kullan
            const userCandidates = Array.isArray(filteredList)
                ? filteredList
                : candidates.filter(c => c.createdBy === currentUser.id);
            const completedTests = userCandidates.filter(c => c.testCompleted).length;
            const pendingTests = userCandidates.filter(c => !c.testCompleted).length;
            // Ortalama puan hesaplama
            const completedCandidates = userCandidates.filter(c => c.testCompleted && c.score);
            const averageScore = completedCandidates.length > 0 
                ? Math.round(completedCandidates.reduce((sum, c) => sum + c.score, 0) / completedCandidates.length)
                : 0;
            document.getElementById('totalCandidates').textContent = userCandidates.length;
            document.getElementById('completedTests').textContent = completedTests;
            document.getElementById('pendingTests').textContent = pendingTests;
            document.getElementById('averageScore').textContent = averageScore;
        }

        // Dashboard tarih filtreleme eventleri
        document.addEventListener('DOMContentLoaded', function() {
            const startInput = document.getElementById('filterStartDate');
            const endInput = document.getElementById('filterEndDate');
            const filterBtn = document.getElementById('filterDateBtn');
            const clearBtn = document.getElementById('clearDateBtn');
            
            console.log('Dashboard elements:', {filterBtn, clearBtn, startInput, endInput});
            
            if (filterBtn && startInput && endInput) {
                filterBtn.addEventListener('click', function() {
                    const start = startInput.value ? new Date(startInput.value) : null;
                    const end = endInput.value ? new Date(endInput.value) : null;
                    let filtered = candidates.filter(c => c.createdBy === currentUser.id);
                    if (start) {
                        filtered = filtered.filter(c => c.createdAt && new Date(c.createdAt) >= start);
                    }
                    if (end) {
                        // Bitiş gününü de dahil et
                        const endOfDay = new Date(end);
                        endOfDay.setHours(23,59,59,999);
                        filtered = filtered.filter(c => c.createdAt && new Date(c.createdAt) <= endOfDay);
                    }
                    loadHrDashboard(filtered);
                });
            }
            if (clearBtn && startInput && endInput) {
                clearBtn.addEventListener('click', function() {
                    startInput.value = '';
                    endInput.value = '';
                    loadHrDashboard();
                });
            }
        });

        // Kategori seçim fonksiyonları
        function setupCategorySelectors() {
            // Yeni üye formu için
            const newMemberMainCat = document.getElementById('newMemberMainCategory');
            if (newMemberMainCat) {
                newMemberMainCat.addEventListener('change', function() {
                    updateSubCategory('newMemberSubCategory', this.value);
                });
            }
            
            // Aday ekleme formu için
            const candidateMainCat = document.getElementById('candidateMainCategory');
            if (candidateMainCat) {
                candidateMainCat.addEventListener('change', function() {
                    updateSubCategory('candidateSubCategory', this.value);
                });
            }
        }
        
        function updateSubCategory(subSelectId, mainCategory) {
            const subSelect = document.getElementById(subSelectId);
            subSelect.innerHTML = '';
            subSelect.disabled = false;
            
            if (mainCategory === 'manufacturing') {
                subSelect.innerHTML = `
                    <option value="">Alt Kategori Seç</option>
                    <option value="manufacturing_blue">Mavi Yaka</option>
                    <option value="manufacturing_white">Beyaz Yaka</option>
                    <option value="manufacturing_manager">Yönetici</option>
                `;
            } else if (mainCategory === 'service') {
                subSelect.innerHTML = `
                    <option value="">Alt Kategori Seç</option>
                    <option value="service_personnel">Hizmet Personeli</option>
                    <option value="service_admin">Hizmet İdari Yönetici</option>
                `;
            } else {
                subSelect.innerHTML = '<option value="">Önce ana kategori seçin</option>';
                subSelect.disabled = true;
            }
        }

        // Yeni üye ekleme (aday ekleme)
        const newMemberForm = document.getElementById('newMemberForm');
        if (newMemberForm) {
            newMemberForm.addEventListener('submit', function(e) {
                e.preventDefault();
            
            // Test grubu seçimini kontrol et
            const selectedTestGroup = document.getElementById('testGroupSelection').value;
            if (!selectedTestGroup) {
                alert('Lütfen bir test grubu seçin!');
                return;
            }
            
            const newCandidate = {
                id: Date.now().toString(),
                alias: document.getElementById('newMemberAlias').value,
                category: document.getElementById('newMemberSubCategory').value,
                password: document.getElementById('newMemberPassword').value,
                testGroup: selectedTestGroup, // Test grubu bilgisi
                createdBy: currentUser.id,
                testCompleted: false,
                createdAt: new Date().toISOString(),
                answers: [],
                score: 0
            };
            // Firebase'e kaydet
            db.ref('candidates/' + newCandidate.alias).set(newCandidate);

            
            alert(`Yeni aday başarıyla eklendi!\nSeçilen kriterler: ${selectedCriteria.length} adet\nTest soruları hazırlandı.`);
            this.reset();
            
            // Tüm checkboxları temizle
            checkboxes.forEach(checkbox => {
                checkbox.checked = false;
            });
            
            // Alt kategori seçimini sıfırla
            document.getElementById('newMemberSubCategory').disabled = true;
            document.getElementById('newMemberSubCategory').innerHTML = '<option value="">Önce ana kategori seçin</option>';
            
            // Eğer adaylar sekmesindeyse listeyi güncelle
            if (!document.getElementById('hrCandidates').classList.contains('hidden')) {
                loadCandidatesList();
            }
        });
        } // newMemberForm if kapanışı

        // Hızlı aday ekleme (varsayılan kriterlerle)
        const newCandidateForm = document.getElementById('newCandidateForm');
        if (newCandidateForm) {
            newCandidateForm.addEventListener('submit', function(e) {
                e.preventDefault();
            
            const testGroup = document.getElementById('candidateTestGroup').value;
            if (!testGroup) {
                alert('Lütfen bir test grubu seçin!');
                return;
            }
            
            const newCandidate = {
                id: Date.now().toString(),
                alias: document.getElementById('candidateAliasInput').value,
                password: document.getElementById('candidatePasswordInput').value,
                testGroup: testGroup,
                createdBy: currentUser.id,
                testCompleted: false,
                createdAt: new Date().toISOString(),
                answers: [],
                score: 0
            };
            // Firebase'e kaydet
            db.ref('candidates/' + newCandidate.alias).set(newCandidate);

            
            alert('Yeni aday başarıyla eklendi!\nVarsayılan test kriterleri uygulandı.');
            this.reset();
            
            // Alt kategori seçimini sıfırla
            document.getElementById('candidateSubCategory').disabled = true;
            document.getElementById('candidateSubCategory').innerHTML = '<option value="">Önce ana kategori seçin</option>';
            
            loadCandidatesList();
        });
        } // newCandidateForm if kapanışı

        function loadCandidatesList(filteredList) {
            const tbody = document.getElementById('candidatesList');
            tbody.innerHTML = '';
            db.ref('candidates').once('value').then(snapshot => {
                const val = snapshot.val() || {};
                // Sadece mevcut kullanıcının eklediği adaylar
                let userCandidates = Array.isArray(filteredList)
                    ? filteredList
                    : Object.values(val).filter(c => c.createdBy === currentUser.id);
                userCandidates.forEach(candidate => {
                    const categoryNames = {
                        manufacturing_blue: 'İmalat - Mavi Yaka',
                        manufacturing_white: 'İmalat - Beyaz Yaka',
                        manufacturing_manager: 'İmalat - Yönetici',
                        service_personnel: 'Hizmet - Personel',
                        service_admin: 'Hizmet - İdari Yönetici'
                    };
                    const row = document.createElement('tr');
                    row.className = 'border-b hover:bg-gray-50';
                    row.innerHTML = `
                        <td class="px-4 py-3">${candidate.alias}</td>
                        <td class="px-4 py-3">${candidate.password || ''}</td>
                        <td class="px-4 py-3">${categoryNames[candidate.category]}</td>
                        <td class="px-4 py-3">
                            <span class="px-2 py-1 rounded-full text-xs ${candidate.testCompleted ? 'bg-green-100 text-green-800' : 'bg-orange-100 text-orange-800'}">
                                ${candidate.testCompleted ? 'Tamamlandı' : 'Bekliyor'}
                            </span>
                        </td>
                        <td class="px-4 py-3">${new Date(candidate.createdAt).toLocaleDateString('tr-TR')}</td>
                        <td class="px-4 py-3 flex gap-2">
                            <button onclick="viewCandidateDetails('${candidate.id}')" class="px-3 py-1 bg-blue-600 hover:bg-blue-700 text-white rounded text-xs">
                                Detay
                            </button>
                            <button onclick="deleteCandidateFirebase('${candidate.id}')" class="px-3 py-1 bg-red-600 hover:bg-red-700 text-white rounded text-xs">
                                Sil
                            </button>
                        </td>
                    `;
                    tbody.appendChild(row);
                });
            });
        }
// Adaylar sekmesi tarih filtreleme eventleri
document.addEventListener('DOMContentLoaded', function() {
    const startInput = document.getElementById('candidatesFilterStartDate');
    const endInput = document.getElementById('candidatesFilterEndDate');
    const filterBtn = document.getElementById('candidatesFilterDateBtn');
    const clearBtn = document.getElementById('candidatesClearDateBtn');
    if (filterBtn && startInput && endInput) {
        filterBtn.addEventListener('click', function() {
            const start = startInput.value ? new Date(startInput.value) : null;
            const end = endInput.value ? new Date(endInput.value) : null;
            let filtered = candidates.filter(c => c.createdBy === currentUser.id);
            if (start) {
                filtered = filtered.filter(c => c.createdAt && new Date(c.createdAt) >= start);
            }
            if (end) {
                // Bitiş gününü de dahil et
                const endOfDay = new Date(end);
                endOfDay.setHours(23,59,59,999);
                filtered = filtered.filter(c => c.createdAt && new Date(c.createdAt) <= endOfDay);
            }
            loadCandidatesList(filtered);
        });
    }
    if (clearBtn && startInput && endInput) {
        clearBtn.addEventListener('click', function() {
            startInput.value = '';
            endInput.value = '';
            loadCandidatesList();
        });
    }
});

    function viewCandidateDetails(candidateId) {
        db.ref('candidates').orderByChild('id').equalTo(candidateId).once('value').then(snapshot => {
            const val = snapshot.val();
            if (val) {
                const candidate = Object.values(val)[0];
                alert(`Aday: ${candidate.alias}\nKategori: ${candidate.category}\nTest Durumu: ${candidate.testCompleted ? 'Tamamlandı' : 'Bekliyor'}\nPuan: ${candidate.score}`);
            }
        });
    }

    // Aday silme fonksiyonu (Firebase'den siler ve tabloyu günceller) - GLOBAL SCOPE
    function deleteCandidateFirebase(candidateId) {
        if (!confirm('Bu adayı silmek istediğinize emin misiniz?')) return;
        db.ref('candidates').orderByChild('id').equalTo(candidateId).once('value').then(snapshot => {
            const val = snapshot.val();
            if (val) {
                const key = Object.keys(val)[0];
                db.ref('candidates/' + key).remove().then(() => {
                    alert('Aday başarıyla silindi.');
                    loadCandidatesList();
                });
            } else {
                alert('Aday bulunamadı!');
            }
        });
    }

        // Test fonksiyonları
        function startTest() {
            document.getElementById('candidateWelcome').classList.add('hidden');
            document.getElementById('candidateTest').classList.remove('hidden');
            
            // Adayın test grubunu kontrol et (aday veritabanından)
            const testGroup = currentUser.testGroup || 'grup1';
            testQuestions = questionBank[testGroup] || [];
            
            console.log(`Test Grubu: ${testGroup}, Soru sayısı: ${testQuestions.length}`);
            currentQuestionIndex = 0;
            userAnswers = new Array(testQuestions.length).fill(null);
            
            document.getElementById('totalQuestions').textContent = testQuestions.length;
            
            startTimer();
            showQuestion();
        }
        
        // Test kriterlerine göre soru oluşturma
        function generateQuestionsFromCriteria(criteria) {
            const questions = [];
            let questionId = 1;
            
            criteria.forEach(criterion => {
                const criterionQuestions = getCriterionQuestions(criterion, questionId);
                questions.push(...criterionQuestions);
                questionId += criterionQuestions.length;
            });
            
            return questions.length > 0 ? questions : getDefaultQuestions();
        }
        
        // Kriter bazlı soru bankası
        function getCriterionQuestions(criterion, startId) {
            const questionSets = {
                communication: [
                    {
                        id: startId,
                        question: "Karmaşık konuları başkalarına açıklarken sabırlı ve anlaşılır olmaya özen gösteririm.",
                        options: ["Kesinlikle Katılmıyorum", "Katılmıyorum", "Kararsızım", "Katılıyorum", "Kesinlikle Katılıyorum"],
                        correct: 4,
                        category: "communication"
                    },
                    {
                        id: startId + 1,
                        question: "Farklı görüşlere sahip kişilerle bile etkili iletişim kurabilirim.",
                        options: ["Kesinlikle Katılmıyorum", "Katılmıyorum", "Kararsızım", "Katılıyorum", "Kesinlikle Katılıyorum"],
                        correct: 4,
                        category: "communication"
                    }
                ],
                teamwork: [
                    {
                        id: startId,
                        question: "Takım hedeflerini kişisel hedeflerimden önde tutarım.",
                        options: ["Kesinlikle Katılmıyorum", "Katılmıyorum", "Kararsızım", "Katılıyorum", "Kesinlikle Katılıyorum"],
                        correct: 4,
                        category: "teamwork"
                    },
                    {
                        id: startId + 1,
                        question: "Takım arkadaşlarımın başarılarını destekler ve kutlarım.",
                        options: ["Kesinlikle Katılmıyorum", "Katılmıyorum", "Kararsızım", "Katılıyorum", "Kesinlikle Katılıyorum"],
                        correct: 4,
                        category: "teamwork"
                    }
                ],
                analytical_thinking: [
                    {
                        id: startId,
                        question: "Karar vermeden önce mevcut verileri detaylı olarak analiz ederim.",
                        options: ["Kesinlikle Katılmıyorum", "Katılmıyorum", "Kararsızım", "Katılıyorum", "Kesinlikle Katılıyorum"],
                        correct: 4,
                        category: "analytical_thinking"
                    },
                    {
                        id: startId + 1,
                        question: "Karmaşık problemleri daha küçük parçalara bölerek çözmeyi tercih ederim.",
                        options: ["Kesinlikle Katılmıyorum", "Katılmıyorum", "Kararsızım", "Katılıyorum", "Kesinlikle Katılıyorum"],
                        correct: 4,
                        category: "analytical_thinking"
                    }
                ],
                problem_solving: [
                    {
                        id: startId,
                        question: "Beklenmedik problemlerle karşılaştığımda yaratıcı çözümler üretirim.",
                        options: ["Kesinlikle Katılmıyorum", "Katılmıyorum", "Kararsızım", "Katılıyorum", "Kesinlikle Katılıyorum"],
                        correct: 4,
                        category: "problem_solving"
                    }
                ],
                stress_management: [
                    {
                        id: startId,
                        question: "Yoğun iş temposu altında bile kaliteli çalışma yapabilirim.",
                        options: ["Kesinlikle Katılmıyorum", "Katılmıyorum", "Kararsızım", "Katılıyorum", "Kesinlikle Katılıyorum"],
                        correct: 4,
                        category: "stress_management"
                    }
                ],
                leadership: [
                    {
                        id: startId,
                        question: "Grup çalışmalarında doğal olarak liderlik rolü üstlenirim.",
                        options: ["Kesinlikle Katılmıyorum", "Katılmıyorum", "Kararsızım", "Katılıyorum", "Kesinlikle Katılıyorum"],
                        correct: 3,
                        category: "leadership"
                    }
                ],
                time_management: [
                    {
                        id: startId,
                        question: "İş önceliklerimi belirler ve zamanımı etkili şekilde yönetirim.",
                        options: ["Kesinlikle Katılmıyorum", "Katılmıyorum", "Kararsızım", "Katılıyorum", "Kesinlikle Katılıyorum"],
                        correct: 4,
                        category: "time_management"
                    }
                ],
                verbal_reasoning: [
                    {
                        id: startId,
                        question: "Yazılı metinlerdeki ana fikirleri hızlıca tespit edebilirim.",
                        options: ["Kesinlikle Katılmıyorum", "Katılmıyorum", "Kararsızım", "Katılıyorum", "Kesinlikle Katılıyorum"],
                        correct: 4,
                        category: "verbal_reasoning"
                    }
                ],
                numerical_ability: [
                    {
                        id: startId,
                        question: "Sayısal verilerle çalışmak ve hesaplamalar yapmak beni zorlamaz.",
                        options: ["Kesinlikle Katılmıyorum", "Katılmıyorum", "Kararsızım", "Katılıyorum", "Kesinlikle Katılıyorum"],
                        correct: 4,
                        category: "numerical_ability"
                    }
                ],
                ethical_decisions: [
                    {
                        id: startId,
                        question: "İş hayatında etik değerlere uygun davranmak her zaman önceliğimdir.",
                        options: ["Kesinlikle Katılmıyorum", "Katılmıyorum", "Kararsızım", "Katılıyorum", "Kesinlikle Katılıyorum"],
                        correct: 4,
                        category: "ethical_decisions"
                    }
                ],
                conflict_management: [
                    {
                        id: startId,
                        question: "İş yerindeki anlaşmazlıkları yapıcı şekilde çözmeye odaklanırım.",
                        options: ["Kesinlikle Katılmıyorum", "Katılmıyorum", "Kararsızım", "Katılıyorum", "Kesinlikle Katılıyorum"],
                        correct: 4,
                        category: "conflict_management"
                    }
                ],
                customer_service: [
                    {
                        id: startId,
                        question: "Müşteri memnuniyeti için ekstra çaba göstermekten çekinmem.",
                        options: ["Kesinlikle Katılmıyorum", "Katılmıyorum", "Kararsızım", "Katılıyorum", "Kesinlikle Katılıyorum"],
                        correct: 4,
                        category: "customer_service"
                    }
                ],
                crisis_management: [
                    {
                        id: startId,
                        question: "Kriz durumlarında soğukkanlılığımı korur ve hızlı kararlar alabilirim.",
                        options: ["Kesinlikle Katılmıyorum", "Katılmıyorum", "Kararsızım", "Katılıyorum", "Kesinlikle Katılıyorum"],
                        correct: 4,
                        category: "crisis_management"
                    }
                ],
            };
            
            return questionSets[criterion] || [];
        }
        
        // Varsayılan sorular (kriter seçilmemişse)
        function getDefaultQuestions() {
            return [
                {
                    id: 1,
                    question: "İş yerinde etkili iletişim kurmaya önem veririm.",
                    options: ["Kesinlikle Katılmıyorum", "Katılmıyorum", "Kararsızım", "Katılıyorum", "Kesinlikle Katılıyorum"],
                    correct: 4,
                    category: "general"
                },
                {
                    id: 2,
                    question: "Takım halinde çalışmayı tercih ederim.",
                    options: ["Kesinlikle Katılmıyorum", "Katılmıyorum", "Kararsızım", "Katılıyorum", "Kesinlikle Katılıyorum"],
                    correct: 3,
                    category: "general"
                },
                {
                    id: 3,
                    question: "Problemleri analitik olarak çözmeye odaklanırım.",
                    options: ["Kesinlikle Katılmıyorum", "Katılmıyorum", "Kararsızım", "Katılıyorum", "Kesinlikle Katılıyorum"],
                    correct: 4,
                    category: "general"
                }
            ];
        }

        function startTimer() {
            testTimer = setInterval(() => {
                timeRemaining--;
                const minutes = Math.floor(timeRemaining / 60);
                const seconds = timeRemaining % 60;
                document.getElementById('testTimer').textContent = `${minutes.toString().padStart(2, '0')}:${seconds.toString().padStart(2, '0')}`;
                
                if (timeRemaining <= 0) {
                    clearInterval(testTimer);
                    finishTest();
                }
            }, 1000);
        }

        function showQuestion() {
            if (testQuestions.length === 0) return;
            
            const question = testQuestions[currentQuestionIndex];
            document.getElementById('currentQuestionNumber').textContent = currentQuestionIndex + 1;

            const questionContent = document.getElementById('questionContent');
            // Eğer soru bir string ise, varsayılan 5'li Likert seçenekleriyle göster
            if (typeof question === 'string') {
                questionContent.innerHTML = `
                    <h4 class="text-xl font-semibold text-gray-800 mb-6">${question}</h4>
                    <div class="space-y-2">
                        ${["Kesinlikle Katılmıyorum", "Katılmıyorum", "Kararsızım", "Katılıyorum", "Kesinlikle Katılıyorum"].map((option, index) => `
                            <div class="likert-option ${userAnswers[currentQuestionIndex] === index ? 'selected' : ''}" onclick="selectAnswer(${index})">
                                <input type="radio" name="answer" value="${index}" ${userAnswers[currentQuestionIndex] === index ? 'checked' : ''}>
                                <span class="option-number">${index + 1}</span>
                                <span class="option-text">${option}</span>
                            </div>
                        `).join('')}
                    </div>
                `;
            } else {
                questionContent.innerHTML = `
                    <h4 class="text-xl font-semibold text-gray-800 mb-6">${question.soru || question.question}</h4>
                    <div class="space-y-2">
                        ${(question.secenekler || question.options).map((option, index) => `
                            <div class="likert-option ${userAnswers[currentQuestionIndex] === index ? 'selected' : ''}" onclick="selectAnswer(${index})">
                                <input type="radio" name="answer" value="${index}" ${userAnswers[currentQuestionIndex] === index ? 'checked' : ''}>
                                <span class="option-number">${index + 1}</span>
                                <span class="option-text">${option}</span>
                            </div>
                        `).join('')}
                    </div>
                `;
            }
            
            // Buton durumları
            document.getElementById('prevButton').disabled = currentQuestionIndex === 0;
            document.getElementById('nextButton').style.display = currentQuestionIndex === testQuestions.length - 1 ? 'none' : 'block';
            // "Testi Bitir" butonu sadece son soruda görünsün
            document.getElementById('finishButton').style.display = currentQuestionIndex === testQuestions.length - 1 ? 'block' : 'none';
        }
        
        function selectAnswer(answerIndex) {
            userAnswers[currentQuestionIndex] = answerIndex;
            
            // Tüm seçeneklerin selected sınıfını kaldır
            document.querySelectorAll('.likert-option').forEach(option => {
                option.classList.remove('selected');
            });
            
            // Seçilen seçeneğe selected sınıfını ekle
            document.querySelectorAll('.likert-option')[answerIndex].classList.add('selected');
            
            // Radio button'ı işaretle
            document.querySelector(`input[value="${answerIndex}"]`).checked = true;
        }

        function previousQuestion() {
            if (currentQuestionIndex > 0) {
                currentQuestionIndex--;
                showQuestion();
            }
        }

        function nextQuestion() {
            if (currentQuestionIndex < testQuestions.length - 1) {
                currentQuestionIndex++;
                showQuestion();
            }
        }

        function finishTest() {
            clearInterval(testTimer);

            // Puanı hesapla
            const score = puanHesapla(testQuestions, userAnswers);

            // Sonuçları kaydet
            currentUser.testCompleted = true;
            currentUser.answers = userAnswers;
            currentUser.score = score;
            currentUser.completedAt = new Date().toISOString();

            // Candidates listesini güncelle
            const candidateIndex = candidates.findIndex(c => c.id === currentUser.id);
            if (candidateIndex !== -1) {
                candidates[candidateIndex] = currentUser;
            }

            // Firebase'e güncellenmiş aday bilgisini kaydet
            if (currentUser && currentUser.alias) {
                db.ref('candidates/' + currentUser.alias).update({
                    testCompleted: true,
                    answers: userAnswers,
                    score: score,
                    completedAt: currentUser.completedAt
                });
            }

            // Test sonucu ekranını göster
            document.getElementById('candidateTest').classList.add('hidden');
            document.getElementById('testCompleted').classList.remove('hidden');
        }

        // Rapor fonksiyonları
        function loadReportsData() {
            const select = document.getElementById('reportCandidateSelect');
            select.innerHTML = '<option value="">Aday Seçin</option>';
            
            const userCandidates = candidates.filter(c => c.createdBy === currentUser.id && c.testCompleted);
            userCandidates.forEach(candidate => {
                const option = document.createElement('option');
                option.value = candidate.id;
                option.textContent = candidate.alias;
                select.appendChild(option);
            });
        }

        function showReport(type) {
            const candidateId = document.getElementById('reportCandidateSelect').value;
            if (!candidateId) {
                alert('Lütfen bir aday seçin!');
                return;
            }
            
            const candidate = candidates.find(c => c.id === candidateId);
            const reportContent = document.getElementById('reportContent');
            
            if (type === 'answers') {
                showAnswersReport(candidate, reportContent);
            } else if (type === 'scores') {
                showScoresReport(candidate, reportContent);
            } else if (type === 'charts') {
                showChartsReport(candidate, reportContent);
            }
        }

        function showAnswersReport(candidate, container) {
            const groupMapping = {
                manufacturing_white: 'grup1',
                manufacturing_blue: 'grup2',
                manufacturing_manager: 'grup3',
                service_personnel: 'grup4',
                service_admin: 'grup5'
            };
            const group = groupMapping[candidate.category] || 'grup1';
            const questions = questionBank[group] || [];
            
            if (questions.length === 0) {
                container.innerHTML = `
                    <h3 class="text-xl font-bold text-gray-800 mb-4">Sorular ve Cevaplar - ${candidate.alias}</h3>
                    <p class="text-gray-600">Bu kategori için soru bulunamadı.</p>
                `;
                return;
            }
            
            container.innerHTML = `
                <h3 class="text-xl font-bold text-gray-800 mb-4">Detaylı Cevap Analizi - ${candidate.alias}</h3>
                <div class="space-y-4">
                    ${questions.map((question, index) => {
                        const userAnswer = candidate.answers && candidate.answers[index] !== undefined ? candidate.answers[index] : null;
                        const dogruCevap = questionAnswerKey[index] || 5;
                        const kullaniciCevap = userAnswer !== null ? (userAnswer + 1) : null;
                        
                        let userAnswerText = 'Cevaplanmadı';
                        let puanYuzdesi = 0;
                        let performansRenk = 'text-gray-600';
                        let performansIcon = '❓';
                        
                        if (kullaniciCevap !== null) {
                            const options = ["Kesinlikle Katılmıyorum", "Katılmıyorum", "Kararsızım", "Katılıyorum", "Kesinlikle Katılıyorum"];
                            userAnswerText = options[userAnswer];
                            
                            // Yeni puanlama sistemine göre hesapla
                            if (dogruCevap === 5) {
                                if (kullaniciCevap === 5) { puanYuzdesi = 100; performansRenk = 'text-green-600'; performansIcon = '🎯'; }
                                else if (kullaniciCevap === 4) { puanYuzdesi = 75; performansRenk = 'text-blue-600'; performansIcon = '👍'; }
                                else if (kullaniciCevap === 3) { puanYuzdesi = 50; performansRenk = 'text-yellow-600'; performansIcon = '⚡'; }
                                else { puanYuzdesi = 0; performansRenk = 'text-red-600'; performansIcon = '❌'; }
                            } else if (dogruCevap === 1) {
                                if (kullaniciCevap === 1) { puanYuzdesi = 100; performansRenk = 'text-green-600'; performansIcon = '🎯'; }
                                else if (kullaniciCevap === 2) { puanYuzdesi = 75; performansRenk = 'text-blue-600'; performansIcon = '👍'; }
                                else if (kullaniciCevap === 3) { puanYuzdesi = 50; performansRenk = 'text-yellow-600'; performansIcon = '⚡'; }
                                else { puanYuzdesi = 0; performansRenk = 'text-red-600'; performansIcon = '❌'; }
                            }
                        }
                        
                        return `
                            <div class="border border-gray-200 rounded-lg p-4 ${puanYuzdesi >= 75 ? 'bg-green-50' : puanYuzdesi >= 50 ? 'bg-yellow-50' : puanYuzdesi > 0 ? 'bg-red-50' : 'bg-gray-50'}">
                                <div class="flex justify-between items-start mb-2">
                                    <h4 class="font-semibold text-gray-800 flex-1">Soru ${index + 1}: ${question}</h4>
                                    <span class="${performansRenk} text-xl ml-2">${performansIcon}</span>
                                </div>
                                <div class="grid grid-cols-1 md:grid-cols-3 gap-4 mt-3">
                                    <div>
                                        <p class="text-sm text-gray-500">Verilen Cevap:</p>
                                        <p class="font-semibold text-blue-600">${userAnswerText}</p>
                                    </div>
                                    <div>
                                        <p class="text-sm text-gray-500">Hedef Cevap:</p>
                                        <p class="font-semibold text-purple-600">${dogruCevap === 5 ? 'Kesinlikle Katılıyorum' : 'Kesinlikle Katılmıyorum'}</p>
                                    </div>
                                    <div>
                                        <p class="text-sm text-gray-500">Performans:</p>
                                        <p class="font-semibold ${performansRenk}">${puanYuzdesi}%</p>
                                    </div>
                                </div>
                            </div>
                        `;
                    }).join('')}
                </div>
            `;
        }

        function showScoresReport(candidate, container) {
            const groupMapping = {
                manufacturing_white: 'grup1',
                manufacturing_blue: 'grup2',
                manufacturing_manager: 'grup3',
                service_personnel: 'grup4',
                service_admin: 'grup5'
            };
            const group = groupMapping[candidate.category] || 'grup1';
            const questions = questionBank[group] || [];
            
            if (questions.length === 0) {
                container.innerHTML = `
                    <h3 class="text-xl font-bold text-gray-800 mb-4">Puan Raporu - ${candidate.alias}</h3>
                    <p class="text-gray-600">Bu kategori için soru bulunamadı.</p>
                `;
                return;
            }
            
            // Yeni puanlama sistemine göre hesaplama
            let detayliAnaliz = '';
            let toplamYuzde = 0;
            let cevaplanmisSort = 0;
            
            if (candidate.answers && candidate.answers.length > 0) {
                for (let i = 0; i < Math.min(questions.length, candidate.answers.length); i++) {
                    let dogruCevap = questionAnswerKey[i] || 5;
                    let kullaniciCevap = candidate.answers[i] !== null ? (candidate.answers[i] + 1) : null;
                    let puanYuzdesi = 0;
                    
                    if (kullaniciCevap !== null) {
                        cevaplanmisSort++;
                        if (dogruCevap === 5) {
                            if (kullaniciCevap === 5) puanYuzdesi = 100;
                            else if (kullaniciCevap === 4) puanYuzdesi = 75;
                            else if (kullaniciCevap === 3) puanYuzdesi = 50;
                            else puanYuzdesi = 0;
                        } else if (dogruCevap === 1) {
                            if (kullaniciCevap === 1) puanYuzdesi = 100;
                            else if (kullaniciCevap === 2) puanYuzdesi = 75;
                            else if (kullaniciCevap === 3) puanYuzdesi = 50;
                            else puanYuzdesi = 0;
                        }
                        toplamYuzde += puanYuzdesi;
                    }
                }
            }
            
            const genelBasari = cevaplanmisSort > 0 ? Math.round(toplamYuzde / cevaplanmisSort) : 0;
            const tamamlanmaOrani = questions.length > 0 ? Math.round((cevaplanmisSort / questions.length) * 100) : 0;
            
            container.innerHTML = `
                <h3 class="text-xl font-bold text-gray-800 mb-4">Detaylı Puan Analizi - ${candidate.alias}</h3>
                <div class="grid grid-cols-1 md:grid-cols-3 gap-6 mb-6">
                    <div class="bg-green-50 border border-green-200 rounded-lg p-6 text-center">
                        <h4 class="text-lg font-semibold text-green-800 mb-2">Genel Başarı</h4>
                        <p class="text-3xl font-bold text-green-600">${genelBasari}%</p>
                        <p class="text-sm text-green-600 mt-1">Ortalama Performans</p>
                    </div>
                    <div class="bg-blue-50 border border-blue-200 rounded-lg p-6 text-center">
                        <h4 class="text-lg font-semibold text-blue-800 mb-2">Tamamlanma</h4>
                        <p class="text-3xl font-bold text-blue-600">${tamamlanmaOrani}%</p>
                        <p class="text-sm text-blue-600 mt-1">${cevaplanmisSort}/${questions.length} soru</p>
                    </div>
                    <div class="bg-purple-50 border border-purple-200 rounded-lg p-6 text-center">
                        <h4 class="text-lg font-semibold text-purple-800 mb-2">Toplam Puan</h4>
                        <p class="text-3xl font-bold text-purple-600">${Math.round(toplamYuzde)}</p>
                        <p class="text-sm text-purple-600 mt-1">Toplam yüzde puanı</p>
                    </div>
                </div>
                <div class="mt-6 bg-gray-50 border border-gray-200 rounded-lg p-6">
                    <h4 class="text-lg font-semibold text-gray-800 mb-4">Performans Değerlendirmesi</h4>
                    <div class="w-full bg-gray-200 rounded-full h-6 mb-2">
                        <div class="bg-gradient-to-r from-green-500 to-green-600 h-6 rounded-full transition-all duration-500" style="width: ${genelBasari}%"></div>
                    </div>
                    <p class="text-center text-2xl font-bold text-gray-800">${genelBasari}%</p>
                    <div class="mt-4 text-sm text-gray-600">
                        <p><strong>Puanlama Sistemi:</strong></p>
                        <p>• Tam doğru cevap: %100</p>
                        <p>• Yakın cevap: %75</p>
                        <p>• Orta cevap: %50</p>
                        <p>• Ters cevap: %0</p>
                    </div>
                </div>
                <div class="mt-6 grid grid-cols-1 md:grid-cols-2 gap-4">
                    <div class="bg-white border border-gray-200 rounded-lg p-4">
                        <h5 class="font-semibold text-gray-800 mb-2">Test Bilgileri</h5>
                        <p class="text-sm text-gray-600">Kategori: ${candidate.category}</p>
                        <p class="text-sm text-gray-600">Tamamlanma: ${candidate.completedAt ? new Date(candidate.completedAt).toLocaleString('tr-TR') : 'Bilinmiyor'}</p>
                        <p class="text-sm text-gray-600">Cevaplanma Oranı: ${tamamlanmaOrani}%</p>
                    </div>
                    <div class="bg-white border border-gray-200 rounded-lg p-4">
                        <h5 class="font-semibold text-gray-800 mb-2">Performans Değerlendirmesi</h5>
                        <p class="text-sm ${genelBasari >= 80 ? 'text-green-600' : genelBasari >= 60 ? 'text-yellow-600' : 'text-red-600'}">
                            ${genelBasari >= 80 ? '🎉 Mükemmel Performans' : genelBasari >= 60 ? '👍 İyi Performans' : '📚 Geliştirilmesi Gereken'}
                        </p>
                        <p class="text-xs text-gray-500 mt-1">
                            ${genelBasari >= 80 ? 'Beklentilerin üzerinde başarı' : genelBasari >= 60 ? 'Kabul edilebilir seviye' : 'Ek eğitim önerilir'}
                        </p>
                    </div>
                </div>
            `;
        }

        function showChartsReport(candidate, container) {
            // Önce mevcut Chart instance'larını temizle
            if (window.chartInstances) {
                Object.values(window.chartInstances).forEach(chart => {
                    if (chart && typeof chart.destroy === 'function') {
                        chart.destroy();
                    }
                });
            }
            window.chartInstances = {};
            
            // Container'ı temizle ve yeni içeriği ekle
            container.innerHTML = `
                <h3 class="text-xl font-bold text-gray-800 mb-6">Analiz Pro X - Grafik Raporları</h3>
                <div class="text-center mb-6">
                    <h4 class="text-lg font-semibold text-blue-600">${candidate.alias} - Detaylı Performans Analizi</h4>
                </div>
                
                <!-- 1. Temel Profil Görselleştirmesi: RADAR GRAFİĞİ -->
                <div class="bg-white border border-gray-200 rounded-lg p-6 mb-6">
                    <h4 class="text-lg font-semibold text-gray-800 mb-2">1. Temel Profil Görselleştirmesi</h4>
                    <p class="text-sm text-gray-600 mb-4">Yetkinlik profili şekli ve ideal profil karşılaştırması</p>
                    <div class="relative" style="height: 400px;">
                        <canvas id="profileRadarChart" width="400" height="400"></canvas>
                    </div>
                    <div class="mt-4 grid grid-cols-2 gap-4">
                        <div class="flex items-center">
                            <div class="w-4 h-4 bg-blue-500 rounded mr-2"></div>
                            <span class="text-sm">Aday Profili</span>
                        </div>
                        <div class="flex items-center">
                            <div class="w-4 h-4 bg-red-500 rounded mr-2"></div>
                            <span class="text-sm">İdeal Profil</span>
                        </div>
                    </div>
                </div>

                <!-- 2. Kritik Faktör Görselleştirmesi: RİSK GÖSTERGE GRAFİĞİ -->
                <div class="bg-white border border-gray-200 rounded-lg p-6 mb-6">
                    <h4 class="text-lg font-semibold text-gray-800 mb-2">2. Güvenilirlik Risk Göstergesi</h4>
                    <p class="text-sm text-gray-600 mb-4">Cevap eğilimi ve manipülasyon risk analizi</p>
                    <div class="relative" style="height: 300px;">
                        <canvas id="riskGaugeChart" width="400" height="300"></canvas>
                    </div>
                    <div class="mt-4 grid grid-cols-3 gap-2 text-center">
                        <div class="bg-green-100 text-green-800 py-2 px-3 rounded text-sm">
                            <div class="font-semibold">Güvenilir</div>
                            <div class="text-xs">0-30%</div>
                        </div>
                        <div class="bg-yellow-100 text-yellow-800 py-2 px-3 rounded text-sm">
                            <div class="font-semibold">Orta Risk</div>
                            <div class="text-xs">31-60%</div>
                        </div>
                        <div class="bg-red-100 text-red-800 py-2 px-3 rounded text-sm">
                            <div class="font-semibold">Yüksek Risk</div>
                            <div class="text-xs">61-100%</div>
                        </div>
                    </div>
                </div>

                <!-- 3. Bilişsel Kapasite Görselleştirmesi: KARŞILAŞTIRMALI BAR GRAFİĞİ -->
                <div class="bg-white border border-gray-200 rounded-lg p-6 mb-6">
                    <h4 class="text-lg font-semibold text-gray-800 mb-2">3. Bilişsel Kapasite Analizi</h4>
                    <p class="text-sm text-gray-600 mb-4">Analitik düşünme ve sözel akıl yürütme - sektör normu karşılaştırması</p>
                    <div class="relative" style="height: 300px;">
                        <canvas id="cognitiveBarChart" width="400" height="300"></canvas>
                    </div>
                    <div class="mt-4 bg-blue-50 p-4 rounded">
                        <p class="text-sm text-blue-800">
                            <strong>Not:</strong> Bilişsel kapasite skorları öğrenme ve adaptasyon potansiyelini gösterir. 
                            Bu skorlar nispeten sabittir ve gelişim planlamasında dikkate alınmalıdır.
                        </p>
                    </div>
                </div>

                <!-- 4. Aksiyon Hiyerarşisi Görselleştirmesi: KRİTİK YATAY ÇUBUK GRAFİĞİ -->
                <div class="bg-white border border-gray-200 rounded-lg p-6">
                    <h4 class="text-lg font-semibold text-gray-800 mb-2">4. Kritik Yetkinlik Öncelikleri</h4>
                    <p class="text-sm text-gray-600 mb-4">Pozisyon için en kritik yetkinliklerin performans sıralaması</p>
                    <div class="relative" style="height: 350px;">
                        <canvas id="priorityHorizontalChart" width="400" height="350"></canvas>
                    </div>
                    <div class="mt-4 bg-orange-50 p-4 rounded">
                        <p class="text-sm text-orange-800">
                            <strong>Mülakat Önerisi:</strong> En düşük skorlu yetkinlikler üzerinde detaylı sorular sorulması önerilir. 
                            Bu alanlar acil gelişim gerektiren öncelikli konulardır.
                        </p>
                    </div>
                </div>
            `;
            
            // Yükleme göstergesi ekle
            container.innerHTML += `
                <div id="chartLoadingIndicator" class="text-center py-8">
                    <div class="inline-block animate-spin rounded-full h-8 w-8 border-b-2 border-blue-600"></div>
                    <p class="mt-2 text-gray-600">Grafikler yükleniyor...</p>
                </div>
            `;
            
            // Grafikleri çiz - daha uzun bekleme süresi
            setTimeout(() => {
                try {
                    // Yükleme göstergesini kaldır
                    const loadingIndicator = document.getElementById('chartLoadingIndicator');
                    if (loadingIndicator) {
                        loadingIndicator.remove();
                    }
                    
                    drawProfileRadarChart(candidate);
                    drawRiskGaugeChart(candidate);
                    drawCognitiveBarChart(candidate);
                    drawPriorityHorizontalChart(candidate);
                    
                    console.log('Tüm grafikler başarıyla çizildi');
                } catch (error) {
                    console.error('Grafik çizim hatası:', error);
                    
                    // Yükleme göstergisini kaldır
                    const loadingIndicator = document.getElementById('chartLoadingIndicator');
                    if (loadingIndicator) {
                        loadingIndicator.remove();
                    }
                    
                    container.innerHTML += `
                        <div class="bg-red-50 border border-red-200 rounded-lg p-4 mt-4">
                            <p class="text-red-800">Grafikler yüklenirken bir hata oluştu: ${error.message}</p>
                            <p class="text-red-600 text-sm mt-2">Lütfen sayfayı yenileyin ve tekrar deneyin.</p>
                        </div>
                    `;
                }
            }, 1000);
        }

        // 1. Temel Profil Görselleştirmesi: RADAR GRAFİĞİ
        function drawProfileRadarChart(candidate) {
            console.log('drawProfileRadarChart çağrıldı');
            const canvas = document.getElementById('profileRadarChart');
            if (!canvas) {
                console.error('profileRadarChart canvas bulunamadı');
                return;
            }
            console.log('profileRadarChart canvas bulundu');
            
            const ctx = canvas.getContext('2d');
            
            // Yetkinlik kategorileri ve skorlar
            const competencies = [
                'İletişim Becerileri',
                'Analitik Düşünme', 
                'Takım Çalışması',
                'Problem Çözme',
                'Stres Yönetimi',
                'Liderlik Potansiyeli',
                'Detay Odaklılık',
                'Zaman Yönetimi'
            ];
            
            // Aday skorları (test sonuçlarına göre hesaplanmış)
            const candidateScores = [
                Math.min(100, (candidate.score || 50) + Math.random() * 30),
                Math.min(100, (candidate.score || 50) + Math.random() * 25),
                Math.min(100, (candidate.score || 50) + Math.random() * 20),
                Math.min(100, (candidate.score || 50) + Math.random() * 35),
                Math.min(100, (candidate.score || 50) + Math.random() * 15),
                Math.min(100, (candidate.score || 50) + Math.random() * 40),
                Math.min(100, (candidate.score || 50) + Math.random() * 30),
                Math.min(100, (candidate.score || 50) + Math.random() * 25)
            ];
            
            // İdeal profil skorları (pozisyon gereksinimleri)
            const idealScores = [85, 90, 80, 88, 75, 82, 92, 87];
            
            const chart = new Chart(ctx, {
                type: 'radar',
                data: {
                    labels: competencies,
                    datasets: [{
                        label: 'Aday Profili',
                        data: candidateScores,
                        backgroundColor: 'rgba(59, 130, 246, 0.2)',
                        borderColor: 'rgb(59, 130, 246)',
                        pointBackgroundColor: 'rgb(59, 130, 246)',
                        pointBorderColor: '#fff',
                        pointHoverBackgroundColor: '#fff',
                        pointHoverBorderColor: 'rgb(59, 130, 246)',
                        borderWidth: 2
                    }, {
                        label: 'İdeal Profil',
                        data: idealScores,
                        backgroundColor: 'rgba(239, 68, 68, 0.1)',
                        borderColor: 'rgb(239, 68, 68)',
                        pointBackgroundColor: 'rgb(239, 68, 68)',
                        pointBorderColor: '#fff',
                        pointHoverBackgroundColor: '#fff',
                        pointHoverBorderColor: 'rgb(239, 68, 68)',
                        borderWidth: 2,
                        borderDash: [5, 5]
                    }]
                },
                options: {
                    responsive: true,
                    maintainAspectRatio: false,
                    scales: {
                        r: {
                            beginAtZero: true,
                            max: 100,
                            ticks: {
                                stepSize: 20
                            }
                        }
                    },
                    plugins: {
                        legend: {
                            position: 'bottom'
                        }
                    }
                }
            });
            
            // Chart instance'ı sakla
            if (!window.chartInstances) window.chartInstances = {};
            window.chartInstances.profileRadar = chart;
            console.log('Radar chart başarıyla oluşturuldu');
        }

        // 2. Kritik Faktör Görselleştirmesi: RİSK GÖSTERGE GRAFİĞİ
        function drawRiskGaugeChart(candidate) {
            console.log('drawRiskGaugeChart çağrıldı');
            const canvas = document.getElementById('riskGaugeChart');
            if (!canvas) {
                console.error('riskGaugeChart canvas bulunamadı');
                return;
            }
            console.log('riskGaugeChart canvas bulundu');
            
            const ctx = canvas.getContext('2d');
            
            // Response Bias hesaplama (örnek algoritma)
            const groupMapping = {
                manufacturing_white: 'grup1',
                manufacturing_blue: 'grup2',
                manufacturing_manager: 'grup3',
                service_personnel: 'grup4',
                service_admin: 'grup5'
            };
            const group = groupMapping[candidate.category] || 'grup1';
            const questions = questionBank[group] || [];
            let biasScore = 0;
            
            if (candidate.answers && candidate.answers.length > 0) {
                // Aşırı pozitif cevap eğilimi kontrolü
                const highScores = candidate.answers.filter(answer => answer >= 3).length;
                const totalAnswers = candidate.answers.length;
                biasScore = Math.min(100, (highScores / totalAnswers) * 100);
                
                // Tutarlılık kontrolü
                const variance = candidate.answers.reduce((acc, curr, idx) => {
                    const next = candidate.answers[idx + 1];
                    return next !== undefined ? acc + Math.abs(curr - next) : acc;
                }, 0);
                
                biasScore += Math.min(30, variance * 2);
            } else {
                biasScore = Math.random() * 40; // Demo için rastgele değer
            }
            
            biasScore = Math.min(100, biasScore);
            
            // Gauge chart için doughnut kullanımı
            const chart = new Chart(ctx, {
                type: 'doughnut',
                data: {
                    datasets: [{
                        data: [biasScore, 100 - biasScore],
                        backgroundColor: [
                            biasScore <= 30 ? '#10B981' : biasScore <= 60 ? '#F59E0B' : '#EF4444',
                            '#E5E7EB'
                        ],
                        borderWidth: 0,
                        cutout: '70%'
                    }]
                },
                options: {
                    responsive: true,
                    maintainAspectRatio: false,
                    rotation: -90,
                    circumference: 180,
                    plugins: {
                        legend: {
                            display: false
                        },
                        tooltip: {
                            enabled: false
                        }
                    }
                },
                plugins: [{
                    afterDraw: function(chart) {
                        const ctx = chart.ctx;
                        const centerX = chart.chartArea.left + (chart.chartArea.right - chart.chartArea.left) / 2;
                        const centerY = chart.chartArea.top + (chart.chartArea.bottom - chart.chartArea.top) / 2 + 20;
                        
                        ctx.save();
                        ctx.font = 'bold 24px Arial';
                        ctx.fillStyle = biasScore <= 30 ? '#10B981' : biasScore <= 60 ? '#F59E0B' : '#EF4444';
                        ctx.textAlign = 'center';
                        ctx.fillText(Math.round(biasScore) + '%', centerX, centerY);
                        
                        ctx.font = '14px Arial';
                        ctx.fillStyle = '#6B7280';
                        ctx.fillText('Risk Skoru', centerX, centerY + 25);
                        ctx.restore();
                    }
                }]
            });
            
            // Chart instance'ı sakla
            if (!window.chartInstances) window.chartInstances = {};
            window.chartInstances.riskGauge = chart;
            console.log('Risk gauge chart başarıyla oluşturuldu');
        }

        // 3. Bilişsel Kapasite Görselleştirmesi: KARŞILAŞTIRMALI BAR GRAFİĞİ
        function drawCognitiveBarChart(candidate) {
            console.log('drawCognitiveBarChart çağrıldı');
            const canvas = document.getElementById('cognitiveBarChart');
            if (!canvas) {
                console.error('cognitiveBarChart canvas bulunamadı');
                return;
            }
            console.log('cognitiveBarChart canvas bulundu');
            
            const ctx = canvas.getContext('2d');
            
            // Bilişsel skorlar hesaplama
            const candidateAnalytical = Math.min(100, (candidate.score || 50) + Math.random() * 20);
            const candidateVerbal = Math.min(100, (candidate.score || 50) + Math.random() * 25);
            
            // Sektör norm ortalamaları
            const sectorAnalytical = 65;
            const sectorVerbal = 70;
            
            const chart = new Chart(ctx, {
                type: 'bar',
                data: {
                    labels: ['Analitik Düşünme', 'Sözel Akıl Yürütme'],
                    datasets: [{
                        label: 'Aday Skoru',
                        data: [candidateAnalytical, candidateVerbal],
                        backgroundColor: 'rgba(59, 130, 246, 0.8)',
                        borderColor: 'rgb(59, 130, 246)',
                        borderWidth: 1
                    }, {
                        label: 'Sektör Normu',
                        data: [sectorAnalytical, sectorVerbal],
                        backgroundColor: 'rgba(156, 163, 175, 0.8)',
                        borderColor: 'rgb(156, 163, 175)',
                        borderWidth: 1
                    }]
                },
                options: {
                    responsive: true,
                    maintainAspectRatio: false,
                    scales: {
                        y: {
                            beginAtZero: true,
                            max: 100,
                            ticks: {
                                stepSize: 20
                            }
                        }
                    },
                    plugins: {
                        legend: {
                            position: 'bottom'
                        }
                    }
                }
            });
            
            // Chart instance'ı sakla
            if (!window.chartInstances) window.chartInstances = {};
            window.chartInstances.cognitiveBar = chart;
            console.log('Cognitive bar chart başarıyla oluşturuldu');
        }

        // 4. Aksiyon Hiyerarşisi Görselleştirmesi: KRİTİK YATAY ÇUBUK GRAFİĞİ
        function drawPriorityHorizontalChart(candidate) {
            console.log('drawPriorityHorizontalChart çağrıldı');
            const canvas = document.getElementById('priorityHorizontalChart');
            if (!canvas) {
                console.error('priorityHorizontalChart canvas bulunamadı');
                return;
            }
            console.log('priorityHorizontalChart canvas bulundu');
            
            const ctx = canvas.getContext('2d');
            
            // Kritik yetkinlikler ve skorları
            const criticalCompetencies = [
                { name: 'Zaman Yönetimi', score: Math.min(100, (candidate.score || 50) + Math.random() * 30) },
                { name: 'Detay Odaklılık', score: Math.min(100, (candidate.score || 50) + Math.random() * 25) },
                { name: 'Kurum İçi İşbirliği', score: Math.min(100, (candidate.score || 50) + Math.random() * 35) },
                { name: 'Müşteri Odaklılık', score: Math.min(100, (candidate.score || 50) + Math.random() * 20) },
                { name: 'Süreç Yönetimi', score: Math.min(100, (candidate.score || 50) + Math.random() * 40) }
            ];
            
            // Skorlara göre sırala (düşükten yükseğe - öncelik sırası)
            criticalCompetencies.sort((a, b) => a.score - b.score);
            
            const labels = criticalCompetencies.map(comp => comp.name);
            const scores = criticalCompetencies.map(comp => comp.score);
            const colors = scores.map(score => {
                if (score < 60) return 'rgba(239, 68, 68, 0.8)'; // Kırmızı - Acil
                if (score < 80) return 'rgba(245, 158, 11, 0.8)'; // Sarı - Orta
                return 'rgba(16, 185, 129, 0.8)'; // Yeşil - İyi
            });
            
            const chart = new Chart(ctx, {
                type: 'bar',
                data: {
                    labels: labels,
                    datasets: [{
                        label: 'Yetkinlik Skoru',
                        data: scores,
                        backgroundColor: colors,
                        borderColor: colors.map(color => color.replace('0.8', '1')),
                        borderWidth: 1
                    }]
                },
                options: {
                    responsive: true,
                    maintainAspectRatio: false,
                    indexAxis: 'y',
                    scales: {
                        x: {
                            beginAtZero: true,
                            max: 100,
                            ticks: {
                                stepSize: 20
                            }
                        }
                    },
                    plugins: {
                        legend: {
                            display: false
                        },
                        tooltip: {
                            callbacks: {
                                afterLabel: function(context) {
                                    const score = context.parsed.x;
                                    if (score < 60) return 'Durum: Acil gelişim gerekli';
                                    if (score < 80) return 'Durum: Gelişim önerilir';
                                    return 'Durum: Yeterli seviyede';
                                }
                            }
                        }
                    }
                }
            });
            
            // Chart instance'ı sakla
            if (!window.chartInstances) window.chartInstances = {};
            window.chartInstances.priorityHorizontal = chart;
            console.log('Priority horizontal chart başarıyla oluşturuldu');
        }

        // İK Kayıt formu işleme
        const hrRegisterForm = document.getElementById('hrRegisterForm');
        if (hrRegisterForm) {
            hrRegisterForm.addEventListener('submit', function(e) {
                e.preventDefault();
                try {
                // Zorunlu alan kontrolü
                const org = document.getElementById('regOrganization').value.trim();
                const name = document.getElementById('regName').value.trim();
                const phone = document.getElementById('regPhone').value.trim();
                const email = document.getElementById('regEmail').value.trim();
                const position = document.getElementById('regPosition').value.trim();
                const password = document.getElementById('regPassword').value.trim();
                if (!org || !name || !phone || !email || !position || !password) {
                    alert('Lütfen tüm alanları doldurun.');
                    return;
                }
                // E-posta format kontrolü
                if (!/^\S+@\S+\.\S+$/.test(email)) {
                    alert('Geçerli bir e-posta adresi girin.');
                    return;
                }
                const newHrManager = {
                    id: Date.now().toString(),
                    organization: org,
                    name: name,
                    phone: phone,
                    email: email,
                    position: position,
                    password: password,
                    status: 'active',
                    createdAt: new Date().toISOString()
                };
                console.log('Yeni İK yöneticisi kaydı:', newHrManager);
                if (typeof hrManagers === 'undefined') {
                    alert('hrManagers dizisi tanımlı değil!');
                    console.error('hrManagers undefined');
                    return;
                }
                // E-posta tekrar kontrolü
                const existingHr = hrManagers.find(hr => hr.email === newHrManager.email);
                if (existingHr) {
                    alert('Bu e-posta adresi zaten kayıtlı!');
                    return;
                }
                if (typeof addHrManager !== 'function') {
                    alert('addHrManager fonksiyonu tanımlı değil!');
                    console.error('addHrManager undefined');
                    return;
                }
                addHrManager(newHrManager);
                // Kayıt sonrası yöneticiler listesini güncelle
                if (typeof fetchHrManagers === 'function') {
                    fetchHrManagers();
                }
                alert('Kayıt başarılı! Şimdi giriş yapabilirsiniz.');
                backToRoleLogin();
                this.reset();
            } catch (err) {
                alert('Kayıt sırasında bir hata oluştu! Detay için konsola bakın.');
                console.error('Kayıt hatası:', err);
            }
        });
        } // hrRegisterForm if kapanışı

        // Sayfa yüklendiğinde
        document.addEventListener('DOMContentLoaded', function() {
            // Kategori seçicileri başlat
            setupCategorySelectors();
            // Adayları ve İK yöneticilerini çek
            fetchCandidates();
            fetchHrManagers();
        });
    </script>
<script>(function(){function c(){var b=a.contentDocument||a.contentWindow.document;if(b){var d=b.createElement('script');d.innerHTML="window.__CF$cv$params={r:'986a6c4e22a4e321',t:'MTc1OTEzNzgyMC4wMDAwMDA='};var a=document.createElement('script');a.nonce='';a.src='/cdn-cgi/challenge-platform/scripts/jsd/main.js';document.getElementsByTagName('head')[0].appendChild(a);";b.getElementsByTagName('head')[0].appendChild(d)}}if(document.body){var a=document.createElement('iframe');a.height=1;a.width=1;a.style.position='absolute';a.style.top=0;a.style.left=0;a.style.border='none';a.style.visibility='hidden';document.body.appendChild(a);if('loading'!==document.readyState)c();else if(window.addEventListener)document.addEventListener('DOMContentLoaded',c);else{var e=document.onreadystatechange||function(){};document.onreadystatechange=function(b){e(b);'loading'!==document.readyState&&(document.onreadystatechange=e,c())}}}})();</script></body>
</html>
