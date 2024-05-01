Nama: Isa Citra Buana
NPM: 2206081465

1.
- Unary RPC (Remote Procedure Call):
Metode RPC unary melibatkan klien mengirimkan satu permintaan ke server dan kemudian server memproses permintaan tersebut dan mengirimkan satu respons kembali ke klien.
Cocok digunakan saat klien hanya perlu mengirimkan data sekali dan mendapatkan respons tunggal dari server. Contohnya adalah permintaan untuk mendapatkan detail satu entitas, seperti mendapatkan profil pengguna berdasarkan ID.

- Server Streaming RPC:
Dalam metode RPC server streaming, klien mengirimkan satu permintaan ke server dan server mengirimkan serangkaian respons (stream) ke klien.
Cocok digunakan saat klien perlu mendapatkan sejumlah besar data atau hasil yang dihasilkan secara bertahap oleh server. Contohnya adalah permintaan untuk mengambil log data dari server atau mengunduh file dari server.

- Bi-Directional Streaming RPC:
Metode RPC bi-directional streaming melibatkan klien dan server mengirimkan serangkaian pesan (stream) satu sama lain secara terus-menerus.
Cocok digunakan saat klien dan server perlu berkomunikasi secara interaktif, saling bertukar data, dan memproses respons secara paralel. Contohnya adalah fitur chat real-time dan interaksi dalam aplikasi real-time.

2. 
 Ketika kita membuat layanan gRPC di Rust, ada beberapa pertimbangan keamanan yang penting yang perlu dipertimbangkan, terutama seputar otentikasi, otorisasi, dan enkripsi data.

- Otentikasi (Authentication):
Otentikasi berkaitan dengan memastikan bahwa klien yang berkomunikasi dengan layanan gRPC adalah yang seharusnya dan memiliki izin untuk mengakses layanan tersebut.
Dalam Rust, kita bisa menggunakan berbagai mekanisme otentikasi seperti JWT (JSON Web Tokens), OAuth, atau otentikasi berbasis sertifikat untuk memverifikasi identitas klien.
Penting untuk memastikan bahwa hanya klien yang terotorisasi yang dapat mengakses layanan, untuk mencegah akses yang tidak sah atau penyalahgunaan.

- Otorisasi (Authorization):
Otorisasi berkaitan dengan mengendalikan akses ke sumber daya berdasarkan izin atau peran pengguna yang terotentikasi.
Dalam implementasi layanan gRPC, kita perlu memeriksa izin pengguna saat mereka melakukan panggilan ke metode-metode layanan.
Ini bisa dilakukan dengan menggunakan middleware atau interceptor untuk memeriksa peran atau izin pengguna sebelum memproses permintaan mereka.

- Enkripsi Data (Data Encryption):
Enkripsi data penting untuk melindungi kerahasiaan dan integritas data yang dikirimkan antara klien dan server.
Rust memiliki dukungan yang kuat untuk enkripsi data menggunakan protokol seperti TLS (Transport Layer Security).
Dalam implementasi layanan gRPC, kita dapat menggunakan TLS untuk mengenkripsi data yang dikirimkan melalui jaringan, sehingga mencegah pihak ketiga dari mencuri atau mengubah data yang dikirimkan.

3. 
Ketika menangani streaming bidirectional dalam layanan gRPC di Rust, terutama dalam skenario seperti aplikasi obrolan, ada beberapa tantangan atau masalah yang mungkin muncul yang perlu dipertimbangkan.

Pertama-tama, dalam aplikasi obrolan yang menggunakan streaming bidirectional, masalah konsistensi dan konsumsi memori dapat menjadi perhatian. Dalam situasi di mana banyak pengguna terhubung dan berinteraksi secara simultan, server harus mampu mengelola aliran data masuk dan keluar dengan efisien. Ini termasuk memastikan bahwa tidak ada kebocoran memori atau peningkatan konsumsi memori yang signifikan seiring waktu.

Selain itu, keamanan juga menjadi faktor penting. Dalam aplikasi obrolan, penting untuk memastikan bahwa pesan yang dikirim dan diterima oleh pengguna terenkripsi dengan baik dan hanya dapat diakses oleh pihak yang berwenang. Perlindungan terhadap serangan seperti man-in-the-middle (MITM) dan perlindungan terhadap pengiriman pesan yang disusupi adalah hal yang harus diperhatikan.

Selain itu, manajemen kesalahan dan pemulihan yang baik juga penting dalam streaming bidirectional. Koneksi internet yang tidak stabil atau gangguan jaringan lainnya dapat menyebabkan kesalahan dalam pengiriman atau penerimaan pesan. Oleh karena itu, mekanisme untuk mendeteksi kesalahan dan menangani ulang pengiriman pesan yang gagal akan menjadi krusial untuk menjaga kualitas layanan obrolan.

