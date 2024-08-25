# Enterprise Service Bus (ESB) Nedir?

- ESB, servisler arasındaki entegrasyonu sağlayan komponentlerin bir bütünüdür diyebiliriz.
- Yani, farklı yazılım sistemlerinin birbirleriyle iletişim kurmasını sağlamak için kullanılan bir yazılım mimarisi ve araç setidir.
- Burada şöyle bir örnek üzerinden devam edebiliriz. RabbitMQ farklı sistemler arasında bir iletişim modeli ortaya koymamızı sağlayan bir teknolojidir. Doğru mu? Heh işte! ESB, RabbitMQ gibi farklı sistemlerin birbiriyle etkileşime girmesini sağlayan teknolojilerin kullanımını ve yönetilebilirliğini kolaylaştırmakta ve buna bir ortam sağlamaktadır.
- ESB, servisler arası etkileşim sürecinde aracı uygulamalara karşın yüksek bir abstraction görevi görmekte ve böylece bütünsel olarak sistemin tek bir teknolojiye bağımlı olmasını engellemektedir.
- Bu da, RabbitMQ teknolojisi ile hazırlanan bir sistemin, yarın ihtiyaç duyulduğunda Kafka, Amazon SQS, Azure Service Bus vs. gibi farklı message broker teknolojilerine geçişini kolaylaştırmaktadır.

<p  align="center">
<img  src="https://raw.githubusercontent.com/hasanyurdakul/RABBITMQ_NOTLARI/main/11/images/esb.png"  />
</p>

# MassTransit Nedir?

- .NET için geliştirilmiş olan, distributed uygulamaları rahatlıkla yönetmeyi ve çalıştırmayı amaçlayan, ücretsiz, open source bir Enterprise Service Bus framework'üdür.
- MassTransit, tamamen farklı uygulamalar arasında, message-based communication yapabilmemizi sağlayan bir transport gatewaydir.
- Messaging tabanlı, gevşek bağlı (loosely coupled) ve asenkron olarak tasarlanmış dağınık sistemlerde yüksek dereceli kullanılabilirlik, güvenilirlik ve ölçeklenebilirlik sağlayabilmek için servisler oluşturmayı oldukça kolaylaştırmaktadır.

### Özellikleri

- Open source ve ücretsizdir.
- Kullanımı kolaydır.
- Güçlü mesaj patternlerini desteklemektedir.
- Distributed transaction sağlar.
- Test edilebilirdir.
- Monitoring sağlar.
- Transport işlemlerinin kompleksliğini düşürür.
- Multiple transport desteği sağlar.
- Hata yönetimi mevcuttur.
- Scheduling mevcuttur.
- Request / Response patternlarını destekler.
- Message Broker exchangelerini yönetebilir.

### Transport Gateway Nedir?

- Farklı sistemler arasında farklı iletişim protokollerini kullanarak sistemler arasında iletişim kurmayı sağlayan bir araçtır.
- Bu araç, iletişim protokolleri arasındaki farklılığı gizleyerek bir abstraction oluşturur ve sistemlerin sorunsuz bir biçimde çalışmasını sağlar.
- Misal olarak, Windows işletim sistemi ile Linux arasında bir iletişim süreci söz konusu ise farkı protokollere dayalı olan bu sistemler arasında ister istemez bir uyumsuzluk söz konusu olabilir ve bundan dolayı işlemlerde problemler meydana gelebilir. İşte böyle bir durumda Transport Gateway kullanarak süreç en idel hale getirilebilir.

### MassTransit Hangi Durumlarda Kullanılır?

- Uygulamanın message broker bağımlılığını ortadan kaldırmak için tercih edilebilir.
- MassTransit, öncelikle microservice mimarisi gibi distributed sistemlerin oluşturulması ve bu sistemlerin kendi aralarındaki, haberleşme sürecinde herhangi bir teknolojiye dair olabilecek bağımlılığı soyutlamak için kullanılan bir kütüphanedir.

### Basit Düzeyde MassTransit Kullanımı

- MassTransit kullanımı için öncelikle iki ayrı uygulamaya/servise ihtiyaç vardır.
- Bu servisler ASP.NET Core, Worker Service olabileceği gibi tipik bir console uygulamaları da olabilirler.
- Bizler ilk olarak console uygulamaları üzerinde örneklendirmelerde bulunacak, ardından Worker Service'ler üzerinde de çalışmalar gerçekleştireceğiz.
- Şimdi aşağıdaki görselde olduğu gibi Consumer ve Producer olmak üzere iki ayrı uygulama oluşturunuz.
<p  align="center">
<img  src="https://raw.githubusercontent.com/hasanyurdakul/RABBITMQ_NOTLARI/main/11/images/solutionfiles.png"  />
</p>

- Bu uygulamaların yanında ayrıca Shared isimli bir class library de oluşturduk. Bu katmanda, iki servis arasında gerçekleştirilecek iletişimin mesaj formatı belirlenecektir.
- Görüldüğü üzere her iki servis arasında, içerisinde **Text** değeri olan, **ExampleMessage** türünden bir mesajın iletiminin gerçekleştirileceğini ayarlamış oluyoruz.
<p  align="center">
<img  src="https://raw.githubusercontent.com/hasanyurdakul/RABBITMQ_NOTLARI/main/11/images/shared.png"  />
</p>

- Ardından Consumer ve Producer uygulamaları arasında iletişimi sağlayabilmek için her iki uygulamaya da **MassTransit.RabbitMQ** kütüphanelerini yüklüyoruz.
<p  align="center">
<img  src="https://raw.githubusercontent.com/hasanyurdakul/RABBITMQ_NOTLARI/main/11/images/mtlib.png"  />
</p>

- Bu adımdan sonra, uygulama içeriklerini geliştirmeye odaklanacağız.
- MassTransit'te consumerlar, gelen mesajı IConsumer interface'ini implement etmiş olan sınıflarda **receive** etmektedirler.
  > Console application üzerinde kullanım
  <p  align="center">
  <img  src="https://raw.githubusercontent.com/hasanyurdakul/RABBITMQ_NOTLARI/main/11/images/example1.png"  />
  </p>

> Worker service üzerinde kullanım

<p  align="center">
<img  src="https://raw.githubusercontent.com/hasanyurdakul/RABBITMQ_NOTLARI/main/11/images/example2-1.png"  />
</p>
<p  align="center">
<img  src="https://raw.githubusercontent.com/hasanyurdakul/RABBITMQ_NOTLARI/main/11/images/example2-2.png"  />
</p>

### Publish ile Send Arasındaki Fark Nedir?

Farkındaysanız MassTransit'i kullanırken servisler arasında mesaj iletimini Publish ve Send olmak üzere iki farklı yolla gerçekleştirmiş bulunuyoruz.
**Publish:** Event tabanlı mesaj iletim yöntemini ifade eder. Özünde publish / subscribe pattern'ı uygulanmaktadır. Event publish edildiğinde, o event'e subscribe olan tüm queue'lara mesaj iletilecektir.

</p>
<p  align="center">
<img  src="https://raw.githubusercontent.com/hasanyurdakul/RABBITMQ_NOTLARI/main/11/images/publishersubscriber.png"  />
</p>

**Send:** Command tabanlı mesaj iletim yönetmini ifade eder. Hangi kuyruğa mesaj iletimi gerçekleştirilecekse, endpoint olarak bildirilmesi gerekmektedir.

</p>
<p  align="center">
<img  src="https://raw.githubusercontent.com/hasanyurdakul/RABBITMQ_NOTLARI/main/11/images/senderreceiver.png"  />
</p>
