# Gelişmiş Kuyruk Mimarisi

RabbitMQ teknolojisinin ana fikri, yoğun kaynak gerektiren işleri/görevleri/operasyonları hemen yapmaya koyularak beklemek zorunda kalmaksızın, bu işleri ölçeklendirilebilir bir vaziyette daha sonra yapılacak bir biçimde planlamaktır.
Tabi bu planlamayı gerçekleştirirken kuyruklardan istifade edilmekte ve görevleri temsil edecek olan mesajlar bu kuyruklara atılmakta ve tüketiciler tarafından bu mesajlar elde edilerek görevlerin asenkron bir şekilde işlenmesi sağlanmaktadır.
Tüm bu süreçlerde, kuyrukların bakımının yapılması gerekmektedir. Senaryoya göre mesajların kalıcılığına dair durumlar vs. konfigüre edilmesi gerekmektedir. Ayrıca birden fazla tüketicinin söz konusu olduğu durumlarda nasıl bir davranışın sergileneceği durumları da oldukça önem arz etmektedir.
Gelişmiş Kuyruk Mimarisi tam da bu noktada devreyegirmektedir. Kuyrukların ve mesajların kalıcılığı, mesajların birden fazla tüketiciye karşı dağıtım stratejisi yahut tüketici tarafından işlenmiş bir mesajın kuyruktan silinebilmesi için onay/bildiri sistemi vs. tüm detaylar bu konu altında değerlendirilir.

### Round Robin Dispatching

- RabbitMQ, default olarak tüm consumer'lara mesajları sırasıyla gönderir. Bir poker masasında kartların sırayla dağıtılması gibi düşünülebilir.
<p  align=center>

<img  src="https://raw.githubusercontent.com/hasanyurdakul/RABBITMQ_NOTLARI/main/5/images/roundrobin.png" />

</p>

### Message Acknowledgement

- RabbitMQ, default olarak tüketiciye gönderdiği bir mesajı, mesaj başarılı bir şekilde işlensin veya işlenmesin, hemen kuyruktan silinmesi üzere işaretler.
- Tüketicilerin kuyruktan aldıkları mesajları işlemesi sürecinde herhangi bir kesinti veya problem meydana gelirse, ilgili mesaj işlenemediği için görevini tamamlamamış olacaktır.
- Bu tarz durumlarda, eğer mesaj başarıyla işlendiyse, kuyruktan silinebilmesi için RabbitMQ'nun bilgilendirilmesi gerekmektedir.
- Consumer'dan mesajın başarıyla işlendiğine dair bir dönüş alan RabbitMQ, mesajı silecektir.

### Message Acknowledgement Problemleri Nelerdir?

- Bir mesaj işlenmeden, consumer problem yaşarsa, bu mesajın sağlıklı bir şekilde işlenebilmesi için başka bir consumer tarafından tüketilebilir olmalıdır.
- Aksi taktirde mesaj kuyruktan alındığı an silinirse ve consumerda bir problem meydana gelirse, bu durumda **veri kaybı** ihtimali söz konusu olacaktır. İşte bu tarz durumlar için Message Acknowledgement özelliği şarttır diyebiliriz.
- Eğer ki Message Acknowledgement özelliğini kullanıyorsanız, mesaj işleme başarılı olduktan sonra, mesajın silinmesi için RabbitMQ'yu uyarmayı kesinlikle unutmayınız. Aksi taktirde, mesaj tekrar yayınlanacak ve başka bir tüketici tarafından tekrar tüketilecektir. Böyle bir durumda da **bir mesajın birden çok defa işlenmesi** durumu meydana gelebilir.
- Tabi ayriyeten mesajlar onaylanarak silinmediği taktirde, mesaj kuyrukta kalmaya devam edecektir ve bu durum kuyruğun boyutunun büyümesi ve yavaşlamasıyla sonuçlanıp, performans düşüşüne yol açacaktır.
- Tabi her tüketicinin işlem yoğunluğuna göre RabbitMQ'ya silme onayı bildirimi verme süresi değişiklik gösterecektir.
- RabbitMQ açısından tüketiciden gelecek olan onay bildirimi için bir zaman aşımı süresi söz konusudur. Default olarak 30 dakika olarak belirlenmiştir.
- RabbitMQ tüketiciden gelecek olan onay bildirimini 30 dk boyunca bekler. 30 dk sonunda mesaj tekrardan kuyruğa alınır. Silinme onay bildirimi gelene dek bu süreç devam edecektir.

### Message Acknowledgement Nasıl Yapılandırılır? - BasicAck

- BasicAck, consumerda ayarlanan bir yapıdır.
- RabbitMQ'da onaylama sürecini başlatmak için, consumer uygulamasında "BasicConsume" metodundaki "autoAck" parametresini görseldeki gibi false değerine getirebilirsiniz. Böylece default davranış olan mesajların kuyruktan silinme davranışı değiştirilecek ve silme işlemi için consumer uygulamadan onay beklenecektir.
- Consumer, mesajı başarı ile işlediğine dair onay mesajını, channel.BasicAck() metodu ile gönderir.
- "multiple" parametresi, birden fazla mesaja dair onay bildirisi gönderir. Eğer true değeri verilirse, DeliveryTag değerine sahip olan bu mesajla birlikte, bundan önceki mesajların da işlendiğini onaylar. Aksi taktirde false verilirse, sadece bu mesaj için onay bildiriminde bulunur.

