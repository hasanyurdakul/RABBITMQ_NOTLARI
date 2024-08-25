# Mesaj Tasarımları

## Mesaj Tasarımlarından Kastedilen Nedir?

- Mesaj tasarımlarından kastedilen, tıpkı design patternlarda olduğu gibi belli başlı senaryolara karşı gösterilebilecek önceden tanımlanmış, tarif edilmiş ve praktiksel olarak adımları saptanmış davranışlardır.
- Belirli bir problemi çözmek için kullanılan bu tasarımlar, genel anlamda yapısal davranışı ve iletişim modelini ifade etmektedir.
- Yani iki servis arasında message broker ile yapılacak hebrleşme sürecinde iletikecek mesajların nasıl iletilmesi gerektiğini, nasıl işleneceğini ve ne şekilde yapılandırılacağını ve ne tür bilgiler tanımlanacağını belirler.
- Her tasarım farklı bir uygulama senaryosuna ve gereksinimine göre şekillenmekte ve en iyi sonuçlar alınabilecek şekilde yapılandırılmalıdır.

## Yaygın Mesaj Tasarımları Nelerdir ?

### P2P (Point to Point) Mesaj Tasarımı

Bu tasarımda, bir publisher ilgili mesajı bir kuyruğa gönderir ve bu mesaj kuyruğu işleyen bir consumer taradından tüketilir. Eğer ki senaryo gereği bir mesajın bir tüketici taradından işlenmesi gerekiyorsa bu yaklaşım kullanılır. P2P tasarımı gerektiren senaryolarda genellikle direct exchange modeli kullanılır.

<p  align="center">
<img  src="https://raw.githubusercontent.com/hasanyurdakul/RABBITMQ_NOTLARI/main/10/images/p2p.png"  />
</p>

### Publish / Subscribe (Pub/Sub) Tasarımı

Bu tasarımda, bir publisher mesajı bir exchange'e gönderir ve böylece bu exchange'e bind edilmiş olan tüm kuyruklara yönlendirilir. Bu tasarım, bir mesajın birden fazla consumer tarafından işlenmesi gereken senaryolarda tercih edilir. Pub / Sub uygulanırken genellikle fanout exchange tercih edilir.

<p  align="center">
<img  src="https://raw.githubusercontent.com/hasanyurdakul/RABBITMQ_NOTLARI/main/10/images/p2p.png"  />
</p>

### Work Queue Tasarımı

Bu tasarımda, publisher tarafından yayınlanmış bir mesajın birden fazla consumer arasından yalnızca biri tarafından tüketilmesi amaçlanmaktadır. Böylece mesajların işlenmesi sürecinde tüm consumerlar aynı iş yüküne ve eşit görev dağılımına sahip olacaktır. Work Queue tasarımı, iş yükünün dağıtılması gereken ve paralel işlem yapılması gereken senaryolar için oldukça uygundur.

<p  align="center">
<img  src="https://raw.githubusercontent.com/hasanyurdakul/RABBITMQ_NOTLARI/main/10/images/workqueue.png"  />
</p>

### Request / Response Tasarımı

Bu tasarımda, publisher bir request yapar gibi kuyruğa mesaj gönderir ve bu mesajı tüketen consumer'dan sonuca dair başka bir kuyruktan bir yanıt/response bekler. Bu tarz senaryolar için oldukça uygun bir tasarımdır.

<p  align="center">
<img  src="https://raw.githubusercontent.com/hasanyurdakul/RABBITMQ_NOTLARI/main/10/images/reqres.png"  />
</p>

<p  align="center">
<img  src="https://raw.githubusercontent.com/hasanyurdakul/RABBITMQ_NOTLARI/main/10/images/reqres2.png"  />
</p>
