# C2C Marketplace 1.0.0 — kurulum ve kapsam

Bu paket, gönderilen opencartmarketi_yeni_tema_oc_3.0.5.1.zip içindeki OpenCart 3.0.5.1 ve Codervia default teması için hazırlandı. Başka tema/sürümde uyumluluk doğrulanmadı. Kaynak ZIP, config.php, müşteri verisi ve SQL yedeği dağıtıma dahil edilmez.

## Kurulum

1. Dosyaların ve veritabanının yedeğini alın. Önce aynı temayı kullanan test kopyasında kurun. PHP 8.1+ ve MySQL/MariaDB, mysqli, GD, mbstring, fileinfo, ZIP ve DOM gereklidir. İşlevsel test ortamı PHP 8.2 / MariaDB 10.11'dir.
2. Yönetim → Eklentiler → Eklenti Yükleyici: `.ocmod.zip` dosyasını yükleyin.
3. Eklentiler → Modüller → **C2C Marketplace**: Kur, ardından Düzenle. Durumu açın, komisyon yüzdesini ve panel markasını ayarlayın.
4. Eklentiler → Değişiklikler: Yenile. Yönetim ana sayfasındaki geliştirici ayarlarından tema önbelleğini temizleyin. Varsa CDN önbelleğini de temizleyin.
5. Partner alanı boşken satıcı merkezi mağaza alanında `index.php?route=extension/module/c2c_partner` yoluyla açılır. Yönetimden `partner.siteadi.com` veya başka bir panel sunucu adı girilebilir; `https://`, port, dizin girmeyin.
6. Seçilen alt alan için DNS A/CNAME, geçerli TLS sertifikası ve ana mağazayla **aynı OpenCart belge kökünü** kullanan web sunucusu sanal sunucusu gerekir. Modül DNS/SSL oluşturmaz. Alt alan kökü paneli açar. Alt alandaki giriş oturumu ana mağazadan bağımsızdır; mevcut OpenCart müşteri hesabıyla giriş yapılır.
7. Satıcı müşteri hesabıyla başvuru yapar. Yönetici satıcıyı, ardından ürünlerini onaylar. Ürün değişikliği tekrar onay gerektirir.
8. Kampanya toplamı otomatik kaydedilir. Ücretsiz kargo kampanyaları için modül ayarındaki **Mağaza bazlı kargo yöntemi**ni etkinleştirin ve mağaza başına sabit kargo bedelini belirleyin. Müşteri kasada bu yöntemi seçmelidir; diğer taşıyıcı fiyatları değiştirilmez.

## Dahil olan işlevler

- Başvuru, yönetici onayı/askıya alma, mağaza profili ve herkese açık vitrin.
- Bağımsız ve mobil uyumlu satıcı paneli; Türkçe/İngilizce panel ve yönetim arayüzü.
- Basit ürün ekleme/düzenleme, stok, fiyat, kategori, güvenli ana görsel, ürün bazında 0–60 gün hazırlama/termin süresi.
- Satıcıya ait sipariş kalemleri, teslimat adresi, kargo firması/takip numarası; diğer mağazaların sipariş kalemleri gösterilmez.
- Mağaza takibi ve takipçiye özel yüzde indirimi. İndirim takip devam ettiği sürece geçerlidir; tek kullanımlık nakit ödül değildir.
- Tarihli satıcı kampanyaları: 3 al 2 öde, takipçi indirimi, ücretsiz kargo, tutar üzeri ücretsiz kargo. Aynı türden tek etkin kampanya; en avantajlı ürün indirimi seçilir. Kupon/ödül puanı ile birleşmez. 3 al 2 öde aynı satıcının sepetindeki her üç ürün için en ucuz birimi ücretsiz yapar.
- Gerçek takipçi sayısı, tamamlanmış satın almaya bağlı mağaza puanı ve yorum. Her müşteri/mağaza için tek yorum; iade/uyuşmazlıkta incelemeye düşen siparişlerin puanları görünmez. Ürün değerlendirmeleri mevcut OpenCart sistemiyle devam eder.
- Ürün soruları ve mağaza soruları; giriş, hız sınırı ve satıcı cevabı. Yalnızca yanıtlanan sorular herkese açık görünür.
- Sipariş anında sabitlenen komisyon ve kampanya indirimi; tamamlanan siparişe göre hakediş; yönetici kontrollü manuel ödeme kaydı. Kayda para transferi yapılmış gibi anlam yüklenmemelidir.
- OpenCart'ın mevcut iade başvurusunun satıcıya ait liste/detayı; iade kararı ve gerçek geri ödeme yönetici/ödeme kuruluşundadır.
- CSRF, sunucu tarafı sahiplik kontrolü, oturum yenileme, HTML kaçışları ve görselin yeniden kodlanması.

