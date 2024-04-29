1. What are the key differences between unary, server streaming, and bi-directional streaming RPC (Remote Procedure Call) methods, and in what scenarios would each be most suitable?<br>

> - `Unary RPC` adalah format yang paling sederhana dalam RPC di mana klien mengirimkan permintaan ke server dan menunggu untuk menerima respons dari server. Biasanya digunakan dalam skenario permintaan-respons yang sederhana di mana tidak ada interaksi tambahan dengan data permintaan yang diperlukan, seperti pengambilan data tertentu dari server.
> - `Server streaming RPC` terjadi ketika klien mengirimkan permintaan ke server dan server memberikan respons dalam bentuk serangkaian pesan berurutan hingga tidak ada lagi pesan yang dikirim. Biasanya digunakan ketika klien memerlukan server untuk mengirimkan data yang besar, seperti pengambilan volume data besar dari basis data.
> - `Bi-Directional Streaming RPC` terjadi ketika kedua belah pihak, baik klien maupun server, dapat mengirimkan serangkaian pesan secara independen menggunakan aliran baca-tulis dalam arah yang sama atau berlawanan secara bersamaan. Biasanya digunakan ketika klien dan server perlu berinteraksi untuk mengirimkan dan menerima data secara real-time, seperti Layanan Obrolan.

2. What are the potential security considerations involved in implementing a gRPC service in Rust, particularly regarding authentication, authorization, and data encryption?
> Ketika menerapkan layanan gRPC dalam Rust, kita harus memverifikasi identitas pengguna atau service. Hal ini dapat dicapai dengan menggunakan token seperti JWT atau melalui integrasi dengan sistem OAuth2. Manajemen otorisasi juga diperlukan untuk mengatur akses ke sumber daya berdasarkan peran atau aturan yang telah ditetapkan, dan dapat diimplementasikan melalui middleware. Selain itu, untuk menjaga keamanan komunikasi antara klien dan server, kita dapat menggunakan enkripsi data, misalnya melalui TLS, untuk mencegah potensi penyadapan data dan menjaga kerahasiaan informasi yang sedang ditransmisikan.

3. What are the potential challenges or issues that may arise when handling bidirectional streaming in Rust gRPC, especially in scenarios like chat applications?
> Masalah masalah yang mungkin muncul adalah saat read-write stream, Connection Management seperti menangani pemutusan koneksi yang tidak terduga dan mengembalikan koneksi dengan cepat karena client dapat connect dan disconnect kapanpun, dan juga Concurrency Management (Message Synchronization) karena banyak client dapat mengirim dan menerima pesan secara bersamaan sehingga perlu manage responses dari client yang berbeda-beda secara tepat.

4. What are the advantages and disadvantages of using the tokio_stream::wrappers::ReceiverStream for streaming responses in Rust gRPC services?
> ### Keuntungan:
> - Tokio-Stream terintegrasi dengan ekosistem Tokio yang memungkinkan untuk bekerja dengan streams pada asynchronous
> - ReceiverStream dapat mengubah tokio receiver menjadi stream yang dapat diproses oleh tonic

> ### Kerugian:
> - ReceiverStream hanya dapat mendukung komunikasi dari pengirim ke penerima saja (Hanya satu arah)
> - Jika terjadi error ketika menerima message, ReceiverStream tidak memiliki built-in mechanism yang dapat membantu mengatasi hal tersebut.


5. In what ways could the Rust gRPC code be structured to facilitate code reuse and modularity, promoting maintainability and extensibility over time?
> Kita dapat membuat kode Rust gRPC untuk dapat digunakan kembali dan meningkatkan modularitas dengan beberapa cara seperti membuat modul terpisah antara kode Rust gRPC dengan business logic, penggunaan library external untuk fungsionalitas umum yang dapat digunakan kembali, dan menggunakan traits dan generics untuk komponen yang fleksibel sehingga dapat meningkatkan maintainability dan ekstensibilitas
6. In the MyPaymentService implementation, what additional steps might be necessary to handle more complex payment processing logic?
> Implementasi MyPaymentService dapat ditambahkan beberapa langkah berikut untuk menangani proses payment yang lebih kompleks:
> - Validasi PaymentRequest untuk keamanan dan kelengkapan data
> - Pemantauan status pembayaran real-time
> - Interaksi dengan database secara langsung untuk mengecek balance user, update balance setelah payment, dan merecord transaction tersebut.
7. What impact does the adoption of gRPC as a communication protocol have on the overall architecture and design of distributed systems, particularly in terms of interoperability with other technologies and platforms?
> Penerapan gRPC mempermudah keterhubungan antar modul dengan secara otomatis menangani panggilan metode antarsistem melalui definisi yang terdapat dalam file proto. Ini mengurangi kebutuhan untuk mengatur konfigurasi metode HTTP secara manual. Dikarenakan gRPC menggunakan protokol HTTP/2, yang mendukung komunikasi yang cepat dan efisien antara layanan, termasuk kemampuan streaming yang handal dan overhead yang minim, sehingga interaksi antarmodul menjadi lebih efisien
8. What are the advantages and disadvantages of using HTTP/2, the underlying protocol for gRPC, compared to HTTP/1.1 or HTTP/1.1 with WebSocket for REST APIs?
> HTTP/2 dapat mendukung komunikasi yang lebih cepat dan scalable antara client dan server dengan fitur- fiturnya seperti multiplexing, header compression, dan server push. Fitur ini sesuai dengan tujuan gRPC yaitu meningkatkan performance dan low latency communication.<br>
> Kekurangan dari HTTP/2 adalah kompleksitasnya yang menyebabkan HTTP/2 lebih sulit diimplementasikan dan didebug. HTTP/2 juga mengonsumsi lebih banyak resource server.
9. How does the request-response model of REST APIs contrast with the bidirectional streaming capabilities of gRPC in terms of real-time communication and responsiveness?
> REST API mengikuti pola response-request, di mana klien mengirim permintaan ke server dan server memberikan respons dengan data yang diminta. Sementara gRPC memiliki dukungan untuk bidirectional streaming di mana klien dan server dapat bertukar pesan secara asinkron melalui satu koneksi, memungkinkan komunikasi secara real-time di mana keduanya dapat saling bertukar data tanpa harus menunggu request atau response.
10. What are the implications of the schema-based approach of gRPC, using Protocol Buffers, compared to the more flexible, schema-less nature of JSON in REST API payloads?
> Pendekatan gRPC yang berbasis skema dengan menggunakan Protocol Buffers memberikan validasi data yang ketat, otomatisasi kode, dan konsistensi yang kuat antara klien dan server. Di sisi lain, penggunaan JSON dalam REST API memberikan fleksibilitas yang lebih besar dalam penanganan data yang dinamis dan perubahan struktur tanpa perlu melakukan pembaruan skema atau kode. Pilihan antara keduanya bergantung pada kebutuhan spesifik dari masing-masing kasus penggunaan.