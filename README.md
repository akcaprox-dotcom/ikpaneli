<!DOCTYPE html>
<html lang="tr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Giriş - Analiz Pro X</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0/css/all.min.css" rel="stylesheet">
</head>
<body class="bg-gradient-to-br from-blue-900 via-purple-900 to-indigo-900 min-h-screen flex items-center justify-center overflow-auto">
    <!-- Background Pattern -->
    <div class="absolute inset-0 bg-black opacity-30"></div>
    <div class="absolute inset-0" style="background-image: url('data:image/svg+xml,<svg width="60" height="60" viewBox="0 0 60 60" xmlns="http://www.w3.org/2000/svg"><g fill="none" fill-rule="evenodd"><g fill="%234F46E5" fill-opacity="0.05"><circle cx="30" cy="30" r="30"/></g></g></svg>'); background-size: 120px 120px;"></div>
    
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
    <div class="grid grid-cols-1 md:grid-cols-3 gap-6 md:gap-8">
        <!-- Admin Panel (Başlangıçta gizli) -->
    <div id="adminPanel" class="hidden fixed inset-0 bg-black bg-opacity-70 flex items-center justify-center z-[9999]">
        <!-- İK Paneli (Başlangıçta gizli) -->
        <div id="ikPanel" class="hidden fixed inset-0 bg-black bg-opacity-60 flex items-center justify-center z-50">
            <div class="bg-white rounded-2xl shadow-2xl p-10 w-full max-w-3xl relative overflow-y-auto max-h-screen">
                <button onclick="document.getElementById('ikPanel').classList.add('hidden')" class="absolute top-4 right-4 text-gray-400 hover:text-red-500 text-2xl">&times;</button>
                <h2 class="text-3xl font-bold text-green-800 mb-6 text-center">İK Yöneticisi Paneli</h2>
                <form id="addCandidateForm" class="bg-green-50 p-6 rounded-lg shadow mb-6">
                    <div class="grid grid-cols-1 md:grid-cols-2 gap-4">
                        <div>
                            <label class="block text-sm font-medium text-gray-700 mb-1">Aday Rumuz</label>
                            <input type="text" id="newCandidateNickname" required class="w-full px-3 py-2 border rounded-lg" placeholder="Rumuz">
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
            <div class="bg-white rounded-2xl shadow-2xl p-10 w-full max-w-2xl relative">
                <button onclick="document.getElementById('adminPanel').classList.add('hidden')" class="absolute top-4 right-4 text-gray-400 hover:text-red-500 text-2xl">&times;</button>
                <h2 class="text-3xl font-bold text-blue-800 mb-6 text-center">Yönetici Paneli</h2>
                <div class="space-y-4">
                    <button class="w-full bg-gradient-to-r from-blue-600 to-indigo-600 text-white py-3 px-6 rounded-lg font-semibold hover:from-blue-700 hover:to-indigo-700">İK Yöneticisi Ekle</button>
                    <button class="w-full bg-gradient-to-r from-green-600 to-green-700 text-white py-3 px-6 rounded-lg font-semibold hover:from-green-700 hover:to-green-800">Sistem Ayarları</button>
                    <button class="w-full bg-gradient-to-r from-red-600 to-red-700 text-white py-3 px-6 rounded-lg font-semibold hover:from-red-700 hover:to-red-800">Çıkış Yap</button>
                </div>
                <div class="mt-8 text-center text-gray-500 text-sm">© 2025 Analiz Pro X</div>
            </div>
        </div>
            <!-- Admin Login -->
            <div class="bg-white rounded-2xl shadow-2xl p-8 transform transition duration-500 hover:scale-105">
                <div class="text-center mb-6">
                    <div class="inline-flex items-center justify-center w-16 h-16 bg-red-100 rounded-full mb-4">
                        <i class="fas fa-crown text-2xl text-red-600"></i>
                    </div>
                    <h2 class="text-2xl font-bold text-gray-900 mb-2">Yönetici</h2>
                    <p class="text-gray-600">Sistem yöneticisi girişi</p>
                </div>

                <form id="adminLoginForm" class="space-y-6">
                    <div>
                        <label class="block text-sm font-medium text-gray-700 mb-2">
                            <i class="fas fa-user text-red-600 mr-2"></i>
                            Kullanıcı Adı
                        </label>
                        <input type="text" id="adminUsername" required 
                               class="w-full px-4 py-3 border border-gray-300 rounded-lg focus:ring-2 focus:ring-red-500 focus:border-transparent transition duration-200"
                               placeholder="admin">
                    </div>
                    
                    <div>
                        <label class="block text-sm font-medium text-gray-700 mb-2">
                            <i class="fas fa-lock text-red-600 mr-2"></i>
                            Şifre
                        </label>
                        <input type="password" id="adminPassword" required 
                               class="w-full px-4 py-3 border border-gray-300 rounded-lg focus:ring-2 focus:ring-red-500 focus:border-transparent transition duration-200"
                               placeholder="Şifrenizi girin">
                    </div>
                    
                    <button type="submit" 
                            class="w-full bg-gradient-to-r from-red-600 to-red-700 text-white py-3 px-6 rounded-lg hover:from-red-700 hover:to-red-800 focus:outline-none focus:ring-2 focus:ring-red-500 focus:ring-offset-2 transform transition duration-200 hover:scale-105 font-medium">
                        <i class="fas fa-sign-in-alt mr-2"></i>
                        Yönetici Girişi
                    </button>
                </form>
            </div>

            <!-- HR Manager Login -->
            <div class="bg-white rounded-2xl shadow-2xl p-8 transform transition duration-500 hover:scale-105">
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
                </form>
            </div>

            <!-- Candidate Login -->
            <div class="bg-white rounded-2xl shadow-2xl p-8 transform transition duration-500 hover:scale-105">
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

    <script src="giris-logic.js"></script>
    <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
    <script>
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
                'Çalışanlarıma düzenli ve yapıcı geri bildirim veririm.',
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
        candidateLoginForm.addEventListener('submit', function(e) {
            e.preventDefault();
            const nickname = document.getElementById('candidateNickname').value;
            const password = document.getElementById('candidatePassword').value;
            // İK panelinde kayıtlı adaylar ile kontrol
            const found = candidates.find(c => c.rumuz === nickname && c.sifre === password);
            if (found) {
                activeCandidate = found;
                candidateLoginForm.classList.add('hidden');
                // Soruları dinamik oluştur
                renderTestQuestions(found.tip, found.baslik);
                testSection.classList.remove('hidden');
            } else {
                alert('Rumuz veya şifre hatalı!');
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

    // Test cevaplarını kaydet ve İK paneline yansıt
    const testForm = document.getElementById('testForm');
    if (testForm) {
        testForm.onsubmit = function(e) {
            e.preventDefault();
            if (!activeCandidate) return;
            const selects = testForm.querySelectorAll('select');
            activeCandidate.cevaplar = Array.from(selects).map(s => s.value);
            alert('Cevaplarınız kaydedildi!');
            // İK panelindeki cevap listesine yansıt
            renderAnswers(activeCandidate);
            // Demo: rastgele puanlarla radar güncelle
            renderRadar([Math.random()*100, Math.random()*100, Math.random()*100]);
        }
    }

    // İK panelinde cevapları göster
    function renderAnswers(candidate) {
        const answerList = document.getElementById('answerList');
        if (!answerList) return;
        answerList.innerHTML = '';
        if (candidate && candidate.cevaplar && candidate.cevaplar.length) {
            candidate.cevaplar.forEach((cevap, i) => {
                answerList.innerHTML += `<li>${i+1}. Soru: ${cevap}</li>`;
            });
        }
    }

    // Admin giriş formu işlemleri
    const adminLoginForm = document.getElementById('adminLoginForm');
    const adminPanel = document.getElementById('adminPanel');
    const ikPanel = document.getElementById('ikPanel');
    if (adminLoginForm) {
        adminLoginForm.addEventListener('submit', function(e) {
            e.preventDefault();
            const adminUsername = document.getElementById('adminUsername').value;
            const adminPassword = document.getElementById('adminPassword').value;
            if (adminUsername === 'superadmin' && adminPassword === '030714') {
                // Tüm ana içeriği gizle
                const anaGrid = document.querySelector('.grid');
                if (anaGrid) anaGrid.style.display = 'none';
                adminPanel.classList.remove('hidden');
                adminPanel.style.display = 'flex';
            } else {
                alert('Kullanıcı adı veya şifre hatalı!');
            }
        });
    }

    // Admin panelinden İK panelini aç
    if (adminPanel) {
        adminPanel.querySelector('button').onclick = function() {
            adminPanel.classList.add('hidden');
            ikPanel.classList.remove('hidden');
        };
    }

    // İK paneli: aday ekleme ve listeleme
    let candidates = [];
    const addCandidateForm = document.getElementById('addCandidateForm');
    const candidateList = document.getElementById('candidateList');
    if (addCandidateForm) {
        addCandidateForm.onsubmit = function(e) {
            e.preventDefault();
            const rumuz = document.getElementById('newCandidateNickname').value;
            const sifre = document.getElementById('newCandidatePassword').value;
            const tip = document.getElementById('candidateType').value;
            const baslik = document.getElementById('questionCategory').value;
            candidates.push({rumuz, sifre, tip, baslik, cevaplar: []});
            renderCandidates();
            addCandidateForm.reset();
        };
    }
    function renderCandidates() {
        candidateList.innerHTML = '';
        candidates.forEach(c => {
            candidateList.innerHTML += `<tr><td class='p-2'>${c.rumuz}</td><td class='p-2'>${c.tip}</td><td class='p-2'>${c.baslik}</td></tr>`;
        });
    }

    // Radar grafik örneği (demo verisi)
    let radarChart;
    function renderRadar(dataArr) {
        const ctx = document.getElementById('radarChart').getContext('2d');
        if (radarChart) radarChart.destroy();
        radarChart = new Chart(ctx, {
            type: 'radar',
            data: {
                labels: ['İletişim', 'Liderlik', 'Teknik'],
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
    </script>
</body>
</html>
