Tablo Oluşturma

CREATE TABLE users (
    id INT PRIMARY KEY IDENTITY(1,1),  -- PostgreSQL: SERIAL PRIMARY KEY
    fullname VARCHAR(100) NOT NULL,
    email VARCHAR(150) NOT NULL UNIQUE
);

Kullanıcı Ekleme

INSERT INTO users (fullname, email) VALUES
('Ahmet Yılmaz', 'ahmet@example.com'),
('Ayşe Kaya', 'ayse@example.com'),
('Mehmet Demir', 'mehmet@example.com');

Listeleme

SELECT * FROM users;

Sonuç
id	fullname	     email
1	Ahmet Yılmaz	ahmet@example.com
2	Ayşe Kaya	    ayse@example.com
3	Mehmet Demir	mehmet@example.com

Güncelleme

UPDATE users
SET email = 'ahmet.yilmaz@newmail.com'
WHERE id = 1;
Sonuç
id	fullname	    email
1	Ahmet Yılmaz	ahmet.yilmaz@newmail.com

Silme

SELECT * FROM users WHERE id = 1;
Sonuç
1	Ahmet Yılmaz	ahmet.yilmaz@newmail.com
2	Ayşe Kaya	ayse@example.com

Ekran Görüntüsü ve Ekran Kaydı

Kredi kartı/bakiye gibi hassas veriler ekran görüntüsü veya kayıt yoluyla galeriye, bulut yedeklerine ya da kötü amaçlı bir uygulamaya sızabilir; bu yüzden engellenmesi gerekir.

Android → FLAG_SECURE: bu flag set edildiğinde ekran görüntüsü siyah çıkar ve recent apps önizlemesi boş görünür.

iOS → UIScreen.capturedDidChangeNotification ekran kaydını tespit edip hassas alanı gizleme/bulanıklaştırma veya userDidTakeScreenshotNotification screenshot anında uyarı gösterme kullanılır — Android'deki gibi engelleyen tek bir flag yoktur, tespit edip tepki verme mantığıyla çalışır.

Overlay Saldırıları

Android'de bir uygulama, "Draw over other apps" (SYSTEM_ALERT_WINDOW) iznini alarak diğer uygulamaların üzerine görünmez veya sahte bir katman çizebilir; kullanıcı gerçek bankacılık uygulamasına dokunduğunu sanırken aslında saldırganın görünmez butonuna tıklamış olur (tapjacking).

Örnek: Kötü amaçlı bir uygulama, bankacılık uygulaması açıldığında algılayıp üzerine bankanın giriş ekranına birebir benzeyen sahte bir "kullanıcı adı/şifre" formu bindirir; kullanıcı bilgilerini girdiğinde veriler gerçek bankaya değil saldırgana gider.

Root / Jailbreak

Root/Jailbreak yapılmış cihazlarda işletim sisteminin uygulamaları birbirinden izole eden sandbox koruması aşılabilir, bu yüzden root yetkisine sahip kötü amaçlı bir uygulama diğer uygulamaların verilerine normalde erişemeyeceği şekilde erişebilir.

Örnek: Saldırgan, root erişimiyle bankacılık uygulamasının SharedPreferences/Keychain dosyalarını veya RAM'deki (memory dump) oturum token'ını ya da şifrelenmemiş kullanıcı verilerini doğrudan okuyabilir.

SQLite ve Şifreleme

Normal bir SQLite veritabanı şifrelenmemiş olduğu için, cihaza fiziksel erişim sağlayan veya root/jailbreak ile dosya sistemine ulaşan bir saldırgan .db dosyasını doğrudan bir metin editörü veya DB Browser gibi araçla açıp kullanıcı adı, şifre, kart bilgisi gibi verileri düz metin olarak okuyabilir.

SQLCipher kullanıldığında veritabanı dosyasının tamamı AES-256 ile şifrelenir; doğru şifreleme anahtarı (passphrase) olmadan dosya açılamaz veya içeriği anlamsız/okunamaz baytlar olarak görünür.


Access Token ve Refresh Token

Access Token, API isteklerinde kimlik doğrulamak için kullanılan, kısa ömürlü (15 dk - 1 saat) bir token'dır; çalınsa bile saldırganın kullanabileceği süre kısıtlıdır.

Refresh Token ise süresi dolan access token'ı yenilemek için kullanılan, uzun ömürlü bir token'dır ve daha güvenli saklanmalıdır (Keystore/Keychain), çünkü ele geçirilirse saldırgan sürekli yeni access token üretebilir.