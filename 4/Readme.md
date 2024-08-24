# Publisher ve Consumer

- .NET teknolojileri ile RabbitMQ'yu kullanabilmek için RabbitMQ.Client kütüphanesini projenize yüklemeniz gerekmektedir. Publisher veya Consumer uygulamalarının her ikisinde de aynı kütüphane kullanılır.

### Publisher Uygulaması İçin İşlem Sırası

1. **Bağlantı oluşturma**: RabbitMQ sunucusuna bağlantı olusturunuz.
2. **Bağlantıyı aktifleştirme ve kanal açma**: Bağlantıyı aktifleştiriniz ve ardından bu bağlantı üzerinden işlemleri yürütebilmek için bir kanal açınız.
3. **Queue oluşturma**: Mesajların gönderileceği kuyruğu olusturunuz.
 <p align=center>
<img src="https://raw.githubusercontent.com/hasanyurdakul/RABBITMQ_NOTLARI/main/4/images/publisher.png" />
</p>

### Consumer Uygulaması İçin İşlem Sırası

1. **Bağlantı oluşturma**: RabbitMQ sunucusuna bağlantı olusturunuz.
2. **Bağlantıyı aktifleştirme ve kanal açma**: Bağlantıyı aktifleştiriniz ve ardından bu bağlantı üzerinden işlemleri yürütebilmek için bir kanal açınız.
3. **Queue oluşturma**: Mesajların gönderileceği kuyruğu olusturunuz.
4. **Queue'dan mesaj okuma**: Kuyruktaki mesajları okuyunuz.
 <p align=center>
<img src="https://raw.githubusercontent.com/hasanyurdakul/RABBITMQ_NOTLARI/main/4/images/consumer.png" />
</p>
