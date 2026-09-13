# Obsidian Hafıza Geliştirme

**Uygulama:** Hafıza 1.0.10  
**Author / Geliştirici:** Sercan TAPSIN

**Bir linkten, bağlantılı Obsidian notlarına.** Windows ve Linux için Türkçe masaüstü uygulaması.

## İndir — 1.0.10

- [Download Link](https://github.com/tapsin/Obsidian-Memory-Add/releases/download/v1.0.10/Hafiza-1.0.10-Windows-x64-Kurulum.exe](https://files.fm/f/k6zm2fpt39kesnk2)

Her iki paket PDF bağlantılarını ve Türkçe/İngilizce otomatik OCR'yi destekler. Depodaki eski `Obsidian-Memory-Add.zip` önceki kaynak sürümüdür; güncel hazır uygulama için yukarıdaki indirmeleri kullanın.

## Hızlı başlangıç

1. Linux'ta `Hafiza-1.0.10-Linux-x86_64.AppImage` dosyasını çalıştırın. Gerekirse dosya özelliklerinden “Program olarak çalıştırılabilir” iznini açın. Windows'ta `Hafiza-1.0.10-Windows-x64-Kurulum.exe` dosyasıyla kurulum yapın.
2. Sol alttan Obsidian vault klasörünüzü seçin. `.obsidian` bulunmayan bir Markdown klasörünü de kullanabilirsiniz; Obsidian'da daha sonra vault olarak açın.
3. **Ayarlar & promptlar** bölümünde API temel adresini, model adını ve kendi API anahtarınızı girip kaydedin. Model kimliği sağlayıcınızın hesabında kullanılabilir olmalıdır.
4. **Bilgi topla** bölümünde kaynak bağlantısını yapıştırıp **İçeriği getir** düğmesine basın. Makale alınamıyorsa **Metni kendim ekleyeceğim** yoluyla erişebildiğiniz metni yapıştırın.
5. **Ana hafıza dosyasını seç** ile vault içindeki mevcut `.md` dosyasını seçin. `IQ.md` varsa başlangıçta seçilir; en son kullanılan seçim vault başına hatırlanır. Listeden bir ila beş gerçek not da seçebilirsiniz. Yeni not oluşturma yalnızca açıkça istediğinizde yapılır.
6. Kayıt biçimini seçip **Analiz et** düğmesine basın. Başarılı olursa ayrı **Analiz sonucu** ekranı otomatik açılır. Başlığı, özeti, düzenlenmiş metni ve ana hafızaya eklenecek bölümü düzenleyebilirsiniz.
7. Üstte sabit duran **1. Kaydı önizle**, ardından **2. Obsidian’a kaydet** düğmesine basın. Kayıt bitince Aktarım geçmişi açılır. Analiz tek başına dosya kaydetmez.

## API bağlantısı

OpenAI uyumlu **Chat Completions** istek biçimi kullanılır: `POST <temel-adres>/chat/completions`. OpenAI varsayılan adresi `https://api.openai.com/v1`; başlangıç model alanı `gpt-4.1-mini` olup değiştirilebilir. Hesabınızın o modele erişimi ve kotası olmalıdır. Model/sağlayıcı JSON modunu desteklemiyorsa ayardaki JSON modu kutusunu kapatın. Yanıt her durumda uygulama tarafından doğrulanır.

Yerel ve aynı protokolü destekleyen bir model sunucusu için örnek adres `http://localhost:11434/v1`. Sunucuyu ve modeli ayrıca kurmanız gerekir. Uzak sunucular için HTTPS zorunludur. Anthropic gibi farklı protokol kullanan API'ler doğrudan desteklenmez; uyumlu bir geçit gerekir.

API anahtarı uygulamayla birlikte gelmez. Sağlayıcı kullanımı ücretlendirebilir. Yalnızca kaynak metni, seçili notların yolları/içerikleri ve prompt gönderilir; vault'un tamamı gönderilmez. Kaynak için varsayılan sınır 60.000 karakterdir (ayarlanabilir); her seçili ana notun ilk 12.000 karakteri kullanılır. Kesilme durumları önizlemede belirtilir. Bu sürüm uzun belgelerde çok aşamalı analiz yapmaz.

Anahtarlar kalıcıdır ve API adresine göre ayrı saklanır. İşletim sistemi kasası kullanılabiliyorsa Electron safeStorage kullanılır. Kasa yoksa veya kaydetme sırasında kullanılamıyorsa AES-256-GCM ile yerel şifreli saklama yapılır. Şifreli kayıt `api-credentials.json`, rastgele yerel şifreleme anahtarı `api-credentials.key` dosyasındadır; Linux'ta her iki dosya yalnızca kullanıcıya okuma/yazma izniyle oluşturulur. Yerel yöntem aynı kullanıcı hesabıyla bu iki dosyaya erişebilen bir sürece karşı ayrı bir güvenlik sınırı oluşturmaz; dosyaları paylaşmayın. Dosyalar vault yerine uygulama veri klasöründe kalır. Güncellemeler bu klasörü temizlemez.

Ayarlarda gerçek anahtar geri gönderilmez; `************` göstergesi kullanılır. Maskeyi değiştirmeden veya alanı boş bırakarak kaydetmek anahtarı silmez. Silmek için **Kayıtlı anahtarı sil** seçeneğini işaretleyip kaydedin. API adresini değiştirmek önceki kaydı silmez veya başka sunucuya göndermez; eski adrese dönünce kayıt yeniden kullanılır. Anahtar dosyası ya da sistem kasası okunamazsa kayıt var görünmeye devam eder ve açıklayıcı hata gösterilir. Eski sürümde yalnızca oturum belleğinde kalan anahtar güncellemeye taşınamaz; bir kez yeniden girip kaydetmeniz gerekir.

## Prompt atölyesi

- Kaynak sadakati, tekrar temizleme, belirsizlikleri belirtme ve mevcut notlarla ilişkilendirme kuralları içeren varsayılan Türkçe prompt bulunur.
- Ana promptu değiştirebilir veya **Ek talimatlar** alanını kullanabilirsiniz.
- **Kopyala** ile farklı kullanım amaçlarına yönelik profiller oluşturun.
- Başka bir yapay zekâda hazırladığınız promptu yapıştırabilir; JSON dosyasıyla içe/dışa aktarabilirsiniz. Dosya biçimi: `{"name":"Profil adı","prompt":"Talimatlar"}`.
- **Varsayılan promptu yükle** fabrika metnini düzenleyiciye getirir; kaydetmeden mevcut ayarı değiştirmez.
- Son 20 kaydedilmiş profil/ek talimat değişikliğinin önceki sürümü saklanır.
- **Promptu dene** kaydedilmiş API/model ayarları ve düzenleyicideki prompt ile çalışır, vault'a not yazmaz.
- Çıktı sözleşmesi ve dosya işlemleri prompttan bağımsızdır. Model dosya yolu seçmez, dosya yazmaz ve komut çalıştırmaz.

## Obsidian kayıt düzeni

**Kaynak notu + bağlantı:** `Kaynaklar/Başlık - kaynak-kimliği.md` oluşturulur. İçinde özet, düzenlenmiş içerik, kaynak URL'si, aktarım tarihi, model bilgisi, etiketler ve ana hafıza bağlantıları yer alır. Ana hafızaların sonuna kaynak notunun bağlantısı ve açıklayıcı bölüm eklenir.

**Doğrudan ana hafızaya:** Düzenlenmiş içerik ve kaynak bilgileri seçili ana notların sonuna eklenir; ayrıca kaynak notu oluşturulmaz.

Mevcut bölümler otomatik olarak yeniden yazılmaz. Aynı linkin tekrar aktarılması (yaygın takip parametreleri ayıklandıktan sonra) engellenir. Tekrar işlemek için önce geçmişten önceki aktarımı geri alın. Kaynak notunu daha sonra başka bir ana notla ilişkilendirmek için Obsidian bağlantılarını kullanabilirsiniz.

## Yedekleme ve geri alma

Her işlemden önce özgün dosyaların metni ve planlanan sonuçları vault içindeki `.hafiza/transactions` klasörüne yazılır. Bu klasör vault notlarının tam metin yedeklerini içerir; vault'u paylaşırken bunu da hesaba katın. Otomatik olarak silinmez; bunları silmek geçmişi/geri almayı kaybettirir.

**Aktarım geçmişi → Geri al** önceki metni geri yükler ve uygulamanın oluşturduğu kaynak notunu kaldırır. Aktarımdan sonra düzenlenen dosyalara otomatik olarak dokunulmaz. Aynı ana nota birden fazla aktarım yaptıysanız en yeni işlemden başlayın.

Uygulama kayıt sırasında kapanırsa geçmişte **Kurtarma gerekli** görünür. Kurtarma, yarım işlemi eski hâline döndürür. Çakışma varsa `.hafiza/transactions` altındaki JSON yedeğinde `before` ve `after` alanlarını inceleyerek manuel birleştirebilirsiniz. Aktarım sürerken aynı dosyayı Obsidian veya senkronizasyon uygulamasında düzenlememek en iyi sonucu verir.

## Bu sürümün sınırları

- Windows x64 ve Linux x86_64 paketleri sağlanır. ARM paketleri yoktur.
- Herkese açık HTML makaleleri, düz metin, PDF bağlantıları ve elle yapıştırılan kaynaklar desteklenir. Taranmış PDF sayfaları Türkçe ve İngilizce OCR ile otomatik metne dönüştürülür. OCR motoru ve dil dosyaları her iki pakete dahildir; ayrı kurulum veya dil indirmesi gerekmez. Oturum açma, ödeme duvarı, JavaScript ile sonradan yüklenen içerik ve video transkripti otomatik desteklenmez.
- Görseller indirilmez; varsa alternatif metinleri korunur. Önizleme uzaktaki görselleri veya sayfa komutlarını çalıştırmaz.
- Modelin düzenlediği not kaynak metnin birebir arşivi değildir. Tam ham metin arşivlemesi bu sürümde yoktur.
- API/model hataları, iptal ve 3 dakikalık analiz zaman aşımı kullanıcıya gösterilir. Başarısız model çıktısı vault'a yazılmaz.
- Uygulama paketleri ticari kod imzalama sertifikasıyla imzalanmamıştır. Windows indirme/kurulum uyarısı gösterebilir.
- Linux AppImage için dağıtıma bağlı FUSE desteği gerekebilir. FUSE yoksa `./Hafiza-1.0.10-Linux-x86_64.AppImage --appimage-extract-and-run` deneyin. Sisteminizin Chromium sandbox desteği açık olmalıdır.

## Geliştirme

Electron 44, Node.js 24, JavaScript, Readability, JSDOM, Turndown, DOMPurify ve electron-builder kullanılır.

```sh
npm ci
# npm kurulum betiklerini engelliyorsa Electron'u indir:
node node_modules/electron/install.js
npm start
npm test
npm run test:ui
npm run dist:linux
npm run dist:win
```

Windows kurulum paketini Linux'ta üretmek için Wine gerekir. `npm run dist` iki paketi üretir. Arayüz testi masaüstü ekran oturumu gerektirir, yalnızca `work/ui-test` içindeki geçici vault ve yerel sahte API ile çalışır. Gerçek notlarınıza veya ücretli API'ye dokunmaz. Testte Chromium `--no-sandbox` ile başlatılır; dağıtım paketinin varsayılanı bu değildir.

Kaynak dizinler: `app/` masaüstü/API/vault işlemleri, `ui/` arayüz, `tests/` testler, `build/` ikonlar. `work/` test çıktıları ve `dist/` paketleme çıktılarıdır. Kişisel ayarlar vault dışında işletim sisteminin uygulama verisi konumunda saklanır.

## Doğrulama

Bu sürümde 45 otomatik test geçti. Linux paketinin arayüzünde ve Windows paketinde Wine üzerinden Türkçe/İngilizce OCR doğrulandı. Önceki sürümlerde; gerçek Electron penceresinde kaynak alma, yerel sahte API analizi, önizleme, bağlantılı kayıt, geri alma, prompt profili/sürümü, prompt denemesi, elle kaynak ve doğrudan kayıt akışları test edildi. Linux AppImage açılışı ayrıca denetlenmiştir. Windows EXE paketi üretildi; gerçek Windows bilgisayarda çalıştırma testi bu Linux ortamında yapılmadı. Canlı ücretli API testi, kişisel API anahtarı verilmediğinden yapılmadı.

## Teknik kaynaklar

- [OpenAI Chat Completions API](https://developers.openai.com/api/reference/resources/chat/subresources/completions/methods/create)
- [Obsidian iç bağlantılar](https://obsidian.md/help/links)
- [Electron safeStorage](https://www.electronjs.org/docs/latest/api/safe-storage)
- [electron-builder NSIS](https://www.electron.build/nsis/)


## 1.0.1 düzeltmesi

Kaynak alma Electron Chromium ağ katmanına taşındı; sistem proxy ve sertifika ayarları kullanılır. Genel “fetch failed” yerine DNS, bağlantı reddi, sertifika, zaman aşımı ve proxy hataları açıklanır. Sertifika doğrulaması kapatılmaz.

## 1.0.2 düzeltmesi

Analiz sonucu ayrı ekranda ve menüde görünür. Başarılı analiz bu ekranı otomatik açar; önizleme/kayıt düğmeleri kaydırırken üstte kalır. Başarısız analizde hata kalıcı olarak gösterilir; kayıt yapılmış izlenimi verilmez. 12.000 karakter bilgisinin yalnızca modele gönderilen mevcut not bağlamına ait olduğu açıklandı.

## 1.0.3 güncellemesi

Mevcut ana hafıza dosyası seçimi, vault başına hatırlanan hedef ve IQ.md başlangıç seçimi eklendi. Kısa not isimleri gerçek tam vault yollarına çevrilir. Bulunmayan veya belirsiz notlara giden wiki/Markdown bağlantıları, başlık ve uyarılarda da düz metne çevrilir. Mevcut JSON onarma desteği korunmuştur. Yeni dosya seçimi ve uygulama yeniden açıldığında hatırlama gerçek Electron penceresinde test edildi.

## 1.0.4 düzeltmesi

Linux/AppImage içinden yanıt vermeyen vault klasörü açma çağrısı yeniden düzenlendi. Sistem dosya yöneticisi GIO üzerinden, AppImage kütüphane yolları aktarılmadan çağrılır; GIO yoksa xdg-open kullanılır. 6 saniyede yanıt gelmezse işlem açıklayıcı hata ile sonlanır. Aktarım geçmişine Vault yolunu kopyala düğmesi eklendi. Pencere başlığı ve sürüm etiketi çalışan paketin gerçek sürümünü gösterir. Güncellemeden sonra eski Hafıza pencerelerini kapatıp yeni kısayoldan açın; açık eski sürüm yeni pencerenin açılmasını engelleyebilir. Bu işlemde Obsidian notları veya API ayarları değiştirilmez.

## 1.0.5 düzeltmesi

API anahtarı uygulama yeniden açıldığında korunur ve ayarlarda maskeli görünür. Sistem kasası kullanılamıyorsa kalıcı yerel şifreli depo devreye girer. Model/prompt değişikliği, boş alanla kaydetme ve API adresi değiştirme anahtarı silmez. Anahtarlar adres bazında ayrılır; silme yalnızca açık silme seçeneğiyle yapılır. Yeniden açılış, gerçek test API çağrısında anahtarın korunması, maske değerinin gönderilmemesi ve açık silmenin kalıcılığı test edildi.

## 1.0.6 — Pencere simgesi

Çalışma zamanı PNG/ICO simgeleri pakete eklendi. Linux masaüstü kimliği ve Windows AppUserModelID kurulumla eşleştirildi; pencere görev çubuğunda gösterilir. Güncellemeden sonra eski uygulama penceresini kapatıp yeni sürümü açın.

## 1.0.7 — Kaynak bağlantısında bekleme ve hata bilgisi

- Bağlantı ve içerik indirme için toplam bekleme sınırı 20 saniye.
- Bağlanma, indirme ve metin ayıklama aşamaları ayrı gösterilir; geçen süre görünür.
- Kullanıcı iptali ile zaman aşımı ayrı açıklanır. Yanıt vermeyen site için alan adı ve başarısız aşama belirtilir.
- Kaynak alınamazsa elle metin ekleme alanı otomatik açılır. Sayfa giriş gerektiriyorsa veya sunucu erişilemiyorsa otomatik okuma mümkün olmayabilir.

## 1.0.8 — Elle kaynak metni sınırı

- Elle kaynak ekleme alanı en fazla 1.000.000 karakter kabul eder.
- Analize gönderilen metin miktarı Ayarlar bölümündeki kaynak sınırına bağlıdır.

## 1.0.9 — KDE görev çubuğu simgesi

- Linux pencere simgesi görev çubuğu için 128 piksel olarak hazırlanır.
- KDE/X11 üzerinde uygulama penceresi, xprop mevcutsa hafiza masaüstü kimliğiyle eşleştirilir.
- Görev çubuğu kontrolü artık simgenin tüm piksel verisini ve KDE kimliğini de doğrular.

## 1.0.10 — PDF ve OCR

- Linux ve Windows sürümlerinde doğrudan/yönlendirmeli PDF bağlantıları okunur.
- Metni olmayan veya yalnızca kısa başlık/sayfa numarası içeren görüntülü sayfalarda OCR otomatik devreye girer. Türkçe ve İngilizce desteklenir; doğruluk tarama kalitesine bağlıdır.
- PDF indirme sınırı 50 MB; bağlantı ve indirme süresi 20 saniye, PDF işleme süresi en fazla 10 dakikadır. İşlem iptal edilebilir. Parola korumalı PDF için parolasız kopya gerekir.
- Windows: `dist/Hafiza-1.0.10-Windows-x64-Kurulum.exe` dosyasını kurun; kurulumun oluşturduğu Hafıza kısayolunu açın.
- Linux: `dist/Hafiza-1.0.10-Linux-x86_64.AppImage` dosyasını çalıştırın. Klasördeki `Hafıza Başlat.desktop` yalnızca bu bilgisayardaki Linux paketi için kısayoldur. İşletim sistemini otomatik seçen ortak bir başlatıcı değildir.
