> a. What is amqp?  
 
AMQP (Advanced Message Queuing Protocol) adalah sebuah protokol standar terbuka pada lapisan aplikasi yang dirancang untuk mendukung pengiriman pesan antar komponen sistem secara efisien, aman, dan andal. Protokol ini memungkinkan komunikasi terstandarisasi antara aplikasi client dan message broker seperti RabbitMQ atau Kafka. Dengan menggunakan AMQP, data atau event dapat dikirimkan secara asinkron, sehingga pengirim dan penerima tidak perlu terhubung secara langsung pada waktu yang bersamaan.

> b. What does it mean? guest:guest@localhost:5672 , what is the first guest, and what is the second guest, and what is localhost:5672 is for?  

Format guest:guest@localhost:5672 merupakan sebuah URL atau connection string yang digunakan oleh program untuk melakukan autentikasi dan terhubung ke message broker. Dalam format tersebut, kata guest yang pertama berfungsi sebagai username default, sedangkan kata guest yang kedua berperan sebagai password default untuk mendapatkan hak akses ke dalam sistem RabbitMQ. Selanjutnya, bagian localhost:5672 menunjukkan lokasi server tempat berjalannya message broker tersebut. Secara lebih spesifik, localhost menandakan bahwa RabbitMQ beroperasi secara langsung pada mesin lokal komputer ini, dan ia siap menerima koneksi pertukaran data melalui port 5672.

## Simulation slow subscriber

![](image/RabbitMQ-queue.png)

Berdasarkan grafik tersebut, simulasi slow subscriber telah berhasil dibuktikan dengan munculnya lonjakan signifikan pada grafik "Queued messages". Garis merah menunjukkan bahwa terdapat hingga 15 pesan yang berstatus Unacknowledged secara bersamaan, hal ini terjadi karena publisher dijalankan beberapa kali secara cepat sementara subscriber memproses setiap pesan dengan jeda 1 detik menggunakan thread::sleep.

Pada grafik "Message rates", terlihat bahwa garis oranye (Publish) melonjak tajam saat pesan dikirim, namun garis ungu (Consumer ack) bergerak secara linear dan datar. Perbedaan pola ini menunjukkan adanya backpressure atau penumpukan beban kerja, di mana RabbitMQ bertindak sebagai perantara yang aman untuk menampung lonjakan data agar subscriber yang lebih lambat tidak mengalami overload. Pesan-pesan tersebut tidak hilang, melainkan tersimpan di antrean dan diselesaikan secara bertahap hingga grafik kembali ke angka nol.