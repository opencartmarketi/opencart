# Ödeme ve pazaryeri aktarım durumu

Bu OCMOD, mağazanın mevcut OpenCart ödeme yöntemlerini kullanır. Siparişteki satıcı kalemlerini ve komisyonu kaydeder; yönetici, gerçekleşmiş banka ödemesini hakediş defterine manuel olarak işler. Deftere ödeme kaydetmek bankaya para göndermez.

İsteğiniz üzerine iki kuruluş için doğrulanmış protokol adaptörleri de pakete eklendi. Bunlar **etkin ödeme yöntemi veya tamamlanmış canlı otomatik para bölüştürme entegrasyonu değildir**. Yönetim ekranında çalışmayan bir “otomatik ödeme” seçeneği gösterilmez; mevcut ödeme akışı sessizce başka bir sağlayıcıya yönlendirilmez.

| Sağlayıcı | Pakette bulunan, çevrimdışı test edilen parça | Çalışması için kalan bağımlılık |
| --- | --- | --- |
| iyzico Pazaryeri | IYZWSv2 imzalı kalem onay/geri alma istekleri ve ödeme sepeti kalemine subMerchantKey/subMerchantPrice ekleme | Pazaryeri sözleşmesi ve API bilgileri; satıcıların doğrulanmış alt üye işyeri kayıtları; mevcut ödeme eklentisinin kimliği doğrulanmış paymentTransactionId alanlarını satıcı kalemine bağlaması |
| PayTR Platform/Pazaryeri | Platform transfer isteği imzası, kuruş tutarı/IBAN doğrulaması; ödeme bildiriminin sabit zamanlı HMAC kontrolü | Platform aktarım yetkisi ve API bilgileri; doğrulanmış tahsilat merchant_oid kaydı; banka hesaplarının doğrulanması; transfer durumunu uzlaştıran kalıcı iş akışı |

API anahtarlarını veya müşteri banka bilgilerini sohbetten göndermeyin. Bu sürüm bu bilgileri toplamaz veya kaydetmez. Canlı entegrasyonda sunucu tarafından güvenli biçimde saklanmaları ve yalnızca sağlayıcının HTTPS uçlarına gönderilmeleri gerekir.

## Geliştirici arabirimi

system/library/c2c/payment/iyzico.php içindeki Iyzico sınıfı:

- Yapıcı: apiKey, secretKey, sandbox=true; varsayılan ortam sandbox.
- buildApproval(paymentTransactionId, conversationId, nonce=null) ve buildDisapproval(...): method, sabit sağlayıcı url, headers, JSON body döndürür. Ağ isteği göndermez. Dönen başlıklar hassastır; loglanmaz.
- assignSubMerchant(basketItem, subMerchantKey, subMerchantPrice): mevcut iyzico sepet kaleminin alt üye alanlarını ekler. Fiyatlar "125.50" biçiminde, en çok iki ondalık basamaklı metin olmalıdır. Satıcı tutarı kalem tutarını aşamaz.

system/library/c2c/payment/paytr.php içindeki Paytr sınıfı:

- Yapıcı: merchantId, merchantKey, merchantSalt.
- buildTransfer(merchantOrderId, transactionId, submerchantAmount, totalAmount, accountName, iban): sabit platform transfer adresini ve imzalı form alanlarını döndürür; ağ isteği göndermez. Tutarlar **kuruş** olarak tam sayı metnidir: "10040" = 100,40 TL. Gerçek tahsilatın referansı, aktarım yetkisi ve kullanılabilir tutar çağıran entegrasyon tarafından doğrulanmalıdır. trans_id her ödeme için kalıcı ve benzersiz tutulmalı; belirsiz ağ sonucunda yeni referansla kör tekrar yapılmamalıdır.
- verifyPaymentNotification(notification): ödeme bildiriminin imzasını kontrol eder. Tek başına siparişi ödenmiş saymaz. Doğru sipariş, beklenen tutar/para birimi, daha önce işlenme durumu ve sağlayıcı sonucu ayrıca doğrulanmalıdır. Bu, platform transfer bildirim doğrulayıcısı değildir.

Otomatik entegrasyon eklenirken doğrulanmış tahsilat/alt üye/transfer referansları için ayrı kalıcı tablolar, tekrar güvenliği, başarısız/belirsiz transfer uzlaştırması, iade/iptal senaryoları ve API hata testleri gerekir. Provider onayı veya transfer talebi cevabı doğrudan “bankaya ödendi” kaydı sayılmamalıdır. Mevcut manuel hakediş kaydı otomatik sağlayıcı sonuçlarıyla karıştırılmamalıdır.

## İncelenen resmî kaynaklar

30 Ekim 2026 tarihinde incelendi:

- iyzico resmî SDK, master SHA ba54104c9e08b360ee265173c6d707154a24eba4: https://github.com/iyzico/iyzipay-php
- IyziAuthV2Generator, IyzipayResource, Model/Approval, Model/Disapproval, Request/CreateApprovalRequest, Model/BasketItem sınıfları.
- PayTR resmî Postman koleksiyonu SHA 6b9a1abf0ddf309cef2151381543c4c62e8efc9f: https://github.com/paytr/paytr-postman — “Transfer Talimatı Ver” isteği; https://www.paytr.com/odeme/platform/transfer.
- PayTR resmî Magento ödeme modülü, SHA 9b60b5ebf87406c0fe265a12b4c27c754f9ac365: https://github.com/paytr/magento2-payment — Model/Api/Webhook.php ödeme bildirimi HMAC formülü.

dev.paytr.com belge isteği mevcut ortamın ağ ilkesi tarafından CONNECT 403 ile engellendi. Bu engel aşılmadı; sözleşme resmî GitHub kaynaklarından doğrulandı. Canlı API uçlarına hiçbir istek gönderilmedi. Ek test için ağ erişimi ve sağlayıcı sandbox hesapları ayrıca gereklidir.

## Doğrulama

PHP 8.2 üzerinde tests/payment_adapters.php içindeki 20 çevrimdışı kontrol geçti: resmî istek gövdeleri ve imza formülleri, sandbox varsayılanı, fiyat/alt üye tutarı sınırı, imza tahrifi, kuruş biçimi, IBAN sağlama toplamı ve eksik API bilgisi reddi. Bunlar sandbox veya canlı sağlayıcı kabul testi yerine geçmez.