## Finans ve kargo kuralları

Komisyon tabanı ürün satırı tutarından satıcının kampanya indiriminin düşülmüş halidir. Vergi/kargo ayrıca satıcıya dağıtılmaz. OpenCart sipariş kalemi tutarları temel para birimindedir; dövizli siparişler kayıtlı kurla gösterilir. Mağazanın temel para birimini aktif siparişler varken değiştirmeyin.

Yönetici sipariş kalemlerini değiştirirse, kampanya indirimi doğrulanamazsa veya eşleşen iade başvurusu oluşursa ilgili sipariş **incelemeye alınır ve hakedişten çıkarılır**. Karışık satıcılı siparişte güvenli tarafta kalmak için tüm sipariş bekletilir. İnceleme kayıtları otomatik temizlenmez; kısmi iadelerin mali mutabakatı ayrı yapılmalıdır. Gerçek ödeme/iade sonrası manuel ödeme kayıtlarını muhasebe ile doğrulayın.

Kargo bedeli, fiziksel ürün içeren her mağaza için sabit bedeldir. Platform ürünleri ayrı bir grup sayılır. Ücretsiz kargo eşiği ilgili satıcının vergi ve kargo hariç, kampanya öncesi ürün toplamıdır. Bu ücret taşıyıcının gerçek API tarifesi değildir; kargo entegrasyonu ve otomatik etiket basımı dahil değildir. Termin hazırlanma süresidir, taşıyıcının teslimat garantisi değildir.

Modülü kapatmak veya kaldırmak satıcı ürünlerini pasifleştirir; tablolar, ürünler ve mali kayıtlar silinmez. Yeniden açıldığında ürünler yeniden yönetici onayı ister. Dosyaları doğrudan silmeden önce modülü kapatın.

## Ödeme ve kapsam sınırları

Mevcut OpenCart ödeme yöntemleri korunur. iyzico/PayTR için resmî protokole dayanan imza/istek yardımcıları kaynak pakette bulunur; **aktif ödeme yöntemi veya çalışan otomatik bölüştürme entegrasyonu değillerdir**. Canlı pazaryeri sözleşmesi, API yetkileri, alt üye işyeri/KYC, doğrulanmış tahsilat referansları ve aktarım mutabakatı bağlanmalıdır. Ayrıntılar PAYMENTS.md'dedir; anahtarları sohbetten paylaşmayın.

Bu sürüm test edilebilir C2C çekirdeğidir; Trendyol veya Hepsiburada'nın tüm hizmetlerinin eksiksiz eşdeğeri değildir. Satıcı panelinden varyant/çoklu görsel/XML-toplu aktarım, otomatik banka ödemesi, e-fatura/KYC doğrulama, gelişmiş uyuşmazlık merkezi, canlı kargo API'leri ve otomatik kısmi iade mutabakatı bu pakette yoktur. Varyant/özel fiyat/abonelik içeren ürünleri panelde düzenlemek veri kaybını önlemek için reddedilir; yönetici mevcut OpenCart ekranını kullanır. Tek varsayılan mağaza (store_id=0) desteklenir. Müşteriye açık yeni vitrin metinleri Türkçedir.

## Sorun giderme

Satıcı kartı görünmüyorsa modül ve satıcı/ürün onayını, Değişiklikler yenilemesini ve tema önbelleğini kontrol edin. OCMOD aramaları tam olarak gönderilen temanın beş işaretine bağlıdır; güncellenmiş temada yeniden uyarlama gerekebilir. Başka tema eklentileriyle çakışma testi gerçek sunucuda yapılmalıdır.

Kampanya görünmesine rağmen sepette indirim yoksa tarihleri, miktarı, takip durumunu, kampanya toplamı durumunu ve kupon/ödül kullanımını kontrol edin. Kargo indirimi için kasada Mağaza bazlı kargo seçilmelidir.

Yedekten dönmek için dosya/veritabanı yedeğini birlikte geri yükleyin. Kaldırma tabloları silmez. Mali kayıtları körlemesine DROP TABLE ile silmeyin.
