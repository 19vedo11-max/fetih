# Fetih — Android Uygulaması

Bu klasör, `fetih.html` oyununu gerçek bir Android uygulamasına (APK/AAB)
dönüştürmek için hazırlanmış tam bir Android Studio projesidir. Oyunun
kendisi hiç değişmedi — `app/src/main/assets/www/fetih.html` içinde aynen
duruyor; bu proje sadece onu tek ekranlı bir WebView içinde açan ince bir
"sarmalayıcı" (wrapper) uygulama.

**Önemli not:** Bu proje burada, çalıştığım bulut ortamında derlenemedi —
bu ortamın ağ erişimi Android SDK'yı ve Gradle'ın ihtiyaç duyduğu
kütüphaneleri (Google'ın sunucuları, Maven Central) indiremiyor. Yani
elindeki bu klasör derlenmeye *hazır* ama henüz derlenmiş bir `.apk`
dosyası değil. Aşağıdaki iki yoldan biriyle (ikisi de senin tarafında,
normal internet erişimiyle) birkaç dakikada gerçek bir APK elde edersin.

## Yol 1 — GitHub Actions (kuruluma gerek yok, önerilen)

1. Ücretsiz bir GitHub hesabın yoksa aç, yeni bir repo oluştur (public ya
   da private fark etmez).
2. Bu klasördeki tüm dosyaları o repoya yükle (GitHub web arayüzünden
   sürükle-bırak ile de yapılabilir, ya da `git push`).
3. Repo sayfasında **Actions** sekmesine git — "Fetih APK Derle" iş akışı
   otomatik başlayacak (birkaç dakika sürer).
4. İş bitince aynı sayfada **Artifacts** altında `fetih-debug-apk` dosyasını
   indir, içinden `app-debug.apk` çıkar.
5. Bu APK'yı telefonuna kopyala (Drive, e-posta, USB — fark etmez), açıp
   kur. Android "bilinmeyen kaynaklardan yükleme" izni isteyebilir, izin
   ver.

Bu yol tamamen ücretsizdir ve bilgisayarına hiçbir şey kurmaz.

## Yol 2 — Android Studio (yerel derleme)

1. [Android Studio](https://developer.android.com/studio)'yu ücretsiz indir
   ve kur.
2. "Open" ile bu `fetih-mobile` klasörünü aç. İlk açılışta Gradle
   senkronizasyonu biraz zaman alır (Android SDK bileşenlerini kendi
   internetinle indirir).
3. Eğer "Gradle wrapper bulunamadı" gibi bir uyarı görürsen "Tamam" /
   "OK" diyerek Android Studio'nun kendi Gradle sürümüyle projeyi
   açmasına izin ver.
4. Üstteki menüden **Build → Build Bundle(s) / APK(s) → Build APK(s)**.
5. Derleme bitince çıkan bildirimden "locate" ile APK dosyasını bul,
   telefonuna aktarıp kur.

Gerçek bir cihazda test etmek yerine emülatörde de deneyebilirsin
(Android Studio > Device Manager).

## Google Play'e yayınlamak

Android tarafında satışa/yayına çıkmak için gereken adımlar özetle:

- **Google Play Console** hesabı aç — tek seferlik **25 $** kayıt ücreti
  var (yıllık değil).
- Play, yeni uygulamalar için **AAB** (Android App Bundle) formatı ister,
  APK değil. Android Studio'da "Build APK" yerine "Build App Bundle"
  seçip `.aab` dosyası üretmen gerekir (imzalama anahtarı — keystore —
  oluşturman istenecek, bunu kaybetmemen çok önemli, güncellemelerde
  tekrar lazım olacak).
- Hedef API seviyesi: bu proje zaten **API 36**'yı hedefliyor, Google
  Play'in Ağustos 2026 itibarıyla yeni uygulamalar için istediği güncel
  seviye bu.
- Mağaza listelemesi için: uygulama ikonu (bu projede hazır bir
  `store_icon_512.png` var, istersen kendi tasarımınla değiştir), en az
  birkaç ekran görüntüsü, kısa/uzun açıklama, ve **gizlilik politikası
  linki** gerekiyor (oyun çevrimiçi modda eşler arası bağlantı kurduğu
  için — bunu basit bir sayfada "hangi veriyi ne için kullanıyoruz"
  şeklinde açıklaman istenir; istersen sana bunun taslağını da
  hazırlayabilirim).
- İçerik derecelendirme anketini dolduruyorsun, ödeme/vergi profilini
  giriyorsun (satış yapacaksan).
- Google, Eylül 2026'dan itibaren bazı ülkelerde (önce Brezilya,
  Endonezya, Singapur, Tayland) yeni bir "geliştirici doğrulama"
  zorunluluğu getiriyor; normal Play Console kaydından geçen
  geliştiriciler zaten otomatik doğrulanmış sayılıyor, yani bu senin için
  ekstra bir engel olmayacak — asıl bu kural mağaza dışı ("sideload")
  dağıtımı hedefliyor.

## Satış / gelir modeli

Birkaç seçeneğin var, birbirini dışlamaz:

- **Ücretli indirme** — sabit bir fiyatla satarsın.
- **Reklam destekli** — ücretsiz, uygulama içine reklam SDK'sı eklenir
  (bu, mevcut projeye ek bir kütüphane entegrasyonu gerektirir).
- **Uygulama içi satın alma** — örn. kozmetik oyuncu renkleri/temalar,
  reklamsız sürüm vb.

Hangi modeli seçersen seç, Google Play satışlardan/uygulama içi
satın almalardan genelde **%15** (küçük geliştiriciler / ilk 1M$ gelir
için) ile **%30** arası komisyon alır — güncel oranı Play Console'da
kendi hesabında teyit etmen en sağlıklısı, zamanla değişebiliyor.
Türkiye'den bireysel/şirket olarak satış yaparken vergi (KDV, gelir
vergisi vb.) yükümlülüklerin olabilir — ben avukat ya da mali müşavir
değilim, bu konuda bir muhasebeciyle görüşmen en doğrusu olur.

## iOS (iPhone) tarafı

Bu proje şimdilik sadece Android için. iPhone'da yayınlamak için ayrı bir
Xcode projesi (WKWebView ile aynı mantık) ve **mutlaka bir Mac**
gerekiyor — App Store Connect'te yıllık **99 $** Apple Developer Program
üyeliği de şart. Mac'in yoksa bulut tabanlı bir Mac derleme servisi
(Codemagic, Xcode Cloud gibi) kullanılabilir. İstersen bir sonraki adımda
bu projenin iOS karşılığını da aynı şekilde hazırlayabilirim; o zaman
gerçek derleme ve mağazaya yükleme yine senin (ya da bağladığın bulut
Mac servisinin) elinde olacak.

## Çevrimiçi mod hakkında not

Oyunun çevrimiçi modu şu an PeerJS'in **ücretsiz, herkese açık** sinyal
sunucusunu kullanıyor. Birkaç arkadaşla oynamak için gayet yeterli, ama
gerçek bir "satılan ürün" için üzerine söz veremeyeceğin bir bağımlılık —
yoğun kullanımda kapasitesi/erişilebilirliği garanti değil. İleride
ciddi bir kullanıcı kitlen olursa kendi (ücretli, küçük) bir sinyal
sunucusu kurmak isteyebilirsin; bu ayrı bir iş, istersen o zaman
konuşuruz.
