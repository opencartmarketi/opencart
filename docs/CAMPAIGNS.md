# Mağaza kampanyaları

Satıcı panelindeki Kampanyalar ekranından başlangıç/bitiş tarihleri dahil geçerli kampanya kaydedilir. Yalnızca onaylı mağazanın onaylı ve yayındaki ürünleri kampanyaya girer. Aynı mağazada aynı türde bir kampanya etkinleştirildiğinde önceki aynı tür devre dışı kalır.

- **3 al 2 öde:** aynı mağazanın uygun ürünlerinden her üç adet için bir adet ücretsizdir. Sepetteki bu mağazanın en ucuz birimleri indirilir; farklı mağazaların adetleri birleşmez.
- **Takipçiye özel indirim:** hesabıyla giriş yapmış, mağazayı halen takip eden müşteriye %0,01–%50 indirim. Takibi bırakınca avantaj sona erer; yeniden takip ayrı nakit/kupon oluşturmaz.
- Bu iki fiyat kampanyası aynı mağazada birlikte toplanmaz; müşteriye daha yüksek indirimi veren uygulanır. Yerel OpenCart kuponu, ödül puanı veya önceki negatif sepet indirimi varken C2C fiyat indirimi uygulanmaz.
- **Ücretsiz kargo:** yalnızca bu mağazanın kargo payını sıfırlar.
- **Belirli tutar üzeri ücretsiz kargo:** bu mağazanın vergi ve kargo hariç, kampanya indirimi öncesi ürün toplamı belirtilen tutara eşit veya büyükse kargo payı sıfırlanır.

Kargo kampanyaları, yönetimden etkinleştirilen **Mağaza bazlı kargo** yöntemi seçildiğinde geçerlidir. Bu yöntem mağaza başına yapılandırılmış sabit bir taban ücret toplar. Sepette platformun kendi ürünleri varsa bunlar tek ek kargo grubu sayılır. Ücretsiz kargo kampanyası diğer mağazanın veya platform ürünlerinin kargo payını kaldırmaz. Kargo gerektirmeyen dijital ürünler ücret oluşturmaz. Başka kargo yöntemleri otomatik değiştirilmez; taşıyıcı API/fiyat anlaşması veya etiket kesme entegrasyonu içermez.

Tüm hesaplar OpenCart mağaza ana para birimindedir. Döviz gösterimini OpenCart yapar. Oransal KDV, indirimli tutara göre azaltılır; sabit adet vergileri korunur. Hakediş altyapısı C2C indiriminin satıcıya ait payını ayrıca doğrulamalı ve sipariş anında değişmez olarak kaydetmelidir; doğrulanamayan siparişler mali incelemeye alınmalıdır.

Dosyalar: `system/library/c2c/campaigns.php`, `catalog/model/extension/total/c2c_marketplace.php`, `catalog/model/extension/shipping/c2c_marketplace.php`. Fiyat indirimi uzantısı durum/sıralama ve kargo uzantısı durum/taban ücret/sıralama ayarlarını merkezi C2C modülü yönetir.
