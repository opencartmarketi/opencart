# Doğrulama raporu — 30 Ekim 2026

Hedef: sağlanan OpenCart 3.0.5.1 / Codervia teması. PHP 8.2.35 ve MariaDB 10.11 üzerinde sentetik mağaza kuruldu. Yüklenen SQL'den yalnız şema alındı; müşteri/sipariş/kimlik bilgileri içeri aktarılmadı. MySQL8 utf8mb4_0900_ai_ci kolasyonu yalnız test şemasında MariaDB uyumlu utf8mb4_unicode_ci olarak eşlendi.

Geçen kontroller:

| Kontrol grubu | Sonuç |
|---|---|
| Temel mağaza/ürün/finans: sahiplik, onay, fiyat, XSS, komisyon, iade, mükerrer kayıt | 39 geçti |
| Gerçek OpenCart yükleyicisiyle ZIP yükleme/XML/OCMOD yenileme, yönetim, sepet, partner alanı HTTP | 35 geçti |
| Termin, takip, satın almaya bağlı puan/yorum, gizlilik, mağaza soruları/hız sınırı | 66 geçti |
| Kampanya, satıcı izolasyonu, miktar/eşik/tarih, yüzde vergi ve kargo modelleri | 41 geçti |
| Satıcı iade erişimi ve kişisel veri sınırları | 20 geçti |
| Yönetim yetki, token, CSRF ve ayar doğrulaması | 28 geçti |
| iyzico ve PayTR protokol araçları | 20 çevrimdışı test geçti; canlı çağrı yok |
| Satıcı panelinin gerçek Twig motorunda render/XSS kontrolü | 14 ekran geçti |
| Gerçek model proxy olayları: sipariş/durum ve kampanya sonrası komisyon | 5 kontrol geçti |
| Chromium: ürün ekranı, satıcı girişi, mobil taşma, yakalanmamış JS hatası | 4 kontrol geçti |
| PHP sözdizimi | Tüm paket PHP dosyaları geçti |
| OCMOD ve yükleyici uyumu | 5 tema işareti tekil; gerçek yükleyici ve değişiklik motoru geçti |

İlk testte termin alanının dizi olarak gönderilmesinin varsayılan değere dönüştüğü tespit edildi. Girdi doğrulaması düzeltildi ve sosyal/termin testlerinin 66 kontrolü yeniden geçti. Testlerin başarısız aşaması başarı olarak sayılmadı.

Görseller yapay test hesaplarından alınmıştır. Gerçek ürün resmi eklenmemiş test ürününün galeri alanı boştur; bu durum ürün görseli yüklemesi yerine geçmez.

Sınırlar: Canlı ödeme/transfer/iade, gerçek DNS/TLS, üretim veri tabanı, üçüncü taraf eklenti çakışmaları, taşıyıcı API'leri ve yüksek yük testi yapılmadı. Bu sonuçlar sınırsız hatasızlık veya tüm C2C platformlarına tam özellik eşdeğerliği garantisi değildir. Dahil/harici işlevler KURULUM.md içinde açıkça belirtilmiştir.
