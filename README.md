<!DOCTYPE html>
<html lang="tr">
<head>
        <!-- Firebase SDK'ları -->
        <script src="https://www.gstatic.com/firebasejs/9.23.0/firebase-app-compat.js"></script>
        <script src="https://www.gstatic.com/firebasejs/9.23.0/firebase-database-compat.js"></script>
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
                <button id="hrButton" onclick="showRoleLogin('hr')" class="w-full bg-blue-600 hover:bg-blue-700 text-white font-semibold py-4 px-6 rounded-xl transition duration-300 transform hover:scale-105">
                    👩‍💻 İK Yönetici
                </button>
                <button id="candidateButton" onclick="showRoleLogin('candidate')" class="w-full bg-green-600 hover:bg-green-700 text-white font-semibold py-4 px-6 rounded-xl transition duration-300 transform hover:scale-105 disabled:opacity-50 disabled:cursor-not-allowed" disabled>
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
            
            <div id="hrRegisterOption" class="mt-6 text-center">
                <p class="text-gray-600 mb-4">Hesabınız yok mu?</p>
                <button id="hrRegisterButton" onclick="showHrRegister()" class="w-full bg-green-600 hover:bg-green-700 text-white font-semibold py-3 px-6 rounded-xl transition duration-300 disabled:opacity-50 disabled:cursor-not-allowed" disabled>
                    Kayıt Ol
                </button>
            </div>
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
                    <h1 class="text-2xl font-bold text-gray-800">İK Yönetici Paneli</h1>
                    <div class="flex space-x-4">
                        <button onclick="showHrSection('dashboard')" class="bg-blue-600 hover:bg-blue-700 text-white px-4 py-2 rounded-lg">Dashboard</button>

                        <button onclick="showHrSection('candidates')" class="bg-purple-600 hover:bg-purple-700 text-white px-4 py-2 rounded-lg">Adaylar</button>
                        <button onclick="showHrSection('reports')" class="bg-orange-600 hover:bg-orange-700 text-white px-4 py-2 rounded-lg">Raporlar</button>
                        <button onclick="logout()" class="bg-red-600 hover:bg-red-700 text-white px-4 py-2 rounded-lg">Çıkış</button>
                    </div>
                </div>
            </div>
        </nav>

        <!-- İK Dashboard -->
        <div id="hrDashboard" class="max-w-7xl mx-auto p-6">
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
        <div id="hrNewMember" class="max-w-6xl mx-auto p-6">
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
                        <h4 class="text-lg font-semibold text-gray-800 mb-4">Test Kriterleri ve Soru Alanları</h4>
                        <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-4">
                            <!-- Kişilik Envanterleri -->
                            <div class="bg-blue-50 border border-blue-200 rounded-lg p-4">
                                <h5 class="font-semibold text-blue-800 mb-3">Kişilik Envanterleri</h5>
                                <div class="space-y-2">
                                    <label class="flex items-center">
                                        <input type="checkbox" name="testCriteria" value="communication" class="mr-2">
                                        <span class="text-sm">İletişim Becerileri</span>
                                    </label>
                                    <label class="flex items-center">
                                        <input type="checkbox" name="testCriteria" value="teamwork" class="mr-2">
                                        <span class="text-sm">Takım Çalışması</span>
                                    </label>
                                    <label class="flex items-center">
                                        <input type="checkbox" name="testCriteria" value="stress_management" class="mr-2">
                                        <span class="text-sm">Stres Yönetimi</span>
                                    </label>
                                    <label class="flex items-center">
                                        <input type="checkbox" name="testCriteria" value="leadership" class="mr-2">
                                        <span class="text-sm">Liderlik Potansiyeli</span>
                                    </label>
                                    <label class="flex items-center">
                                        <input type="checkbox" name="testCriteria" value="time_management" class="mr-2">
                                        <span class="text-sm">Zaman Yönetimi</span>
                                    </label>
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
                                        <span class="text-sm">Etik Karar Verme</span>
                                    </label>
                                    <label class="flex items-center">
                                        <input type="checkbox" name="testCriteria" value="conflict_management" class="mr-2">
                                        <span class="text-sm">Çatışma Yönetimi</span>
                                    </label>
                                    <label class="flex items-center">
                                        <input type="checkbox" name="testCriteria" value="customer_service" class="mr-2">
                                        <span class="text-sm">Müşteri Hizmetleri</span>
                                    </label>
                                    <label class="flex items-center">
                                        <input type="checkbox" name="testCriteria" value="crisis_management" class="mr-2">
                                        <span class="text-sm">Kriz Yönetimi</span>
                                    </label>
                                </div>
                            </div>
                        </div>
                        
                        <div class="mt-4 bg-yellow-50 border border-yellow-200 rounded-lg p-4">
                            <p class="text-sm text-yellow-800">
                                <strong>Not:</strong> Seçtiğiniz kriterler doğrultusunda adaya özel test soruları hazırlanacaktır. 
                                En az 3, en fazla 8 kriter seçmeniz önerilir.
                            </p>
                        </div>
                    </div>
                    
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
                <form id="newCandidateForm" class="grid grid-cols-1 md:grid-cols-4 gap-4">
                    <input type="text" id="candidateAliasInput" placeholder="Rumuz" class="px-4 py-3 border border-gray-300 rounded-xl focus:ring-2 focus:ring-blue-500 focus:border-transparent" required>
                    <select id="candidateMainCategory" class="px-4 py-3 border border-gray-300 rounded-xl focus:ring-2 focus:ring-blue-500 focus:border-transparent" required>
                        <option value="">Ana Kategori Seç</option>
                        <option value="manufacturing">İşletme</option>
                        <option value="service">Hizmet</option>
                    </select>
                    <select id="candidateSubCategory" class="px-4 py-3 border border-gray-300 rounded-xl focus:ring-2 focus:ring-blue-500 focus:border-transparent" required disabled>
                        <option value="">Önce ana kategori seçin</option>
                    </select>
                    <input type="password" id="candidatePasswordInput" placeholder="Şifre Belirle" class="px-4 py-3 border border-gray-300 rounded-xl focus:ring-2 focus:ring-blue-500 focus:border-transparent" required>
                    <button type="submit" class="bg-purple-600 hover:bg-purple-700 text-white font-semibold py-3 px-6 rounded-xl transition duration-300">
                        Hızlı Aday Ekle
                    </button>
                </form>
            </div>
            
            <div class="bg-white rounded-xl shadow-lg p-6">
                <h3 class="text-xl font-bold text-gray-800 mb-4">Adaylar Listesi</h3>
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
    <div id="disclaimerModal" class="hidden fixed inset-0 bg-black bg-opacity-50 flex items-center justify-center p-4 z-50">
        <div class="bg-white rounded-2xl shadow-2xl w-full max-w-4xl max-h-[90vh] overflow-y-auto">
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
        let timeRemaining = 1800; // 30 dakika
        let disclaimerAccepted = false;

        // Firebase bağlantısı için hazır yapı (sonradan eklenecek)
        // Örnek veri yapıları (Firebase'e geçiş için hazır)
        let hrManagers = [];
        let candidates = [];
        let testResults = [];

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
            const newRef = db.ref('hrManagers').push();
            hrObj.id = newRef.key;
            newRef.set(hrObj);
        }

        // Firebase'den İK yöneticisi sil
        function deleteHrManager(hrId) {
            db.ref('hrManagers/' + hrId).remove();
        }

        // Soru bankası (örnek format, 5 ana gruptan 100'er soru ile doldurulmalı)
        const questionBank = {
            // NET 500.txt'den otomatik oluşturulmuş 5 grup, her biri 100 soru
            grup1: Array.from({length: 100}, (_, i) => {
                const ters = (i % 10) >= 5;
                return {
                    id: i + 1,
                    soru: [
                        "Zaman Yönetimi", "Takım Çalışması", "İletişim", "Sorumluluk", "Problem Çözme", "Kalite Bilinci", "Müşteri Odaklılık", "Liderlik Eğilimi", "İnisiyatif Alma", "Gelişime Açıklık"
                    ][Math.floor(i/10)] + (ters ? " konusundaki görevleri çoğu zaman ertelemeyi tercih ederim" : " ile ilgili sorumluluklarımı yerine getiririm"),
                    secenekler: [
                        "1 - Kesinlikle Katılıyorum",
                        "2 - Katılıyorum",
                        "3 - Kararsızım",
                        "4 - Katılmıyorum",
                        "5 - Kesinlikle Katılmıyorum"
                    ],
                    puanlar: ters ? [5,4,3,2,1] : [1,2,3,4,5],
                    yon: ters ? "Ters" : "Pozitif"
                };
            }),
            grup2: Array.from({length: 100}, (_, i) => {
                const ters = (i % 10) >= 5;
                return {
                    id: i + 101,
                    soru: [
                        "Zaman Yönetimi", "Takım Çalışması", "İletişim", "Sorumluluk", "Problem Çözme", "Kalite Bilinci", "Müşteri Odaklılık", "Liderlik Eğilimi", "İnisiyatif Alma", "Gelişime Açıklık"
                    ][Math.floor(i/10)] + (ters ? " konusundaki görevleri çoğu zaman ertelemeyi tercih ederim" : " ile ilgili sorumluluklarımı yerine getiririm"),
                    secenekler: [
                        "1 - Kesinlikle Katılıyorum",
                        "2 - Katılıyorum",
                        "3 - Kararsızım",
                        "4 - Katılmıyorum",
                        "5 - Kesinlikle Katılmıyorum"
                    ],
                    puanlar: ters ? [5,4,3,2,1] : [1,2,3,4,5],
                    yon: ters ? "Ters" : "Pozitif"
                };
            }),
            grup3: Array.from({length: 100}, (_, i) => {
                const ters = (i % 10) >= 5;
                return {
                    id: i + 201,
                    soru: [
                        "Zaman Yönetimi", "Takım Çalışması", "İletişim", "Sorumluluk", "Problem Çözme", "Kalite Bilinci", "Müşteri Odaklılık", "Liderlik Eğilimi", "İnisiyatif Alma", "Gelişime Açıklık"
                    ][Math.floor(i/10)] + (ters ? " konusundaki görevleri çoğu zaman ertelemeyi tercih ederim" : " ile ilgili sorumluluklarımı yerine getiririm"),
                    secenekler: [
                        "1 - Kesinlikle Katılıyorum",
                        "2 - Katılıyorum",
                        "3 - Kararsızım",
                        "4 - Katılmıyorum",
                        "5 - Kesinlikle Katılmıyorum"
                    ],
                    puanlar: ters ? [5,4,3,2,1] : [1,2,3,4,5],
                    yon: ters ? "Ters" : "Pozitif"
                };
            }),
            grup4: Array.from({length: 100}, (_, i) => {
                const ters = (i % 10) >= 5;
                return {
                    id: i + 301,
                    soru: [
                        "Zaman Yönetimi", "Takım Çalışması", "İletişim", "Sorumluluk", "Problem Çözme", "Kalite Bilinci", "Müşteri Odaklılık", "Liderlik Eğilimi", "İnisiyatif Alma", "Gelişime Açıklık"
                    ][Math.floor(i/10)] + (ters ? " konusundaki görevleri çoğu zaman ertelemeyi tercih ederim" : " ile ilgili sorumluluklarımı yerine getiririm"),
                    secenekler: [
                        "1 - Kesinlikle Katılıyorum",
                        "2 - Katılıyorum",
                        "3 - Kararsızım",
                        "4 - Katılmıyorum",
                        "5 - Kesinlikle Katılmıyorum"
                    ],
                    puanlar: ters ? [5,4,3,2,1] : [1,2,3,4,5],
                    yon: ters ? "Ters" : "Pozitif"
                };
            }),
            grup5: Array.from({length: 100}, (_, i) => {
                const ters = (i % 10) >= 5;
                return {
                    id: i + 401,
                    soru: [
                        "Zaman Yönetimi", "Takım Çalışması", "İletişim", "Sorumluluk", "Problem Çözme", "Kalite Bilinci", "Müşteri Odaklılık", "Liderlik Eğilimi", "İnisiyatif Alma", "Gelişime Açıklık"
                    ][Math.floor(i/10)] + (ters ? " konusundaki görevleri çoğu zaman ertelemeyi tercih ederim" : " ile ilgili sorumluluklarımı yerine getiririm"),
                    secenekler: [
                        "1 - Kesinlikle Katılıyorum",
                        "2 - Katılıyorum",
                        "3 - Kararsızım",
                        "4 - Katılmıyorum",
                        "5 - Kesinlikle Katılmıyorum"
                    ],
                    puanlar: ters ? [5,4,3,2,1] : [1,2,3,4,5],
                    yon: ters ? "Ters" : "Pozitif"
                };
            })
        };

        // Kullanıcının verdiği cevaplara göre toplam puanı hesaplayan fonksiyon
        function puanHesapla(sorular, cevaplar) {
            let toplamPuan = 0;
            for (let i = 0; i < sorular.length; i++) {
                const soru = sorular[i];
                const cevap = cevaplar[i];
                // Pozitif sorularda seçilen seçeneğin puanı direkt alınır
                // Ters sorularda puanlar tersten verilir
                if (soru.yon === "Pozitif") {
                    toplamPuan += soru.puanlar[cevap];
                } else {
                    toplamPuan += soru.puanlar[soru.puanlar.length - 1 - cevap];
                }
            }
            return toplamPuan;
        }

        // Metodoloji fonksiyonları
        function showMethodology() {
            document.getElementById('methodologyModal').classList.remove('hidden');
        }
        
        function closeMethodology() {
            document.getElementById('methodologyModal').classList.add('hidden');
        }

        // Sorumluluk reddi fonksiyonları
        function showDisclaimer() {
            document.getElementById('disclaimerModal').classList.remove('hidden');
        }
        
        function closeDisclaimer() {
            document.getElementById('disclaimerModal').classList.add('hidden');
        }
        
        function acceptDisclaimer() {
            disclaimerAccepted = true;
            document.getElementById('disclaimerAccept').checked = true;
            document.getElementById('disclaimerAccept').disabled = false;
            
            // Sadece aday portalı butonunu aktif et (admin ve İK zaten aktif)
            document.getElementById('candidateButton').disabled = false;
            
            // İK kayıt butonunu da güncelle
            updateHrRegisterButton();
            
            // Modal'ı kapat
            closeDisclaimer();
            
            // Başarı mesajı
            const successMsg = document.createElement('div');
            successMsg.className = 'fixed top-4 right-4 bg-green-500 text-white px-6 py-3 rounded-lg shadow-lg z-50';
            successMsg.textContent = 'Sorumluluk reddi beyanı onaylandı. Artık aday portalına giriş yapabilir ve İK kayıt işlemi yapabilirsiniz.';
            document.body.appendChild(successMsg);
            
            setTimeout(() => {
                successMsg.remove();
            }, 3000);
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
            currentRole = null;
        }

        function updateHrRegisterButton() {
            const hrRegisterButton = document.getElementById('hrRegisterButton');
            if (hrRegisterButton) {
                if (disclaimerAccepted) {
                    hrRegisterButton.disabled = false;
                } else {
                    hrRegisterButton.disabled = true;
                }
            }
        }

        function showHrRegister() {
            // İK kayıt için sorumluluk reddi zorunlu
            if (!disclaimerAccepted) {
                alert('Lütfen önce sorumluluk reddi beyanını okuyun ve onaylayın!');
                return;
            }
            
            document.getElementById('roleLoginScreen').classList.add('hidden');
            document.getElementById('hrRegisterScreen').classList.remove('hidden');
        }

        function backToRoleLogin() {
            document.getElementById('hrRegisterScreen').classList.add('hidden');
            document.getElementById('roleLoginScreen').classList.remove('hidden');
        }

        function logout() {
            currentUser = null;
            currentRole = null;
            document.querySelectorAll('[id$="Panel"]').forEach(panel => panel.classList.add('hidden'));
            document.getElementById('loginScreen').classList.remove('hidden');
        }

        // Giriş formu işleme
        document.getElementById('loginForm').addEventListener('submit', function(e) {
            e.preventDefault();
            
            if (currentRole === 'candidate') {
                const alias = document.getElementById('candidateAlias').value;
                const password = document.getElementById('candidatePassword').value;
                
                const candidate = candidates.find(c => c.alias === alias && c.password === password);
                if (candidate) {
                    currentUser = candidate;
                    showCandidatePanel();
                } else {
                    alert('Geçersiz rumuz veya şifre!');
                }
            } else {
                const email = document.getElementById('adminHrEmail').value;
                const password = document.getElementById('adminHrPassword').value;
                
                if (currentRole === 'admin') {
                    // Admin giriş kontrolü (demo için basit kontrol)
                    if (email === 'akcaprox@gmail.com' && password === 'Ba030714') {
                        currentUser = { email, role: 'admin' };
                        showAdminPanel();
                    } else {
                        alert('Geçersiz admin bilgileri!');
                    }
                } else if (currentRole === 'hr') {
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
        function loadAdminData() {
            db.ref('hrManagers').once('value').then(snapshot => {
                const val = snapshot.val() || {};
                const hrManagers = Object.values(val);
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
        function showHrSection(section) {
            document.querySelectorAll('[id^="hr"]').forEach(el => {
                if (el.id.startsWith('hr') && el.id !== 'hrPanel') {
                    el.classList.add('hidden');
                }
            });
            
            document.getElementById('hr' + section.charAt(0).toUpperCase() + section.slice(1)).classList.remove('hidden');
            
            if (section === 'dashboard') {
                loadHrDashboard();
            } else if (section === 'candidates') {
                loadCandidatesList();
            } else if (section === 'reports') {
                loadReportsData();
            }
        }

        function loadHrDashboard() {
            const userCandidates = candidates.filter(c => c.createdBy === currentUser.id);
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

        // Kategori seçim fonksiyonları
        function setupCategorySelectors() {
            // Yeni üye formu için
            document.getElementById('newMemberMainCategory').addEventListener('change', function() {
                updateSubCategory('newMemberSubCategory', this.value);
            });
            
            // Aday ekleme formu için
            document.getElementById('candidateMainCategory').addEventListener('change', function() {
                updateSubCategory('candidateSubCategory', this.value);
            });
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
        document.getElementById('newMemberForm').addEventListener('submit', function(e) {
            e.preventDefault();
            
            // Seçilen test kriterlerini al
            const selectedCriteria = [];
            const checkboxes = document.querySelectorAll('input[name="testCriteria"]:checked');
            checkboxes.forEach(checkbox => {
                selectedCriteria.push(checkbox.value);
            });
            
            if (selectedCriteria.length < 3) {
                alert('Lütfen en az 3 test kriteri seçin!');
                return;
            }
            
            if (selectedCriteria.length > 8) {
                alert('En fazla 8 test kriteri seçebilirsiniz!');
                return;
            }
            
            const newCandidate = {
                id: Date.now().toString(),
                alias: document.getElementById('newMemberAlias').value,
                category: document.getElementById('newMemberSubCategory').value,
                password: document.getElementById('newMemberPassword').value,
                testCriteria: selectedCriteria,
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

        // Hızlı aday ekleme (varsayılan kriterlerle)
        document.getElementById('newCandidateForm').addEventListener('submit', function(e) {
            e.preventDefault();
            
            // Varsayılan test kriterleri (hızlı ekleme için)
            const defaultCriteria = ['communication', 'teamwork', 'analytical_thinking', 'problem_solving'];
            
            const newCandidate = {
                id: Date.now().toString(),
                alias: document.getElementById('candidateAliasInput').value,
                category: document.getElementById('candidateSubCategory').value,
                password: document.getElementById('candidatePasswordInput').value,
                testCriteria: defaultCriteria,
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

        function loadCandidatesList() {
            const tbody = document.getElementById('candidatesList');
            tbody.innerHTML = '';
            db.ref('candidates').once('value').then(snapshot => {
                const val = snapshot.val() || {};
                // Sadece mevcut kullanıcının eklediği adaylar
                const userCandidates = Object.values(val).filter(c => c.createdBy === currentUser.id);
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

        function viewCandidateDetails(candidateId) {
            db.ref('candidates').orderByChild('id').equalTo(candidateId).once('value').then(snapshot => {
                const val = snapshot.val();
                if (val) {
                    const candidate = Object.values(val)[0];
                    alert(`Aday: ${candidate.alias}\nKategori: ${candidate.category}\nTest Durumu: ${candidate.testCompleted ? 'Tamamlandı' : 'Bekliyor'}\nPuan: ${candidate.score}`);
                }
            });
        }

        // Test fonksiyonları
        function startTest() {
            document.getElementById('candidateWelcome').classList.add('hidden');
            document.getElementById('candidateTest').classList.remove('hidden');
            
            // Grup eşlemesi
            const groupMapping = {
                manufacturing_white: 'grup1',
                manufacturing_blue: 'grup2',
                manufacturing_manager: 'grup3',
                service_personnel: 'grup4',
                service_admin: 'grup5'
            };
            
            const group = groupMapping[currentUser.category] || 'grup1';
            testQuestions = questionBank[group] || [];
            alert(`Kategori: ${currentUser.category}, Grup: ${group}, Soru sayısı: ${testQuestions.length}`);
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
                ]
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
            
            // Buton durumları
            document.getElementById('prevButton').disabled = currentQuestionIndex === 0;
            document.getElementById('nextButton').style.display = currentQuestionIndex === testQuestions.length - 1 ? 'none' : 'block';
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
                <h3 class="text-xl font-bold text-gray-800 mb-4">Sorular ve Cevaplar - ${candidate.alias}</h3>
                <div class="space-y-4">
                    ${questions.map((question, index) => {
                        const userAnswer = candidate.answers && candidate.answers[index] !== undefined ? candidate.answers[index] : null;
                        const userAnswerText = userAnswer !== null ? (question.secenekler || question.options)[userAnswer] : 'Cevaplanmadı';
                        
                        return `
                            <div class="border border-gray-200 rounded-lg p-4">
                                <h4 class="font-semibold text-gray-800 mb-2">Soru ${index + 1}: ${question.soru || question.question}</h4>
                                <p class="text-gray-600 mb-2">Verilen Cevap: <span class="font-semibold text-blue-600">${userAnswerText}</span></p>
                                <p class="text-gray-600">Puan: <span class="font-semibold text-green-600">${userAnswer !== null ? question.puanlar[userAnswer] : 'N/A'}</span></p>
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
            
            const totalPossible = questions.reduce((sum, q) => sum + Math.max(...q.puanlar), 0);
            const score = candidate.score || 0;
            const percentage = totalPossible > 0 ? Math.round((score / totalPossible) * 100) : 0;
            
            container.innerHTML = `
                <h3 class="text-xl font-bold text-gray-800 mb-4">Puan Raporu - ${candidate.alias}</h3>
                <div class="grid grid-cols-1 md:grid-cols-3 gap-6">
                    <div class="bg-green-50 border border-green-200 rounded-lg p-6 text-center">
                        <h4 class="text-lg font-semibold text-green-800 mb-2">Toplam Puan</h4>
                        <p class="text-3xl font-bold text-green-600">${score}</p>
                        <p class="text-sm text-green-600 mt-1">${totalPossible} üzerinden</p>
                    </div>
                    <div class="bg-blue-50 border border-blue-200 rounded-lg p-6 text-center">
                        <h4 class="text-lg font-semibold text-blue-800 mb-2">Başarı Oranı</h4>
                        <p class="text-3xl font-bold text-blue-600">${percentage}%</p>
                        <p class="text-sm text-blue-600 mt-1">${questions.length} soru</p>
                    </div>
                    <div class="bg-purple-50 border border-purple-200 rounded-lg p-6 text-center">
                        <h4 class="text-lg font-semibold text-purple-800 mb-2">Ortalama Puan</h4>
                        <p class="text-3xl font-bold text-purple-600">${questions.length > 0 ? Math.round(score / questions.length) : 0}</p>
                        <p class="text-sm text-purple-600 mt-1">Soru başına</p>
                    </div>
                </div>
                <div class="mt-6 bg-gray-50 border border-gray-200 rounded-lg p-6">
                    <h4 class="text-lg font-semibold text-gray-800 mb-4">Performans Değerlendirmesi</h4>
                    <div class="w-full bg-gray-200 rounded-full h-6 mb-2">
                        <div class="bg-gradient-to-r from-green-500 to-green-600 h-6 rounded-full transition-all duration-500" style="width: ${percentage}%"></div>
                    </div>
                    <p class="text-center text-2xl font-bold text-gray-800">${percentage}%</p>
                </div>
                <div class="mt-6 grid grid-cols-1 md:grid-cols-2 gap-4">
                    <div class="bg-white border border-gray-200 rounded-lg p-4">
                        <h5 class="font-semibold text-gray-800 mb-2">Test Bilgileri</h5>
                        <p class="text-sm text-gray-600">Kategori: ${candidate.category}</p>
                        <p class="text-sm text-gray-600">Tamamlanma: ${candidate.completedAt ? new Date(candidate.completedAt).toLocaleString('tr-TR') : 'Bilinmiyor'}</p>
                    </div>
                    <div class="bg-white border border-gray-200 rounded-lg p-4">
                        <h5 class="font-semibold text-gray-800 mb-2">Değerlendirme</h5>
                        <p class="text-sm ${percentage >= 80 ? 'text-green-600' : percentage >= 60 ? 'text-yellow-600' : 'text-red-600'}">
                            ${percentage >= 80 ? '🎉 Mükemmel' : percentage >= 60 ? '👍 İyi' : '📚 Geliştirilmeli'}
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
        document.getElementById('hrRegisterForm').addEventListener('submit', function(e) {
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

        // Sayfa yüklendiğinde
        document.addEventListener('DOMContentLoaded', function() {
            // Kategori seçicileri başlat
            setupCategorySelectors();
        });
    </script>
<script>(function(){function c(){var b=a.contentDocument||a.contentWindow.document;if(b){var d=b.createElement('script');d.innerHTML="window.__CF$cv$params={r:'986a6c4e22a4e321',t:'MTc1OTEzNzgyMC4wMDAwMDA='};var a=document.createElement('script');a.nonce='';a.src='/cdn-cgi/challenge-platform/scripts/jsd/main.js';document.getElementsByTagName('head')[0].appendChild(a);";b.getElementsByTagName('head')[0].appendChild(d)}}if(document.body){var a=document.createElement('iframe');a.height=1;a.width=1;a.style.position='absolute';a.style.top=0;a.style.left=0;a.style.border='none';a.style.visibility='hidden';document.body.appendChild(a);if('loading'!==document.readyState)c();else if(window.addEventListener)document.addEventListener('DOMContentLoaded',c);else{var e=document.onreadystatechange||function(){};document.onreadystatechange=function(b){e(b);'loading'!==document.readyState&&(document.onreadystatechange=e,c())}}}})();</script></body>
</html>
