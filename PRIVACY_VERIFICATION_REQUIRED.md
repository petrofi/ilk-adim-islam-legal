# Pişsek? gizlilik doğrulaması

İnceleme tarihi: 25 Ekim 2026. Kapsam: `com.pissek.app`, 0.3.0 geliştirme kaynakları (versionCode 7). Bu dosya geliştirici kontrol listesidir; yayımlanan politika `pissek/privacy/index.html` dosyasındadır. Başka uygulamaların politikalarını değiştirmez.

## Kaynak koduyla doğrulananlar

Kaynak yolları Pişsek? Android projesine göredir; bu hukuk deposu Android kaynaklarını içermez.

| Konu | Kanıt | Sonuç |
| --- | --- | --- |
| Yerel kullanıcı kayıtları | `app/src/main/java/com/pissek/app/data/Database.kt` | Room: favoriler, kaydırma geçmişi ve zamanları, alışveriş listesi, dolap, oturumlar, zamanlayıcılar. |
| Veritabanının konumu | `PissekApplication.kt` | Uygulama özel alanında `pissek.db`; kullanıcı sunucusu veya hesap kurulumu yok. |
| Tercihler | `data/PreferenceStore.kt` | DataStore: ilk kurulum, tema, porsiyon, filtreler, alerjen hariç tutmaları, temel dolap, ses, erişilebilirlik, keşif döngüsü, taslak görünürlüğü. |
| Sesli Şef | `voice/HybridVoiceEngine.kt`, `voice/RecordedAudioVoiceEngine.kt` | Doğrulanmış yerel kayıt; kayıt yoksa Android TTS. Dinleme konumları `recorded_voice_positions` SharedPreferences alanında. |
| Sistem TTS | `voice/AndroidTtsVoiceEngine.kt`, `ui/ScreensSettings.kt` | Metin, hız ve ses seçimi sistem motoruna iletilir. Ağ gerektiren ve cihazda çalışan sesler ayrılır. |
| Mikrofon / konuşma tanıma | Tüm `app/src/main` Kotlin kaynakları ve ana manifest | `SpeechRecognizer`, `RecognizerIntent`, `RECORD_AUDIO` ve mikrofonla kayıt akışı yok. Mikrofon simgesi bir özellik kanıtı olarak kullanılmadı. |
| Reklam altyapısı | `app/build.gradle.kts`, `ads/GoogleMonetization.kt`, `ads/ConsentController.kt`, `ui/ConsentBanner.kt`, `ui/ScreensSettings.kt` | GMA Next-Gen 1.6.0 ve UMP 4.0.0. Debug/QA Google test kimlikleriyle çalışır; varsayılan release reklamları kapalıdır. UMP güncelleme/form çağrıları ve `canRequestAds` denetimi uygulanır. Ana sayfa/favoriler banner uygunluğu pişirme, zamanlayıcı ve anlatım durumlarına bağlıdır. Reklam kişiselleştirmesi kapatılır, NPA istenir. Etkin ödül teklifi ve satın alma akışı yoktur. |
| Dosya indirme | `voice/VoicePackStore.kt`, `ui/ScreensSettings.kt` | Kullanıcı HTTPS ZIP URL'si ve SHA-256 girerek başlatır. Hazır sunucu yok. Yönlendirme izlenmez; boyut ve dosya bütünlüğü denetlenir. |
| Örnek sesler | `voice/ApprovedSamplePack.kt` | Uygulamayla gelen üç örnek yerel olarak kurulabilir. |
| Üçüncü taraf aktarımı | Tüm ana Kotlin kaynakları | Favori, liste, geçmiş ve tercihleri dışa gönderen kod yok. Ağ TTS, seçilen indirme sunucusu, destek e-postası ve Pages bağlantıları ayrıca açıklanır. |
| İzinler | Ana manifest, `timer/TimerCoordinator.kt`, debug/release birleştirilmiş manifestleri | INTERNET, POST_NOTIFICATIONS, RECEIVE_BOOT_COMPLETED; SDK'lardan ACCESS_NETWORK_STATE, READ_BASIC_PHONE_STATE, WAKE_LOCK, FOREGROUND_SERVICE ve uygulamanın imza korumalı alıcı izni. Güncel kaynak manifest AD_ID, AdServices reklam izinleri ve konum izinlerini açıkça kaldırır. Mikrofon, kamera ve rehber izni yok. |
| Yerel silme | `ui/KitchenViewModel.kt::clearData`, `data/KitchenRepository.kt::clearUserData`, `data/PreferenceStore.kt::reset` | Kullanıcı tabloları, tercihler, zamanlayıcılar, indirilen paketler ve dinleme konumları temizlenir; tarif kataloğu korunur. |
| Yedekleme | Ana manifest, `res/xml/data_extraction_rules.xml` | `allowBackup=false`; bulut yedekleme ve cihaz aktarımı dışlamaları var. |
| Kullanıcı kontrolleri | `ui/ScreensSettings.kt` | Ses aç/kapa, ses/hız seçimi, bildirim ayarları, tercih düzenleme, yerel silme. UMP gerekli gördüğünde reklam gizlilik tercihleri yeniden açılabilir. Doğrulanmış politika URL'si yapılandırıldığında tarayıcı bağlantısı gösterilir. |

