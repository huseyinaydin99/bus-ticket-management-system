## Otobüs Bileti Yönetim Sistemi - Proje Özeti 🚀
Bu projede, Java 1.8, JavaFX, JDBC, SQL, MySQL, Jasper Report ve FontAwesome kullanarak tam kapsamlı bir otobüs bileti satış sistemi geliştirdim. 🚍🎟️ Malesef kaynak kodu paylaşamayacağım anlayışınız için teşekkürler.

### Veritabanı Tasarımı 📊
Projemin temelinde veritabanı yapısı oldukça güçlü ve ilişkisel veri modeline dayanıyor. İlk başta veritabanımda oluşturduğum temel tablolar şu şekilde:

##### admin Tablosu:

Bu tablonun amacı, yöneticinin sisteme giriş yapabilmesi. İçinde id, username ve password alanları yer alıyor. Kullanıcı adı benzersiz olmalı ve şifre güvenli bir şekilde saklanmalı. Sistemde yöneticinin kim olduğunu belirlemek için bu verileri kullandım. 💼🔐

```sql
CREATE TABLE admin (
    id INT AUTO_INCREMENT PRIMARY KEY,
    username VARCHAR(50) NOT NULL UNIQUE,
    password VARCHAR(100) NOT NULL
);
```

Admin kullanıcı adı ve şifre ile giriş yapılıyor. Bu sayede, kullanıcı sadece doğru giriş bilgileriyle sisteme girebilir.
##### bus Tablosu:

Otobüs bilgilerini tutan bu tablo, her otobüs için bus_id, location, status, price gibi verileri barındırıyor. Her otobüsün lokasyonu, fiyatı ve sefer tarihi burada saklanıyor. 📍💸🚌

```sql
CREATE TABLE bus (
    id INT AUTO_INCREMENT PRIMARY KEY,
    bus_id INT NOT NULL UNIQUE,
    location VARCHAR(100) NOT NULL,
    status VARCHAR(50) NOT NULL,
    price DECIMAL(10, 2) NOT NULL,
    date DATE NOT NULL
);
```
##### customer Tablosu:

Müşteri bilgilerini burada saklıyorum. Her müşteri için customer_id, firstName, lastName, phoneNumber gibi temel bilgileri içeriyor. Ayrıca, her müşteri bir otobüse bindiği için bus_id ile ilişkilendiriliyor. 🧑‍🦱📱🚍
```sql
CREATE TABLE customer (
    id INT AUTO_INCREMENT PRIMARY KEY,
    customer_id INT NOT NULL,
    firstName VARCHAR(100) NOT NULL,
    lastName VARCHAR(100) NOT NULL,
    gender VARCHAR(50) NOT NULL,
    phoneNumber VARCHAR(20) NOT NULL,
    bus_id INT NOT NULL,
    location VARCHAR(100) NOT NULL,
    type VARCHAR(50) NOT NULL,
    seatNum INT NOT NULL,
    total DECIMAL(10, 2) NOT NULL,
    date DATE NOT NULL,
    FOREIGN KEY (bus_id) REFERENCES bus(id)
);
```

##### customer_receipt Tablosu:

Müşteri ödemelerini takip edebilmek için bu tabloyu kullandım. Her müşteri için ödeme tutarı ve ödeme tarihi burada saklanıyor. 💳💰
```sql
CREATE TABLE customer_receipt (
    id INT AUTO_INCREMENT PRIMARY KEY,
    customer_id INT NOT NULL,
    total DECIMAL(10, 2) NOT NULL,
    date DATE NOT NULL,
    FOREIGN KEY (customer_id) REFERENCES customer(id)
);
```

Veritabanı yapısının temeli işte böyle kurulmuş. Kullanıcılar, yönetici paneli üzerinden bu verilerle etkileşime geçebilecek ve sistemin her aşamasını kontrol edebilecek.

### Java ve JavaFX ile Kullanıcı Arayüzü ve Uygulama Mantığı 🖥️
Projemin önemli bir parçası Java ve JavaFX ile yapılan kullanıcı arayüzüydü. JavaFX'in sağladığı güçlü araçlarla modern ve dinamik bir GUI tasarladım. 🎨💻