4. 
Menggunakan `tokio_stream::wrappers::ReceiverStream` untuk streaming respons dalam layanan gRPC di Rust memiliki beberapa keunggulan dan kelemahan yang perlu dipertimbangkan. Salah satu keuntungan utamanya adalah kemampuan untuk mengubah `Receiver` dari saluran (channel) Tokio menjadi aliran (stream) yang dapat digunakan secara langsung dalam layanan gRPC. Hal ini memudahkan pengembang untuk mengintegrasikan logika pengiriman data asinkronus dengan mudah, meningkatkan fleksibilitas dan kinerja aplikasi secara keseluruhan. Namun, penggunaan `ReceiverStream` juga memiliki beberapa kelemahan potensial. Salah satunya adalah ketergantungan pada library Tokio, yang membatasi interoperabilitas dengan library lain dan mempersulit migrasi ke abstraksi stream lain jika diperlukan. Selain itu, implementasi yang kurang hati-hati dari aliran penerima (receiver stream) dapat mengakibatkan masalah pemrosesan dan manajemen memori yang kompleks, menyebabkan potensi kebocoran memori atau deadlock dalam aplikasi yang lebih besar. Oleh karena itu, sementara `ReceiverStream` menyediakan solusi yang kuat untuk streaming respons dalam layanan gRPC, pengembang perlu mempertimbangkan baik keuntungan dan tantangan yang terkait dengan penggunaannya.

5. Untuk memfasilitasi penggunaan kembali kode dan modularitas dalam proyek Rust gRPC, struktur kode dapat dirancang dengan memanfaatkan konsep pemisahan tugas dan abstraksi. Salah satu pendekatan yang efektif adalah dengan memisahkan logika bisnis dari detail implementasi gRPC. Ini dapat dicapai dengan membuat modul terpisah untuk definisi layanan gRPC, termasuk protokol dan metode yang diperlukan, serta memisahkan logika bisnis ke dalam modul terpisah. Dengan cara ini, kode yang terkait dengan bisnis dapat diuji secara independen dan digunakan kembali dalam berbagai konteks, memungkinkan pengembang untuk dengan mudah memperluas fungsionalitas tanpa perlu mengubah struktur dasar layanan gRPC.

Selain itu, penggunaan trait dalam Rust dapat sangat bermanfaat untuk mencapai tujuan ini. Dengan mendefinisikan trait untuk operasi umum yang terkait dengan layanan gRPC, seperti otentikasi, otorisasi, atau manajemen kesalahan, kita dapat memisahkan logika ini dari detail implementasi dan mengizinkan penggunaan kembali kode dengan cara yang lebih efektif. Dengan pendekatan ini, pengembang dapat membuat berbagai implementasi yang berbeda untuk trait yang sama, memfasilitasi pengujian dan pengembangan modular yang lebih baik. Dengan menerapkan struktur kode yang terorganisir dengan baik menggunakan konsep pemisahan tugas, abstraksi, dan penggunaan trait, proyek Rust gRPC dapat meningkatkan maintainabilitas dan ekstensibilitasnya dari waktu ke waktu.

6. 
Untuk mengelola proses pembayaran yang kompleks, kita bisa memperbaiki MyPaymentService dengan mengubah fungsi process_payment menjadi layanan streaming. Ini akan memungkinkan kita untuk mengirimkan data ke klien secara bertahap dan efisien, sehingga memfasilitasi transfer informasi yang lebih rumit.

7. 
Pengadopsian gRPC sebagai protokol komunikasi memiliki dampak signifikan pada arsitektur dan desain sistem terdistribusi secara keseluruhan. Dengan gRPC, sistem dapat memanfaatkan arsitektur berbasis layanan (service-based architecture) yang terstruktur dan efisien. Ini karena gRPC mempromosikan pengembangan layanan yang independen, di mana setiap layanan dapat diimplementasikan, diperbarui, dan diperlakukan secara terisolasi. Hal ini memungkinkan pengembang untuk memecah aplikasi menjadi komponen yang lebih kecil dan mudah dikelola, meningkatkan fleksibilitas, skalabilitas, dan toleransi kesalahan. Namun, penggunaan gRPC juga dapat mempengaruhi interoperabilitas dengan teknologi dan platform lain. Meskipun gRPC mendukung berbagai bahasa pemrograman dan platform, integrasi dengan teknologi yang tidak mendukung gRPC mungkin memerlukan upaya tambahan. Oleh karena itu, pengembang perlu mempertimbangkan dengan hati-hati kebutuhan interoperabilitas saat merancang arsitektur dan memilih protokol komunikasi yang sesuai dengan kebutuhan sistem secara keseluruhan.

