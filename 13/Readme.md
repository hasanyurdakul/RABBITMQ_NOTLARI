# MassTransit Message Acknowledgement

- MassTransit kütüphanesi, RabbitMQ'daki herhangi bir kuyruktaki bir mesajı aldığı taktirde, bu mesajla ilgili işlemi garantiye almadığı sürece, bu mesajı kuyruktan sildirmeyecektir.
- Ne zaman ki Consumer mesajı işler, MassTransit Message Acknowledgement özelliğini bizim bildirmemize gerek kalmaksızın, işlenen mesajı ilgili kuyruktan siler.
- Mesajın işlenmesi sırasında herhangi bir hata meydana gelirse, MassTransit, "MessageRetry" yapısı ile mesajı tekrardan kuyruğa alacak ve işlemeye çalışacaktır.
