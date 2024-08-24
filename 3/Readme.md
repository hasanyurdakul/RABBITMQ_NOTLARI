# Exchange Tipleri Nelerdir?

### Direct Exchange

- Aksi belirtilmediği sürece default olarak oluşturulan exchange tipidir.
- Mesajların direkt olarak belirli bir kuyruğa gönderilmesini sağlayan exchange tipidir.
- Mesaj, routing key'e uygun olan hedef kuyruklara gönderilir. Bunun için, mesaj gönderilecek kuyruğun adını routing key olarak belirtmek yeterlidir.
- Genellikle hata mesajlarının işlendiği senaryolarda kullanılabilir. Şöyle ki; bir sistemde "dosya yükleme hatası", "veritabanı bağlantı hatası" vs. gibi farklı türde/çeşitte hata mesajları olabilir. Bu hata mesajlarının izlenmesi ve gerektiği taktirde çözülmesi gerekebilir. Haliyle her bir hata türü için hususi ayrı bir kuyruk oluşturulabilir ve hata mesajları direkt olarak ilgili kuyruğa gönderilerek işlenebilir. Böylece hata mesajlarının izlenmesi ve çözülmesi için sadece kuyrukların takip edilmesi yeterli olacaktır.
- Başka bir örnek vermek gerekirse; e-ticaret sistemlerindeki sipariş süreçlerini düşünebiliriz. Biliyorsunuz ki, bir sipariş "Onaylandı", "İptal Edildi", veya "İade Edildi" şeklinde üç farklı durumda gerçekleşmektedir. Bir sipariş sürecinde farklı durumlara nazzaran davranışlar geliştirmek ve yönetim sergileyebilmek için her bir durum için ayrı kuyruklar oluşturulabilir ve siparişler direkt olarak ilgili kuyruklara gönderilerek işlenebilir. Bu şekilde de her bir durum için ayrı aksiyon alabilmek ve siparişleri işlemek daha kolay hale gelecektir.

<p align=center>
<img src="https://raw.githubusercontent.com/hasanyurdakul/RABBITMQ_NOTLARI/main/3/images/direct.png" />
</p>

### Fanout Exchange

- Mesajların, bu exchange'e bind olmuş olan tüm kuyruklara gönderilmesini sağlar. Publisher, mesajların gönderildiği kuyruk isimlerini dikkate almaz ve mesajı tüm kuyruklara gönderir.
- Misal olarak, bir microservice yaklasımı benimsemiş olan mimaride, tüm servislere ortak bildirilerde bulunabilmek için "Fanout Exchange" kullanılabilir. Böylece veri paylaşımı merkezi bir şekilde hızlı ve etkili hale getirilebilir.
<p align=center>
<img src="https://raw.githubusercontent.com/hasanyurdakul/RABBITMQ_NOTLARI/main/3/images/fanout.png" />
</p>

### Topic Exchange

- Routing keyleri kullanarak mesajları kuyruklara yönlendirmek için kullanılan bir exchange tipidir. Bu exchange ile routing key'in bir kısmına/formatına/yapısına /yapısındaki keylere göre kuyruklara mesaj gönderilir.
- Kuyruklara da, routing key'e göre bu exchange'e abone olabilir ve sadece ilgili routing key'e göre gönderilen mesajları alabilirler.
- Örnek olarak log sistemi senaryoları bu exchange tipi için biçilmiş bir kaftan olacaktır. Kuyruklar, log seviyelerine göre abone olabilir ve sadece routing key'ine uygun ilgili log seviyesine ait mesajları alabilirler. Böylece sistem yöneticileri ya da ilgili servisler; belirli bir kategoriye veya key'e göre logları filtreleyebilir ve böylece istedikleri hata yahut uyarı mesajlarına odaklanabilir, izleyebilir ve istedikleri log mesajlarını gözardı edebilirler.

<p align=center>
<img src="https://raw.githubusercontent.com/hasanyurdakul/RABBITMQ_NOTLARI/main/3/images/topic1.png" />
</p>
<p align=center>
<img src="https://raw.githubusercontent.com/hasanyurdakul/RABBITMQ_NOTLARI/main/3/images/topic2.png" />
</p>

### Header Exchange

- Routing key yerine, header'ları kullanarak mesajları kuyruklara yönlendirmek için kullanılan exchangedir.
- Topic exhange yerine kullanılabilir. Fark, routing key yerine header kullanılmasıdır.
 <p align=center>
<img src="https://raw.githubusercontent.com/hasanyurdakul/RABBITMQ_NOTLARI/main/3/images/header.png" />
</p>