8. 
Menggunakan HTTP/2 sebagai protokol dasar untuk gRPC memiliki beberapa keunggulan dan kelemahan dibandingkan dengan HTTP/1.1 atau HTTP/1.1 dengan WebSocket untuk API REST. Salah satu keuntungan utama HTTP/2 adalah kemampuannya untuk mengirimkan multiple requests dan responses secara bersamaan dalam satu koneksi, mengurangi latensi dan meningkatkan throughput secara signifikan. Selain itu, HTTP/2 memiliki fitur kompresi header yang efisien dan pengiriman data secara multiplexed, yang mengoptimalkan penggunaan bandwidth dan meningkatkan kinerja aplikasi secara keseluruhan. Namun, terdapat beberapa kelemahan yang perlu dipertimbangkan, termasuk kompleksitas implementasi yang lebih tinggi dan keterbatasan dukungan pada beberapa platform dan server. Selain itu, penggunaan HTTP/2 dapat memerlukan overhead tambahan dalam manajemen koneksi dan buffering, yang dapat mengakibatkan peningkatan konsumsi sumber daya pada server.

Di sisi lain, HTTP/1.1 atau HTTP/1.1 dengan WebSocket untuk API REST memiliki keuntungan dalam kesederhanaan implementasi dan dukungan yang lebih luas pada berbagai platform dan infrastruktur. Protokol HTTP/1.1 lebih mudah dipahami dan diterapkan, dan WebSocket memungkinkan komunikasi real-time dua arah antara klien dan server. Namun, ada beberapa kelemahan yang perlu diatasi, seperti keterbatasan dalam penanganan multiple requests dan responses secara bersamaan, serta potensi overhead dalam pembukaan koneksi baru untuk setiap permintaan. Selain itu, HTTP/1.1 dan WebSocket mungkin tidak seefisien HTTP/2 dalam hal penggunaan bandwidth dan kinerja, terutama dalam skenario di mana latensi rendah dan throughput tinggi menjadi prioritas utama. Dengan mempertimbangkan keuntungan dan kerugian dari masing-masing protokol, pengembang harus memilih protokol yang paling sesuai dengan kebutuhan spesifik dari aplikasi mereka, memperhatikan faktor-faktor seperti kinerja, kompleksitas, dan dukungan platform.

9. 
Model permintaan-respon dari API REST berbeda dengan kemampuan streaming bidirectional dari gRPC dalam hal komunikasi real-time dan responsivitas. Dalam model permintaan-respon REST, klien mengirimkan permintaan ke server dan menunggu respon dari server sebelum melanjutkan eksekusi. Ini cocok untuk skenario di mana klien hanya memerlukan respons tunggal dari server, tetapi kurang ideal untuk komunikasi real-time yang membutuhkan respons cepat dan kontinu. Di sisi lain, gRPC menawarkan kemampuan streaming bidirectional yang memungkinkan klien dan server saling berkomunikasi secara simultan dan kontinyu. Dengan menggunakan streaming bidirectional, gRPC dapat menyediakan respons secara real-time, memungkinkan aplikasi untuk merespons perubahan status atau kejadian secara langsung tanpa harus menunggu permintaan baru dari klien. Hal ini membuat gRPC lebih responsif dalam skenario yang membutuhkan komunikasi real-time seperti aplikasi obrolan, streaming video, atau permainan daring.

10. 
Pendekatan berbasis skema yang digunakan oleh gRPC dengan menggunakan Protocol Buffers memiliki beberapa implikasi yang berbeda dibandingkan dengan pendekatan yang lebih fleksibel dan tanpa skema yang digunakan oleh JSON dalam muatan API REST. Dalam gRPC, menggunakan Protocol Buffers memungkinkan definisi yang jelas dan kaku tentang struktur data yang dikirimkan antara klien dan server. Ini berarti bahwa kesalahan dalam struktur data dapat dideteksi secara statis selama kompilasi, memberikan keamanan tambahan dan menjamin kompatibilitas antara klien dan server. Namun, kelemahan dari pendekatan ini adalah bahwa fleksibilitasnya terbatas, karena perubahan dalam skema data memerlukan pembaruan dan re-kompilasi dari kode yang terlibat, yang bisa menjadi sulit dalam lingkungan yang berkembang dengan cepat.

Di sisi lain, penggunaan JSON dalam REST API memungkinkan fleksibilitas yang lebih besar dalam muatan data, karena sifatnya yang tidak terikat oleh skema. Ini memungkinkan pengembang untuk menambahkan atau mengubah bidang data tanpa memerlukan pembaruan pada sisi klien atau server. Namun, kekurangan dari pendekatan ini adalah bahwa tidak ada jaminan tentang struktur atau tipe data yang dikirimkan, yang dapat menyebabkan kesalahan pada waktu eksekusi jika ada perbedaan antara apa yang diharapkan oleh klien dan apa yang diterima oleh server. Oleh karena itu, sementara JSON dalam REST API menyediakan fleksibilitas yang lebih besar, hal ini juga memunculkan tantangan dalam menjaga konsistensi dan keandalan aplikasi secara keseluruhan.