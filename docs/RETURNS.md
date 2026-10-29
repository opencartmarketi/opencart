# İade talepleri

Müşteri, mevcut OpenCart hesabındaki ürün iade formunu kullanır: index.php?route=account/return/add. Modül ikinci bir iade başvuru akışı oluşturmaz.

Satıcı panelindeki İade Talepleri bölümü, OpenCart’ın kendi iade kayıtlarını salt okunur biçimde gösterir. Talep listesi ve ayrıntısında ürün, miktar, neden, açıklama, açılmış paket bilgisi ve yöneticinin belirlediği durum/işlem adları bulunur.

Görünürlük, yalnızca onaylı satıcının gerçek satışına bağlı kayıtlarla sınırlıdır. Talepteki müşteri kimliği sipariş sahibiyle eşleşmeli; ürün sipariş kaleminde ve değiştirilemeyen C2C satıcı/sipariş kaydında bulunmalıdır. Başvuruya yazılan sipariş veya ürün numarası tek başına yetki sağlamaz. Satıcıya e-posta, telefon, ödeme adresi veya müşterinin tam adı verilmez. Başvuru açıklaması müşterinin yazdığı metindir; panel HTML olarak çalıştırmaz.

OpenCart’ın misafir iade formunda müşteri kimliği sıfırdır; bu değer gerçek sahipliği kanıtlamaz. Misafir ve geçmiş C2C kaydı bulunmayan talepler bu satıcı görünümünde gösterilmez; yönetici bunları OpenCart panelindeki Satışlar > Ürün İadeleri bölümünden ele alır. Aynı şekilde kapalı/askıya alınmış satıcılar bu verilere erişemez.

Satıcıya iade onaylama, sipariş durumunu değiştirme veya para iadesi yapma yetkisi verilmez. Yönetici, OpenCart iade ve ödeme kuruluşu araçlarıyla inceleme/geri ödeme işlemini yürütür. Bir iade durumunun adı veya sayısal kimliği kendi başına paranın geri ödendiği anlamına gelmez. Kısmi iadelerin hakedişe etkisi mali uzlaştırma işleminde değerlendirilmelidir; iade talebi açılması otomatik banka hareketi oluşturmaz.
