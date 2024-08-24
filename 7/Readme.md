# Direct Exchange Davranışı

### Publisher

<p align="center">
<img  src="https://raw.githubusercontent.com/hasanyurdakul/RABBITMQ_NOTLARI/main/6/images/publisher.png" />
</p>

### Consumer

<p align="center">
<img  src="https://raw.githubusercontent.com/hasanyurdakul/RABBITMQ_NOTLARI/main/6/images/consumer.png" />
</p>

1. **Adım**: Publisherdaki exchange ile birebir aynı isim ve type'a sahip bir exchange declare edilmelidir.
2. **Adım**: Publisher tarafından routing key'de bulunan değerdeki kuyruğa gönderilen mesajları, kendi oluşturduğumuz kuyruğa yönlendirerek tüketmemiz gerekmektedir. Bunun için öncelikle bir kuyruk oluşturulmalıdır.
3. **Adım**: Oluşturulan queue ve exhange birbirleri ile bind edilmelidir.