### Kullanıcı Girişi:
Kullanıcı adı ve şifre ile giriş işlemi yaparak, yöneticinin sisteme erişimini sağladım. JavaFX’in TextField ve PasswordField bileşenlerini kullanarak kullanıcıdan bu bilgileri aldım ve JDBC aracılığıyla MySQL veritabanında doğrulama yaptım. Eğer giriş başarılıysa yönetici sisteme yönlendirilir. 🔑🔒

```java
public boolean validateLogin(String username, String password) {
    Connection conn = DriverManager.getConnection(dbURL, dbUsername, dbPassword);
    String query = "SELECT * FROM admin WHERE username = ? AND password = ?";
    PreparedStatement pstmt = conn.prepareStatement(query);
    pstmt.setString(1, username);
    pstmt.setString(2, password);
    ResultSet rs = pstmt.executeQuery();
    return rs.next();
}
```

### Otobüs ve Müşteri Yönetimi:
Otobüslerin listelendiği bir arayüz oluşturup, ComboBox ve TableView kullanarak otobüs bilgilerini kullanıcıya sundum. Otobüs ekleme, güncelleme ve silme işlemleri için JavaFX butonları ve JDBC ile veri tabanına kayıt işlemleri sağlandı.
Aynı şekilde müşteri bilgilerini de görüntüleyebilmek ve yeni müşteri eklemek için benzer bir yaklaşım kullanarak JavaFX bileşenlerini ve veritabanı bağlantısını entegre ettim.
### Raporlama - Jasper Reports 📄📊:
Otobüslerin satış raporlarını alabilmek ve bunları güzel bir şekilde sunabilmek için Jasper Reports kullandım. Böylece yöneticiler, sistemdeki verileri görsel olarak inceleyebilir. 🧐📈
Jasper Reports sayesinde sistemdeki müşteri işlemlerini PDF olarak raporlayabiliyorum. Bu raporlar, her müşteri için yaptığı ödeme, otobüs bilgileri ve bilet durumu gibi kritik verileri içeriyor.
### Veritabanına Veri Ekleme ve İlişkiler 🔄
Projede ayrıca INSERT komutlarıyla bazı başlangıç verileri ekledim. Örneğin, admin tablosuna bir kullanıcı (huseyin) ekledim ve aynı şekilde bus ve customer tablosuna örnek otobüsler ve müşteri bilgileri de girdim. Bu sayede projeyi test ederken kolayca veri görebildim.

```sql
INSERT INTO admin (username, password)
VALUES ('huseyin', 'huseyin');
```

Projemi başlatırken, öncelikle JavaFX ile güzel bir kullanıcı arayüzü tasarımı yapmak istedim. Hem şık hem de kullanımı kolay bir görünüm elde etmek için gerekli CSS kodlarını yazdım. Bu CSS'lerde renkler, fontlar ve buton tasarımları gibi küçük ama etkili detaylara dikkat ettim. Mesela, gradient background kullanarak arka planda #753036 ve #3c8256 renklerini geçişli bir şekilde kullandım. Hem left-form hem de right-form div'leri için görsel uyumu sağlayacak şekilde arka plan ve border renklerini belirledim. Bu şekilde uygulama hem modern hem de şık görünüyor. 👌