## Android üretim yayını öncesinde ayrıca doğrulanacaklar

- [ ] **Play Console Veri güvenliği formu:** Mevcut form bu incelemeye sağlanmadı. Politika ile formun birebir tutarlılığı onaylanmış değildir. Nihai AAB/APK, üçüncü taraf SDK davranışları ve aşağıdaki ağ senaryoları üzerinden beyanı tamamlayın. Yalnız cihazda işlenen kayıtları sunucuya toplanmış gibi, uygulamada SDK varken tüm uygulamayı koşulsuz “hiç veri toplamıyor” gibi beyan etmeyin.
- [ ] **Nihai ikilinin ağ denetimi:** Test reklamı/UMP entegrasyonu kaynak koduyla doğrulandı; üçüncü tarafların tüm otomatik veya geçişli ağ davranışları trafik yakalama ile henüz kanıtlanmadı. Soğuk açılış, ağ TTS, çevrimdışı TTS, ses paketi indirme, silme ve reklam/UMP senaryolarını nihai sürümde ölçün. Test kimliği kullanımı veri işlenmediği anlamına gelmez.
- [x] **Manifest denetimi:** İlk debug/release manifest derlemeleri tamamlandı; eşzamanlı Android reklam geliştirmesinden sonra izin kaldırma kuralları yeniden incelendi. Güncel derleme sonuçları teslim raporunda belirtilir. Bu kontrol ağ trafiğinin ölçülmesi yerine geçmez.
- [ ] **Nihai mağaza ikilisi:** İmzalanacak AAB/APK'de izinleri ve son bağımlılık ağacını yeniden doğrulayın; sonraki geliştirme değişiklikleri bu incelemenin kapsamı dışındadır.
- [ ] **AdMob / UMP üretim ayarları:** Test entegrasyonundaki onay kontrolü, `canRequestAds` ve yeniden açma akışları kodda vardır; bu inceleme her bölgede formun gerçekten gösterildiğini kanıtlamaz. Canlı Pişsek? reklam kimlikleri, konsoldaki yayımlanmış mesajlar ve uçtan uca hata/ret/yeni tercih senaryoları ayrıca doğrulanmalıdır. Varsayılan üretim reklamları kapalıdır.
- [ ] **Hedef kitle ve çocuklar:** Play Console yaş/hedef kitle ayarları ve mağaza beyanı görülmedi. Kaynaklarda yaş doğrulaması yoktur. Gerçek hedef kitleyi ve gerekiyorsa çocuklara ilişkin reklam ayarlarını doğrulayın; politika onaylanmamış bir yaş sınırı uydurmaz.
- [ ] **Veri sorumlusu kimliği:** İstek sahibi yayıncı adını `darklove`, iletişimi `darkloveai@gmail.com` olarak bildirdi. Hukuken gerekli tam kişi/kurum kimliği, adres, uygulanacak yargı alanları ve varsa ek aydınlatma bilgileri ayrıca yayıncı tarafından doğrulanmalıdır.
- [ ] **Destek yazışmaları:** Posta kutusunun erişim yetkileri, somut saklama/silme takvimi, kullanılan sağlayıcı ve olası uluslararası aktarımlar uygulama kaynaklarından doğrulanamaz; yayıncı süreçlerine göre kesinleştirilmelidir.
- [ ] **TTS sağlayıcısı:** Kullanıcının seçtiği sistem motoru değişebilir. Her motorun sunucu konumu, kayıt ve saklama uygulamaları uygulama kodundan çıkarılamaz. Politika ağ sesi ile cihaz sesini bu nedenle ayırır.
- [ ] **İndirme sunucusu:** Bu sürümde adresi kullanıcı girer. Sunucunun günlükleri ve saklama süreleri bilinmez. Sabit bir yayıncı sunucusu eklenirse sağlayıcı ve veri akışını politikaya ekleyin.

Bu liste web sayfasının yayımlanmasını Android uygulamasının mağazada yayımlanmasıyla eşitlemez. Web adresinin çalışması, Play Console beyanlarının onaylandığı veya Android üretim sürümünün yayımlandığı anlamına gelmez.

## Resmî başvuru kaynakları

25 Ekim 2026 tarihinde kontrol edildi:

- [GMA Next-Gen veri açıklamaları](https://developers.google.com/admob/android/next-gen/privacy/play-data-disclosure)
- [UMP onay ve gizlilik seçenekleri](https://developers.google.com/admob/android/next-gen/privacy)
- [Android TTS sesinin ağ gereksinimi](https://developer.android.com/reference/android/speech/tts/Voice#isNetworkConnectionRequired())
- [Google Play Kullanıcı Verileri politikası](https://support.google.com/googleplay/android-developer/answer/10144311?hl=tr)
- [GitHub Gizlilik Bildirimi](https://docs.github.com/en/site-policy/privacy-policies/github-general-privacy-statement)
