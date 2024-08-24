# Gelişmiş Kuyruk Mimarisi

RabbitMQ teknolojisinin ana fikri, yoğun kaynak gerektiren işleri/görevleri/operasyonları hemen yapmaya koyularak beklemek zorunda kalmaksızın, bu işleri ölçeklendirilebilir bir vaziyette daha sonra yapılacak bir biçimde planlamaktır.
Tabi bu planlamayı gerçekleştirirken kuyruklardan istifade edilmekte ve görevleri temsil edecek olan mesajlar bu kuyruklara atılmakta ve tüketiciler tarafından bu mesajlar elde edilerek görevlerin asenkron bir şekilde işlenmesi sağlanmaktadır.
Tüm bu süreçlerde, kuyrukların bakımının yapılması gerekmektedir. Senaryoya göre mesajların kalıcılığına dair durumlar vs. konfigüre edilmesi gerekmektedir. Ayrıca birden fazla tüketicinin söz konusu olduğu durumlarda nasıl bir davranışın sergileneceği durumları da oldukça önem arz etmektedir.
Gelişmiş Kuyruk Mimarisi tam da bu noktada devreyegirmektedir. Kuyrukların ve mesajların kalıcılığı, mesajların birden fazla tüketiciye karşı dağıtım stratejisi yahut tüketici tarafından işlenmiş bir mesajın kuyruktan silinebilmesi için onay/bildiri sistemi vs. tüm detaylar bu konu altında değerlendirilir.

### Round Robin Dispatching

- RabbitMQ, default olarak tüm consumer'lara mesajları sırasıyla gönderir. Bir poker masasında kartların sırayla dağıtılması gibi düşünülebilir.
<p  align=center>

<img  src="https://raw.githubusercontent.com/hasanyurdakul/RABBITMQ_NOTLARI/main/5/images/roundrobin.png"  />

</p>

### Message Acknowledgement

- RabbitMQ, default olarak tüketiciye gönderdiği bir mesajı, mesaj başarılı bir şekilde işlensin veya işlenmesin, hemen kuyruktan silinmesi üzere işaretler.
- Tüketicilerin kuyruktan aldıkları mesajları işlemesi sürecinde herhangi bir kesinti veya problem meydana gelirse, ilgili mesaj işlenemediği için görevini tamamlamamış olacaktır.
- Bu tarz durumlarda, eğer mesaj başarıyla işlendiyse, kuyruktan silinebilmesi için RabbitMQ'nun bilgilendirilmesi gerekmektedir.
- Consumer'dan mesajın başarıyla işlendiğine dair bir dönüş alan RabbitMQ, mesajı silecektir.