![Ekran görüntüsü 2025-02-02 140457](https://github.com/user-attachments/assets/abcd7ccf-b65c-4a55-a08a-29a07a6de4a7)
![Ekran görüntüsü 2025-02-02 140524](https://github.com/user-attachments/assets/5a7e439f-1739-4de9-bbb4-d448fc042185)

Ayrıca, formdaki her bir öğe için etkileşimli CSS kodları yazdım. Örneğin, login button'ın üzerine geldiğinde renk değiştirmesi için CSS hover efekti ekledim. Tıklanabilir alanları belirgin ve kullanıcı dostu yapmayı amaçladım. Bu sayede hem görsel olarak güzel hem de işlevsel bir arayüz elde ettim. Butonun üzerine gelindiğinde renginin değişmesi ve kullanıcıya hoş bir geri bildirim vermesi gerçekten önemli! 💻✨

![Ekran görüntüsü 2025-02-02 140545](https://github.com/user-attachments/assets/1ed45471-ad52-4f33-bcef-87c5d06d8e9c)
![Ekran görüntüsü 2025-02-02 140612](https://github.com/user-attachments/assets/3e01adbc-f69d-4e73-9c6e-6fc32e712010)

Tabii ki her şey görselle bitmiyor, işin veri tarafına da girmek gerekiyor. Bu noktada, JasperReports ile bir raporlama sistemi kurmaya karar verdim. Bu raporlama aracı sayesinde veritabanımdan aldığım verileri şık bir formatta ekrana yazdırabiliyorum. SQL sorgusuyla customer ve customer_receipt tablolarından aldığım verileri, doğru şekilde ilişkilendirip tek bir rapor halinde sunuyorum. 🧑‍💻💡

![Ekran görüntüsü 2025-02-02 141005](https://github.com/user-attachments/assets/5969534a-fd04-433f-b8e3-67a4ec5d7b22)
![Ekran görüntüsü 2025-02-02 140934](https://github.com/user-attachments/assets/028c8cfa-2f94-4910-bc2a-7e18450fd95d)
![Ekran görüntüsü 2025-02-02 140934](https://github.com/user-attachments/assets/0ca9e44e-cdf8-4bcd-8cdf-eb73e0cf144d)

JasperReport'a girdiğimizde, raporda hangi bilgilerin yer alacağını belirlemek için gerekli olan parametreleri tanımladım. Örneğin, busD parametresini kullanarak müşteri ID'sine göre verileri filtrelemeyi sağladım. Bu da kullanıcının hangi müşteri bilgilerini görmek istediğini seçebilmesini kolaylaştırdı. 🎯
![Ekran görüntüsü 2025-02-02 141813](https://github.com/user-attachments/assets/dba6b8b8-c037-4090-8c24-08e69b989955)

Rapordaki başlıkları, kolonları ve metinleri özelleştirmek için her bir öğeyi dikkatlice yerleştirdim. Hangi verinin hangi alanda görüneceğini belirledim. Raporu çok daha anlaşılır kılmak için, başlıklara ve her kolonun altına açıklayıcı metinler ekledim. Böylece hem kullanıcılar hem de veritabanı yöneticileri için çok daha anlaşılır bir rapor ortaya çıktı. 📰📊

![Ekran görüntüsü 2025-02-02 141906](https://github.com/user-attachments/assets/3ccf202b-0db7-4875-94ac-58a9e401e9d1)

Detaylara inmeye geldiğimizde, JasperReport'ta kullanıcının aldığı biletlerin bilgilerini görsel olarak sergileyebilmek için gerekli alanları oluşturdum. Örneğin, otobüs ID'si, bilet numarası, lokasyon ve tip gibi bilgileri columnHeader kısmında tanımladım. Bu sayede raporun her bir satırında önemli bilgiler kolayca görüntülenebiliyor. ✍️📍

![Ekran görüntüsü 2025-02-02 141937](https://github.com/user-attachments/assets/e4292f0a-ec25-444a-b099-dcc99f17ca3a)

Tabii ki tüm bu veriler dinamik. Yani kullanıcılar farklı verileri görmek istediğinde sadece parametreyi değiştirerek raporlarını yenileyebiliyorlar. Böylece hem hız hem de esneklik sağlanmış oldu. 🚀

Son olarak, her şeyin uyumlu çalışması ve verilerin doğru bir şekilde ekrana yansıması için her bir alanı test ettim. Hem görsel hem de veri tarafındaki tüm işlemleri doğru şekilde bağlantılı hale getirip, projemi sonlandırdım. 💯

![Ekran görüntüsü 2025-02-02 142052](https://github.com/user-attachments/assets/0377babc-93fd-410b-acc9-26247dd12d93)
![Ekran görüntüsü 2025-02-02 140545](https://github.com/user-attachments/assets/334a8451-5d03-428b-bcd4-1f71cdc54947)
![Ekran görüntüsü 2025-02-02 140612](https://github.com/user-attachments/assets/01da569c-191d-4fcc-8287-40aceab5eba4)

### Sonuç ve Sonraki Adımlar 🚀
Bu projede, JavaFX ile güçlü bir kullanıcı arayüzü, Java ve JDBC ile sağlam bir veritabanı entegrasyonu gerçekleştirdim. 💻🔗 Veritabanı yapısı ilişkisel olup, her şey birbirine bağlanmış durumda. Yöneticiler sisteme giriş yaparak otobüs seferlerini, müşteri bilgilerini ve ödeme kayıtlarını rahatlıkla yönetebilecek.

Gelecekte, bu projeye ek özellikler ve daha fazla raporlama seçenekleri eklemeyi planlıyorum. Ayrıca, web tabanlı bir versiyonunu da geliştirmeyi düşünüyorum. 🌐✨