<p align="center">
<img  src="https://raw.githubusercontent.com/hasanyurdakul/RABBITMQ_NOTLARI/main/5/images/autoack.png" />

</p>
	
### BasicNack İle İşlenmeyen Mesajları Geri Gönderme
- Bazen, consumerlarda istemsiz durumların haricinde, kendi kontrollerimiz sonucunda bir mesajı işlememek isteyebiliriz. 
- Böyle durumlarda, channel.BasicNack() metodu ile RabbitMQ'ya bilgi verebilir ve mesajı tekrardan işletebiliriz. 
- Tabi burada "requeue" parametresi oldukça fazla önem arz etmektedir. Bu parametre, bu consumer tarafından işlenemeyen bu mesajın tekrardan kuyruğa eklenip eklenmeyeceğini ifade etmektedir.

<p align="center">
<img  src="https://raw.githubusercontent.com/hasanyurdakul/RABBITMQ_NOTLARI/main/5/images/basicnack.png" />

</p>
	
### BasicCancel İle Bir Kuyruktaki Tüm Mesajların İşlenmesini Reddetme
- BasicCancel metodu ile verilen consumerTag değerine karşılık gelen tüm mesajlar reddedilir ve işlenmez.

<p align="center">
<img  src="https://raw.githubusercontent.com/hasanyurdakul/RABBITMQ_NOTLARI/main/5/images/basiccancel.png" />

</p>

### BasicReject İle Tek Bir Mesajın İşlenmesini Engelleme

- RabbitMQ'da kuyrukta bulunan mesajlardan belirli olanların consumer tarafından işlenmesini istemediğimiz durumlarda BasicReject metodunu kullabiliriz.
<p align="center">
<img  src="https://raw.githubusercontent.com/hasanyurdakul/RABBITMQ_NOTLARI/main/5/images/basicreject.png" />
</p>

# Message Durability

- Consumerlarda yaşanabilecek bir sıkıntı durumunda, mesajların kaybolmayacağının garantisinin nasıl sağlanacağını Message Acknowlegement konusunda öğrenmiş olduk.
- Ancak RabbitMQ sunucusunda yaşabilecek bir problem konusunda ne yapabileceğimizi henüz öğrenmedik.
- RabbitMQ'da normal şartlarda yani default ayarlarda, sunucunun kapanması veya çökmesi gibi bir durumda, tüm kuyruklar ve mesajlar silinecektir.
- Böyle bir durumda mesajların kaybolmaması, yani kalıcı olabilmesi için, ekstradan çalışma gerçekleştirmemiz gerekmektedir.
- Bu çalışma, kuyruk ve mesaj açısından kalıcı işaretlemesi yapmamızı gerektirmektedir.
- İşaretleme, Publisher ve Consumer taraflarının her ikisinde de Queue'lar declare edilirken ve mesajı yayınlarken yapılmalıdır.
- İletileri bu şekilde işaretlemek, olası bir kriz anında, mesajların kaybolmayacağını tam olarak garanti etmez. (Outbox ve Inbox gibi patternler ile veri kaybı en aza indirilmeye çalışılabilir. )
<p align="center">
<img  src="https://raw.githubusercontent.com/hasanyurdakul/RABBITMQ_NOTLARI/main/5/images/messagedurability.png" />
</p>

# Fair Dispatch

- RabbitMQ'da mesajları tüm consumerlara eşit bir şekilde iletebiliriz.
- Bu da kuyrukta bulunan mesajların, mümkün olan en adil şekilde dağıtımını sağlamak için kullanılan bir özelliktir.
- Consumerlara mesajların eşit bir şekilde iletilmesi, sistemdeki performansı düzenli bir hale getirecektir.
- Böylece bir consumerın, diğer bir consumerdan daha fazla yük altında kalması ve mesaj iletilmeyen consumerların kısmi aç kalması önlenmiş olur.

### Mesaj İşleme Konfigürasyonu

- RabbitMQ'da BasicQos metodu ile mesajların işleme hızını ve teslimat sırasını belirleyebiliriz. Böylece Fair Dispatch özelliği konfigüre edilebilmektedir.
- **prefetchSize**: Bir consumer tarafından alınabilecek en büyük mesaj boyutunu byte cinsinden belirler. 0, sonsuz demektir.
- **prefetchCount**: Bir consumer tarafından aynı anda işleme alınabilecek mesaj sayısını belirler.
- **global**: Bu konfigürasyonun, tüm consumerlar için mi yoksa sadece çağrı yapılan consumer için mi geçerli olacağını belirler.
<p align="center">
<img  src="https://raw.githubusercontent.com/hasanyurdakul/RABBITMQ_NOTLARI/main/5/images/basicqos.png" />
</p>
