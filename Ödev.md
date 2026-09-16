Görev 1

BAŞLA

Uygulamayı aç

Oturum durumunu kontrol et

EĞER kullanıcı giriş yapmış İSE
    Ana sayfayı göster
DEĞİLSE
    Giriş ekranına yönlendir
    Kullanıcı giriş bilgilerini girsin

    EĞER giriş başarılı İSE
        Ana sayfayı göster
    DEĞİLSE
        "Giriş başarısız" mesajı göster
        BİTİR
    BİTİR
BİTİR

Sepeti oluştur

DÖNGÜ kullanıcı ürün seçmeye devam ettiği sürece
    Ürün listesini göster
    Kullanıcıdan ürün seçmesini iste
    Seçilen ürünü sepete ekle
    Sepet toplam tutarını güncelle
DÖNGÜ SONU

Sepet içeriğini ve toplam tutarı göster

Kullanıcıdan siparişi onaylamasını iste

EĞER kullanıcı siparişi onayladı İSE

    Kullanıcının cüzdan bakiyesini kontrol et

    EĞER cüzdan bakiyesi >= sepet toplam tutarı İSE

        Sipariş paketini oluştur
        Sipariş paketini sunucuya gönder

        EĞER sipariş sunucuya başarıyla gönderildi İSE
            Sepet toplam tutarını kullanıcının bakiyesinden düş
            Siparişi kaydet
            Sepeti temizle
            "Siparişiniz başarıyla alındı" mesajını göster
        DEĞİLSE
            "Sipariş gönderilirken hata oluştu" mesajını göster
        BİTİR

    DEĞİLSE
        "Yetersiz bakiye - Bakiye Yükle" uyarısını göster
        Kullanıcıyı bakiye yükleme ekranına yönlendir
    BİTİR

DEĞİLSE
    "Sipariş iptal edildi" mesajını göster
BİTİR

BİTİR
Görev 2

Sipariş Oluşturma Endpoint’i
HTTP Metodu: POST
Endpoint: /api/v1/siparisler
Header:
Authorization: Bearer <access_token>
Content-Type: application/json
Request Body:
{
  "kahve_adi": "Caffe Latte",
  "boyut": "buyuk",
  "adet": 2,
  "toplam_tutar": 240.00
}
Sipariş başarılı bir şekilde oluşturulursa:
HTTP Durum Kodu:
201 Created
Response Body:
{
  "basarili": true,
  "mesaj": "Sipariş başarıyla oluşturuldu.",
  "siparis_id": 1057
}
Kullanıcı giriş yapmamışsa veya geçersiz bir token kullanıyorsa:
HTTP Durum Kodu:
401 Unauthorized
Response Body:
{
  "basarili": false,
  "mesaj": "Bu işlemi gerçekleştirmek için giriş yapmalısınız."
}
Bu endpoint'te POST metodu kullanılmasının nedeni sunucuda yeni bir sipariş kaydı oluşturulmasıdır. Authorization header'ı kullanıcının kimliğini doğrulamak için, Content-Type: application/json ise gönderilen verinin JSON formatında olduğunu belirtmek için kullanılır.
Görev 3

•  Bu sınıf; sepet hesaplama, ödeme alma, veritabanına kayıt ve SMS gönderme gibi birden fazla sorumluluğu aynı yerde topladığı için Single Responsibility Principle (SRP) ilkesini ihlal eder. Sınıf; örneğin SepetHesaplamaServisi, OdemeServisi, SiparisRepository ve BildirimServisi gibi ayrı parçalara bölünmelidir.
•  Yeni bir müşteri tipi eklendiğinde if-else yapısını değiştirmek zorunda kalmak Open/Closed Principle (OCP - Açık/Kapalı Prensibi) ilkesine aykırıdır. Kod, yeni müşteri tiplerine genişletmeye açık, mevcut kodu değiştirmeye ise kapalı olacak şekilde tasarlanmalıdır.

