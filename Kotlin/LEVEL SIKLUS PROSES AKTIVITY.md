# LEVEL SIKLUS PROSES AKTIVITY
## Mempelajari metode siklus proses dan menambahkan logging dasar
Setiap aktivitas memiliki tahapan yang dikenal sebagai siklus proses. Tahapan ini adalah menggambarkan siklus proses tanaman dan hewan, seperti siklus hidup kupu-kupu ini—keadaan kupu-kupu yang berbeda menunjukkan pertumbuhannya mulai dari lahir, kemudian dewasa, hingga mati.

<p align="center"> <img src="https://developer.android.com/static/codelabs/basic-android-kotlin-training-activity-lifecycle/img/c685f48ff799f0c9.png" width="35%" height="35%"/> </p>

Demikian pula, siklus proses aktivitas terdiri dari berbagai status yang dapat dijalankan oleh suatu aktivitas, mulai dari saat aktivitas pertama kali diinisialisasi hingga dihancurkan dan memorinya diklaim kembali oleh sistem. Saat pengguna memulai aplikasi, berpindah antar-aktivitas, serta melakukan navigasi dalam dan di luar aplikasi Anda, aktivitas tersebut akan mengubah status. Diagram di bawah menunjukkan semua status siklus proses aktivitas. Seperti yang terlihat dari namanya, status ini menyatakan status aktivitas.

<p align="center"> <img src="https://developer.android.com/static/codelabs/basic-android-kotlin-training-activity-lifecycle/img/c803811f4cb4034b.png" width="35%" height="35%"/> </p>

Sering kali, Anda ingin mengubah beberapa perilaku, atau menjalankan beberapa kode saat status siklus proses aktivitas berubah. Oleh karena itu, class Activity itu sendiri, dan subclass Activity seperti AppCompatActivity, menerapkan sekumpulan metode callback siklus proses. Android mengaktifkan callback ini saat aktivitas berpindah dari satu status ke status lainnya, dan Anda dapat mengganti metode tersebut dalam aktivitas Anda sendiri untuk menjalankan tugas sebagai respons terhadap perubahan status siklus proses tersebut. Diagram berikut menampilkan status siklus proses beserta callback yang bisa diganti.

<p align="center"> <img src="https://developer.android.com/static/codelabs/basic-android-kotlin-training-activity-lifecycle/img/f6b25a71cec4e401.png" width="35%" height="35%"/> </p>

Penting untuk mengetahui waktu pemanggilan callback ini dan tugas yang harus dilakukan di setiap metode callback. Namun kedua diagram ini rumit dan mungkin membingungkan. Dalam codelab ini, Anda tidak hanya membaca arti setiap status dan callback, tetapi juga melakukan beberapa pekerjaan menyelidiki dan membangun pemahaman Anda tentang situasi yang terjadi.

# **Langkah 1: Memeriksa metode onCreate() dan menambahkan logging**

Untuk mengetahui aktivitas siklus proses Android, sebaiknya ketahui waktu pemanggilan berbagai metode siklus proses. Ini akan membantu Anda berburu berbagai tempat yang tidak beres di aplikasi DessertClicker.

Cara sederhana untuk melakukannya adalah menggunakan fungsionalitas logging Android. Dengan logging, Anda dapat menulis pesan singkat ke konsol saat aplikasi berjalan, dan menggunakannya untuk menampilkan Anda saat memicu callback yang berbeda.

1. Jalankan aplikasi DessertClicker, lalu ketuk gambar makanan penutup beberapa kali. Perhatikan perubahan nilai dari Makanan Penutup Terjual dan jumlah total uang dalam dolar.
2. Buka MainActivity.kt dan periksa metode onCreate() untuk aktivitas ini:
 > Dalam diagram siklus proses aktivitas, Anda mungkin telah mengenali metode onCreate(), karena Anda sudah pernah menggunakan callback ini sebelumnya. Ini adalah satu metode yang harus diterapkan setiap aktivitas. Metode onCreate() menjadi tempat wajib untuk melakukan inisialisasi satu kali untuk aktivitas Anda. Misalnya, dalam onCreate() Anda meng-inflate tata letak, menentukan pemroses klik, atau menyiapkan view binding.
<p align="center">
 <img src="https://developer.android.com/static/codelabs/basic-android-kotlin-training-activity-lifecycle/img/9be2255ff49e0af8.png" width="35%" height="35%">
<p/>

 Metode siklus proses onCreate() dipanggil satu kali, tepat setelah aktivitas diinisialisasi (saat objek Activity baru dibuat di memori). Setelah onCreate() dijalankan, aktivitas dianggap telah dibuat.

> Catatan: Saat mengganti metode onCreate(), Anda harus memanggil implementasi superclass untuk menyelesaikan pembuatan Aktivitas. Jadi, di dalamnya, Anda harus segera memanggil super.onCreate(). Hal yang sama berlaku untuk metode callback siklus proses lainnya.

3. Dalam metode onCreate(), tepat setelah panggilan ke super.onCreate(), tambahkan baris berikut.

```kotlin
Log.d("MainActivity", "onCreated is called")
```

Class Log menulis pesan ke Logcat. Logcat merupakan konsol untuk mencatat pesan. Pesan dari Android tentang aplikasi Anda akan muncul di sini, termasuk pesan yang Anda kirim secara eksplisit ke log dengan metode Log.d() atau metode class Log lainnya.

- Ada tiga bagian untuk perintah ini:
  * Prioritas pesan log, yaitu seberapa penting pesan tersebut. Dalam kasus ini, metode Log.d() menulis pesan debug. Metode lain di class Log mencakup Log.i() untuk pesan informasi, Log.e() untuk error, Log.w() untuk peringatan, atau Log.v() untuk pesan verbose.
  * Log tag (parameter pertama), dalam hal ini adalah "MainActivity". Tag adalah string yang memungkinkan Anda menemukan pesan log dengan lebih mudah di Logcat. Tag biasanya berupa nama kelas.
  * Log pesan yang sebenarnya (parameter kedua), adalah string pendek bernama "onCreate called".

 > Catatan: Akan lebih baik jika Anda mendeklarasikan konstanta TAG di class Anda:
 const val TAG = "MainActivity"
 dan menggunakannya dalam panggilan berikutnya di metode log, seperti di bawah ini:
 Log.d(TAG, "onCreate Called")

Konstanta waktu kompilasi adalah nilai yang tidak akan berubah. Gunakan const sebelum deklarasi variabel untuk menandainya sebagai konstanta waktu kompilasi.

4. Kompilasi dan jalankan aplikasi DessertClicker. Anda tidak akan melihat perbedaan perilaku di aplikasi saat mengetuk makanan penutup. Di bagian bawah layar, Android Studio klik tab Logcat.

Logcat dapat berisi banyak pesan dan sebagian besar tidak berguna bagi Anda. Anda dapat memfilter entri Logcat dengan berbagai cara, tetapi menggunakan penelusuran adalah cara termudah. Anda telah menggunakan MainActivity sebagai tag log dalam kode. Oleh karena itu, Anda dapat menggunakan tag tersebut untuk memfilter log. Menambahkan D/ di awal menunjukkan bahwa ini adalah pesan debug yang dibuat oleh Log.d().

Pesan log Anda berisi tanggal dan waktu, nama paket (com.example.android.dessertclicker), tag log Anda (dengan D/ di awal), dan pesan yang sebenarnya. Anda tahu bahwa onCreate() telah dijalankan karena pesan ini muncul di log.

## Langkah 2: Mengimplementasikan metode onStart()
Metode siklus proses onStart() dipanggil tepat setelah onCreate(). Setelah onStart() berjalan, aktivitas Anda akan terlihat di layar. Tidak seperti onCreate() yang hanya dipanggil sekali untuk menginisialisasi aktivitas Anda, onStart() dapat dipanggil beberapa kali dalam siklus proses aktivitas Anda.

<p align="center">
 <img src="https://developer.android.com/static/codelabs/basic-android-kotlin-training-activity-lifecycle/img/385df4ce82ae2de9.png" width="35%" height="35%">
<p/>

Perlu diketahui bahwa onStart() dipasangkan dengan metode siklus proses onStop() yang sesuai. Jika pengguna memulai aplikasi Anda lalu kembali ke layar utama perangkat, aktivitas tersebut akan dihentikan dan tidak lagi terlihat di layar.

1. Di Android Studio, dengan MainActivity.kt terbuka dan kursor ada di dalam class MainActivity, pilih Code > Override Methods atau tekan Control+o (Command+o pada Mac). Dialog akan muncul dan menampilkan daftar

2. Mulai masukkan onStart untuk menelusuri metode yang tepat. Untuk men-scroll ke item yang cocok berikutnya, gunakan panah bawah. Pilih onStart() dari daftar, lalu klik Oke untuk menyisipkan kode pengganti boilerplate. Kode terlihat seperti ini:
```kotlin
override fun onStart() {
  super.onStart()
}
```

3. tambahkan konstanta berikut di tinggkat teratas MainActivity.kt yang berada di atas deklarasi class, class MainActivity.
```kotlin
const val TAG = "MainActivity"
```

4. di dalam metode onStart() tambahkan pesan log:
```kotlin
override fun onStart() {
  super.onStart()
  Log.d(TAG, "onStart is Called")
}
```

5. Kompilasi dan jalankan aplikasi DessertClicker, dan buka panel Logcat. Ketikkan D/MainActivity pada penelusuran untuk memfilter log. Perhatikan bahwa metode onCreate() dan onStart() dipanggil satu demi satu, dan aktivitas Anda akan terlihat di layar.

6. Tekan tombol Layar utama di perangkat, lalu gunakan layar terbaru untuk kembali ke aktivitas. Perhatikan bahwa aktivitas dilanjutkan dari tempat terakhirnya, dengan semua nilai yang sama, dan onStart() dicatat dalam log untuk kedua kalinya di Logcat. Perhatikan juga bahwa metode onCreate() biasanya tidak dipanggil lagi.

```terminal 
16:19:59.125 31107-31107/com.example.android.dessertclicker D/MainActivity: onCreate Called
16:19:59.372 31107-31107/com.example.android.dessertclicker D/MainActivity: onStart Called
16:20:11.319 31107-31107/com.example.android.dessertclicker D/MainActivity: onStart Called
```
> Catatan: Saat bereksperimen dengan perangkat dan mengamati callback siklus proses, Anda mungkin akan melihat perilaku yang tidak wajar saat merotasi perangkat. Anda akan mempelajari perilaku tersebut.

## Langkah 3: Menambahkan laporan bug lainnya
pada langkah ini, anda akan menerapkan logging untuk semua metode siklus proses lainnya.
1. Ganti metode sisa siklus proses di MainActivity, dan tambahkan laopran untuk setiap metode. Berikut kodenya: 
```kotlin
override fun onResume() {
  super.onResume()
  Log.d(TAG, "onResume is Called")
}

override fun onPause() {
  super.onPause()
  Log.d(TAG, "onPause is Called")
}

override fun onStop() {
  super.onStop()
  Log.d(TAG, "onStop is Called")
}

override fun onDestroy() {
  super.onDestroy()
  Log.d(TAG, "onDestroy is Called")
}

override fun onRestart() {
  super.onRestart()
  Log.d(TAG, "onRestart is Called")
}
```

2. Kompilasi dan jalankan lagi aplikasi DessertClicker dan periksa Logcat. Kali ini, selain onCreate() dan onStart(), ada pesan log untuk callback siklus proses onResume().
```terminal 
2020-10-16 10:27:33.244 22064-22064/com.example.android.dessertclicker D/MainActivity: onCreate Called
2020-10-16 10:27:33.453 22064-22064/com.example.android.dessertclicker D/MainActivity: onStart Called
2020-10-16 10:27:33.454 22064-22064/com.example.android.dessertclicker D/MainActivity: onResume Called
```

- Saat aktivitas dimulai dari awal, anda akan melihat ketiga callback siklus proses ini dipanggil secara berurutan:
  * onCreate() untuk membuat aplikasi.
  * onStart() untuk memulai dan menampilkannya dilayar.
  * onResume() untuk memberikan fokus pada aktivitas dan mempersiapkan pengguna untuk berinteraksi dengannya.

Terlepas dari namanya, metode onResume() akan dipanggil saat startup meskipun tidak ada yang dapat dilanjutkan.
<p align="center">
<img src="https://developer.android.com/static/codelabs/basic-android-kotlin-training-activity-lifecycle/img/160054d59f67519.png" width="35%" height="35%"/>
<p/>

## 4. Mempelajari kasus penggunaan siklus proses
Sekarang, setelah aplikasi DessertClicker disiapkan untuk logging, Anda siap untuk mulai menggunakan aplikasi dengan berbagai cara, dan juga mempelajari cara memicu callback siklus proses sebagai respons terhadap penggunaan tersebut.

Kasus penggunaan 1: Membuka dan menutup aktivitas
Mulailah dengan kasus penggunaan yang paling dasar, yaitu memulai aplikasi untuk pertama kali, lalu tutup aplikasi.

Kompilasi dan jalankan aplikasi DessertClicker jika belum berjalan. Seperti yang Anda lihat, callback onCreate(), onStart(), dan onResume() dipanggil saat aktivitas dimulai pertama kali.
```terminal 
2020-10-16 10:27:33.244 22064-22064/com.example.android.dessertclicker D/MainActivity: onCreate Called
2020-10-16 10:27:33.453 22064-22064/com.example.android.dessertclicker D/MainActivity: onStart Called
2020-10-16 10:27:33.454 22064-22064/com.example.android.dessertclicker D/MainActivity: onResume Called
```

Ketuk cupcake beberapa kali.
Ketuk tombol Kembali yang ada di perangkat. Perhatikan dalam Logcat bahwa onPause(), onStop(), dan onDestroy() dipanggil dalam urutan tersebut.

```terminal 
2020-10-16 10:31:53.850 22064-22064/com.example.android.dessertclicker D/MainActivity: onPause Called
2020-10-16 10:31:54.620 22064-22064/com.example.android.dessertclicker D/MainActivity: onStop Called
2020-10-16 10:31:54.622 22064-22064/com.example.android.dessertclicker D/MainActivity: onDestroy Called
```
Dalam hal ini, menggunakan tombol Kembali akan menyebabkan aktivitas (dan aplikasi) ditutup. Eksekusi metode onDestroy() berarti aktivitas telah benar-benar dinonaktifkan dan sampah memori dapat dibersihkan. Pembersihan sampah memori mengacu pada pembersihan objek yang tidak akan digunakan lagi secara otomatis. Setelah onDestroy() dipanggil, sistem akan mengetahui bahwa resource tersebut dapat dihapus, dan memori akan mulai dibersihkan 
<p align="center">
<img src="https://developer.android.com/static/codelabs/basic-android-kotlin-training-activity-lifecycle/img/2dcc4d9c6478a9f4.png" width="35%" height="35%"/>
<p/>

Aktivitas Anda juga dapat dinonaktifkan total jika kode Anda memanggil metode finish() aktivitas secara manual, atau jika pengguna menutup aplikasi secara paksa (misalnya, pengguna dapat menutup paksa atau menutup aplikasi di layar terbaru.) Sistem Android juga dapat menghentikan aktivitas Anda jika aplikasi tidak digunakan untuk waktu yang lama. Android melakukan hal ini untuk menghemat baterai, dan memungkinkan resource aplikasi Anda digunakan oleh aplikasi lain.

Kembali ke aplikasi DessertClicker dengan menemukan semua aplikasi yang terbuka di layar Ringkasan. (Perhatikan bahwa ini juga dikenal sebagai layar Terbaru atau aplikasi terbaru.) Berikut Logcat-nya:

```terminal
2020-10-16 10:31:54.622 22064-22064/com.example.android.dessertclicker D/MainActivity: onDestroy Called
2020-10-16 10:38:00.733 22064-22064/com.example.android.dessertclicker D/MainActivity: onCreate Called
2020-10-16 10:38:00.787 22064-22064/com.example.android.dessertclicker D/MainActivity: onStart Called
2020-10-16 10:38:00.788 22064-22064/com.example.android.dessertclicker D/MainActivity: onResume Called
```
Aktivitas ini dihancurkan pada langkah sebelumnya sehingga saat Anda kembali ke aplikasi, Android akan memulai aktivitas baru dan memanggil metode onCreate(), onStart(), dan onResume(). Perhatikan bahwa tidak satu pun log DessertClicker dari aktivitas sebelumnya yang disimpan.

> Catatan: Hal yang perlu digarisbawahi adalah bahwa onCreate() dan onDestroy() hanya dipanggil sekali selama masa aktif dari instance aktivitas tunggal: onCreate() untuk menginisialisasi aplikasi untuk pertama kalinya dan onDestroy() untuk membersihkan resource yang digunakan oleh aplikasi Anda.

Metode onCreate() merupakan langkah penting karena digunakan sebagai tempat inisialisasi pertama kali dilakukan, menyiapkan tata letak untuk pertama kalinya dengan meng-inflate-nya, dan menginisialisasi variabel.

## Kasus penggunaan 2: Bernavigasi dari dan kembali ke aktivitas
Setelah memulai aplikasi lalu menutupnya dengan sempurna, Anda telah melihat sebagian besar status siklus proses saat aktivitas dibuat untuk pertama kalinya. Anda juga telah melihat semua status siklus proses yang dilewati saat aktivitas tersebut dinonaktifkan dan dimusnahkan. Namun saat pengguna berinteraksi dengan perangkat Android, mereka berpindah antar-aplikasi, kembali ke beranda, memulai aplikasi baru, dan menangani gangguan karena aktivitas lain seperti panggilan telepon.

- Aktivitas Anda tidak benar-benar ditutup setiap kali pengguna keluar dari aktivitas tersebut:
  * Jika aktivitas Anda tidak lagi terlihat di layar, tindakan ini artinya aktivitas tersebut berada di latar belakang. (Kebalikannya adalah saat aktivitas berada di latar depan atau di layar.)
  * Saat pengguna kembali ke aplikasi Anda, aktivitas yang sama akan dimulai ulang dan akan terlihat lagi. Bagian siklus proses ini disebut siklus proses aplikasi yang terlihat.

Jika berada di latar belakang, aplikasi Anda seharusnya tidak aktif berjalan karena ingin mempertahankan resource sistem dan masa pakai baterai. Anda menggunakan siklus proses Activity dan callback untuk mengetahui waktu pemindahan aplikasi ke latar belakang sehingga Anda dapat menjeda operasi yang sedang berlangsung. Kemudian, mulai ulang operasi ketika aplikasi muncul di latar depan.

- Pada langkah ini, Anda melihat siklus proses aktivitas saat aplikasi muncul di latar belakang dan kembali lagi ke latar depan.

  1. Dengan aplikasi DessertClicker masih berjalan, klik cupcake beberapa kali.
  2. Tekan tombol Beranda di perangkat dan amati Logcat di Android Studio. Saat Anda kembali ke layar utama, aplikasi tersebut tidak akan dimatikan, tetapi akan tetap berjalan di latar belakang. Perhatikan bahwa metode onPause() dan metode onStop() dipanggil, tetapi onDestroy() tidak.
```terminal 
2020-10-16 10:41:05.383 22064-22064/com.example.android.dessertclicker D/MainActivity: onPause Called
2020-10-16 10:41:05.966 22064-22064/com.example.android.dessertclicker D/MainActivity: onStop Called
```
Saat onPause() dipanggil, aplikasi tidak lagi memiliki fokus. Setelah onStop(), aplikasi tidak lagi akan terlihat di layar. Meskipun aktivitas telah dihentikan, objek Activity masih ada di memori, yakni di latar belakang. Aktivitas belum dimusnahkan. Pengguna mungkin kembali ke aplikasi. Jadi karena alasan ini, Android menyimpan resource aktivitas 
<p align="center">
<img src="https://developer.android.com/static/codelabs/basic-android-kotlin-training-activity-lifecycle/img/b488b32801220b79.png" width="35%" height="35%"/>
<p/>

3. Gunakan layar terbaru untuk kembali ke aplikasi. Perhatikan di Logcat bahwa aktivitas dimulai ulang dengan onRestart() dan onStart(), lalu dilanjutkan dengan onResume().

```terminal 
2020-10-16 10:42:18.144 22064-22064/com.example.android.dessertclicker D/MainActivity: onRestart Called
2020-10-16 10:42:18.158 22064-22064/com.example.android.dessertclicker D/MainActivity: onStart Called
2020-10-16 10:42:18.158 22064-22064/com.example.android.dessertclicker D/MainActivity: onResume Called
```
Saat aktivitas kembali ke latar depan, metode onCreate() tidak akan dipanggil lagi. Objek aktivitas tidak dimusnahkan sehingga tidak perlu dibuat lagi. Sebagai ganti onCreate(), metode onRestart() dipanggil. Perhatikan saat sekarang aktivitas kembali ke latar depan, jumlah Makanan Penutup yang Terjual tetap dipertahankan.
  
  4. Mulai dengan minimal satu aplikasi selain DessertClicker sehingga ada beberapa aplikasi di layar terbaru perangkat.
  5. Munculkan layar terbaru dan buka aktivitas terbaru lainnya. Kemudian, kembali ke aplikasi terbaru dan pindahkan DessertClicker ke latar depan.

Perhatikan bahwa di sini Anda melihat callback yang juga Anda lihat di Logcat saat Anda menekan tombol Beranda. onPause() dan onStop() dipanggil saat aplikasi berpindah ke latar belakang, lalu onRestart(), onStart(), dan saat aplikasi kembali

> Catatan: Hal yang perlu digarisbawahi disini adalah bahwa onStart() dan onStop() dipanggil beberapa kali saat pengguna masuk dan keluar aktivitas.

Metode ini dipanggil saat aplikasi dihentikan dan dipindahkan ke latar belakang, atau saat aplikasi dimulai lagi ketika kembali ke latar depan. Jika Anda perlu melakukan beberapa pekerjaan di aplikasi selama proses ini, ganti metode callback siklus proses yang relevan.

Jadi, bagaimana dengan onRestart()? Metode onRestart() mirip dengan onCreate(). onCreate() atau onRestart() dipanggil sebelum aktivitas terlihat. Metode onCreate() dipanggil hanya pada saat pertama, dan onRestart() dipanggil setelah itu. Metode onRestart() adalah tempat untuk meletakkan kode yang hanya ingin Anda panggil jika aktivitas tidak dimulai untuk pertama kalinya.

## Kasus penggunaan 3: Menyembunyikan sebagian aktivitas
Anda telah mempelajari bahwa saat aplikasi dimulai dan onStart() dipanggil, aplikasi akan terlihat di layar. Saat aplikasi dilanjutkan dan onResume() dipanggil, pengguna akan berfokus ke aplikasi, yaitu pengguna dapat berinteraksi dengan aplikasi. Bagian siklus proses aplikasi yang menampilkan aplikasi di layar penuh dan mendapatkan fokus pengguna disebut siklus hidup interaktif.

Saat aplikasi berpindah ke latar belakang, fokus akan hilang setelah onPause(), dan aplikasi tidak lagi terlihat setelah onStop().

Perbedaan antara fokus dan visibilitas sangat penting karena ada kemungkinan aktivitas terlihat sebagian di layar, tetapi tidak mendapatkan fokus pengguna. Pada langkah ini, Anda melihat satu kasus saat aktivitas terlihat sebagian, tetapi tidak mendapatkan fokus pengguna.

1. DEngan aplikasi DessertClicker masih berjalan, klik tombol Bagikan dikanan atas layar
2. Aktivitas berbagi muncul di paruh bawah layar, tetapi aktivitas tersebut juga masih terlihat paruh atas.
3. perikasa Logcat dan perhatikan bahwa hanya onPause() yang dipanggil.

```terminal 
2020-10-16 11:00:53.857 22064-22064/com.example.android.dessertclicker D/MainActivity: onPause Called
```

Dalam kasus penggunaan ini, onStop() tidak dipanggil karena aktivitas masih terlihat sebagian. Namun, aktivitas tersebut tidak mendapatkan fokus pengguna, dan pengguna tidak dapat berinteraksi dengannya—aktivitas "berbagi" yang ada di latar depan mendapatkan fokus pengguna.

Mengapa perbedaan ini penting? Gangguan dengan onPause() saja biasanya berlangsung dalam waktu singkat sebelum kembali ke aktivitas atau membuka aktivitas atau aplikasi lain. Anda biasanya ingin terus mengupdate UI sehingga bagian aplikasi lainnya tidak akan berhenti berfungsi.

Kode apa pun yang berjalan di onPause() akan memblokir aplikasi lain agar tidak ditampilkan. Jadi, pastikan kode dalam mode onPause() selalu ringan. Misalnya, jika panggilan telepon masuk, kode dalam onPause() dapat menunda notifikasi panggilan masuk.

4. Klik di luar dialog berbagi untuk kembali ke aplikasi, dan perhatikan bahwa onResume() dipanggil.

Baik onResume() maupun onPause() harus memiliki fokus. Metode onResume() dipanggil saat aktivitas memiliki fokus, dan onPause() dipanggil saat aktivitas kehilangan fokus.

# Mempelajari perubahan konfigurasi
Ada kasus lain dalam mengelola siklus proses aktivitas yang penting untuk dipahami: bagaimana perubahan konfigurasi memengaruhi siklus proses aktivitas Anda.

Perubahan konfigurasi terjadi saat status perangkat berubah secara drastis sehingga cara termudah bagi sistem untuk menyelesaikan perubahan adalah dengan benar-benar menonaktifkan dan membuat ulang aktivitas. Misalnya, jika pengguna mengubah bahasa perangkat, seluruh tata letak mungkin perlu diubah untuk mengakomodasi arah teks dan panjang string yang berbeda. Jika pengguna mencolokkan perangkat ke dok atau menambahkan keyboard fisik, tata letak aplikasi mungkin perlu memanfaatkan ukuran tampilan atau tata letak yang berbeda. Selain itu juga, jika orientasi perangkat berubah—jika perangkat diputar dari mode potret ke mode lanskap atau ke belakang, ataupun sebaliknya—tata letak mungkin perlu diubah agar sesuai dengan orientasi baru. Mari lihat perilaku aplikasi dalam skenario ini.

## Kehilangan data di rotasi perangkat
1. Kompilasi dan jalankan aplikasi Anda, lalu buka Logcat.
2. Memutar perangkat atau emulator ke mode lanskap. Anda dapat memutar emulator ke kiri atau ke kanan dengan tombol rotasi, atau dengan tombol Control dan panah (Command dan tombol panah di Mac).
3. Periksa output di Logcat. Filter outpun di MainActivity.
```terminal 
2020-10-16 11:03:09.618 23206-23206/com.example.android.dessertclicker D/MainActivity: onCreate Called
2020-10-16 11:03:09.806 23206-23206/com.example.android.dessertclicker D/MainActivity: onStart Called
2020-10-16 11:03:09.808 23206-23206/com.example.android.dessertclicker D/MainActivity: onResume Called
2020-10-16 11:03:24.488 23206-23206/com.example.android.dessertclicker D/MainActivity: onPause Called
2020-10-16 11:03:24.490 23206-23206/com.example.android.dessertclicker D/MainActivity: onStop Called
2020-10-16 11:03:24.493 23206-23206/com.example.android.dessertclicker D/MainActivity: onDestroy Called
2020-10-16 11:03:24.520 23206-23206/com.example.android.dessertclicker D/MainActivity: onCreate Called
2020-10-16 11:03:24.569 23206-23206/com.example.android.dessertclicker D/MainActivity: onStart Called
```
Perhatikan bahwa saat perangkat atau emulator memutar layar, sistem akan memanggil semua callback siklus proses untuk menghentikan aktivitas. Kemudian, saat aktivitas dibuat ulang, sistem akan memanggil semua callback siklus proses untuk memulai aktivitas.

4. Saat perangkat diputar dan aktivitas dinonaktifkan serta dibuat ulang, aktivitas akan dimulai dengan nilai default—jumlah makanan penutup yang terjual dan pendapatan telah disetel ulang menjadi nol.

# Menggunakan onSaveInstanceState() untuk menyimpan data paket
Metode onSaveInstanceState() adalah callback yang Anda gunakan untuk menyimpan data yang mungkin diperlukan jika Activity dihancurkan. Dalam diagram callback siklus proses, onSaveInstanceState() akan dipanggil setelah aktivitas dihentikan. Kode tersebut akan dipanggil setiap kali aplikasi Anda masuk ke latar belakang.
<p align="center">
<img src="https://developer.android.com/static/codelabs/basic-android-kotlin-training-activity-lifecycle/img/c259ab6beca0ca88.png" width="35%" height="35%"/>

Anggaplah panggilan onSaveInstanceState() sebagai tindakan keamanan; sehingga Anda dapat menyimpan sejumlah kecil informasi ke paket karena aktivitas Anda di latar depan ditutup. Sistem menyimpan data ini sekarang karena jika menunggu hingga aplikasi Anda dinonaktifkan, sistem dapat berada dalam tekanan resource.

> Catatan: Terkadang Android menghentikan seluruh proses aplikasi yang mencakup setiap aktivitas yang terkait dengan aplikasi. Android melakukan penonaktifan seperti ini saat sistem mengalami gangguan dan berisiko mengalami keterlambatan secara visual sehingga tidak ada panggilan balik atau kode tambahan yang dijalankan saat ini. Proses aplikasi Anda dihentikan di latar belakang tanpa suara. Namun, bagi pengguna, sepertinya aplikasi tidak ditutup. Saat pengguna membuka kembali aplikasi yang telah dinonaktifkan oleh sistem Android, Android akan memulai ulang aplikasi tersebut. Anda dapat memastikan bahwa pengguna tidak mengalami kehilangan data saat hal ini terjadi.

Menyiapkan data secara rutin akan membantu memastikan bahwa data yang diperbarui dalam paket dapat dipulihkan, jika diperlukan.
1. Dalam MainActiivity ganti callback onSavedinstaceState(), dan tambahkan pernyataan log.
```kotlin
override fun onSavedIntaceState(outState: Bundle) {
  super.onSavedIntanceState(outState)
  Log.d(Tag, "onSavedintanceState Called")
}
```
> Catatan: Ada dua penggantian untuk onSaveInstanceState(), pertama hanya dengan parameter outState, dan yang kedua dengan menyertakan parameter outState dan outPersistentState. Gunakan kode yang ditunjukkan dalam kode di atas, dengan parameter outState tunggal.

2. Kompilasi dan jalankan aplikasi, lalu klik tombol Beranda untuk memindahkannya ke latar belakang. Perhatikan bahwa callback onSaveInstanceState() muncul tepat setelah onPause() dan onStop():

```terminal 
2020-10-16 11:05:21.726 23415-23415/com.example.android.dessertclicker D/MainActivity: onPause Called
2020-10-16 11:05:22.382 23415-23415/com.example.android.dessertclicker D/MainActivity: onStop Called
2020-10-16 11:05:22.393 23415-23415/com.example.android.dessertclicker D/MainActivity: onSaveInstanceState Called
```
Di bagian atas file, tepat sebelum definisi class, tambahkan konstanta ini:
```kotlin
const val KEY_REVENUE = "revenue_key"
const val KEY_DESSERT_SOLD = "dessert_sold_key"
```
Anda akan menggunakan kunci ini untuk menyimpan dan mengambil data dari paket status instance.

4. Scroll ke bawah ke onSaveInstanceState(), dan perhatikan parameter outState yang berjenis Bundle.

Bundle adalah kumpulan key-value pair dengan kunci selalu berupa string. Anda dapat memasukkan data sederhana, seperti nilai Int dan Boolean, ke dalam paket. Sistem tersebut menyimpan paket ini dalam memori sehingga cara terbaiknya adalah dengan menjaga data dalam paket tetap berukuran kecil. Ukuran paket ini juga terbatas, meskipun ukurannya bervariasi di setiap perangkat. Jika Anda menyimpan terlalu banyak data, Aplikasi Anda berisiko mengalami error karena kesalahan TransactionTooLargeException. 
5. Dalam onSaveInstanceState(), masukkan nilai revenue (bilangan bulat) ke dalam paket dengan metode putInt():
```kotlin
outState.putInt(KEY_REVENUE, revenue)
```
Metode putInt() (dan metode serupa dari class Bundle seperti putFloat() dan putString() membutuhkan dua argumen: string untuk kunci (konstanta KEY_REVENUE) dan nilai aktual untuk disimpan.

6. Ulangi proses yang sama dengan jumlah makanan penutup yang terjual:
```kotlin
outState.puInt(KEY_DESSERT_SOLD, dessertSold)
```

## Menggunakan onCreate() untuk memulihkan data paket
Status Aktivitas dapat dipulihkan dalam onCreate(Bundle) atau onRestoreInstanceState(Bundle) (metode Bundle yang diisi oleh onSaveInstanceState() akan diteruskan ke kedua metode callback siklus proses).

1. Scroll sampai onCreate(), dan periksa tanda tangan metode:
```kotlin
override fun onCreate(savedInstance: Bundle) {

}
```
Perhatikan bahwa onCreate() akan mendapatkan Bundle setiap kali dipanggil. Saat aktivitas dimulai ulang karena proses penghentian, paket yang Anda simpan akan diteruskan ke onCreate(). Jika aktivitas Anda dimulai kembali dari awal, Bundle dalam onCreate() adalah null. Jadi, jika paket bukan null, Anda tahu bahwa Anda sedang "membuat ulang" aktivitas dari titik yang sebelumnya telah diketahui.

> Catatan: Jika aktivitas dibuat ulang, callback onRestoreInstanceState() akan dipanggil setelah onStart(), beserta paketnya. Biasanya, Anda memulihkan status aktivitas di onCreate(). Akan tetapi, karena onRestoreInstanceState() dipanggil setelah onStart(), dan jika Anda harus memulihkan beberapa status setelah onCreate() dipanggil, Anda dapat menggunakan onRestoreInstanceState().

2. Tambahkan code ini ke onCreate(), tepat setelah variable binding ditetapkan:
```kotlin
if (savedInstanceState != null) {
  revenue = savedInstanceState.getInt(KEY_REVENUE, 0)
}
```
Pengujian null menentukan apakah ada data dalam paket, atau apakah paket tersebut adalah null, yang memberi tahu Anda bahwa aplikasi telah dimulai kembali dari awal atau telah dibuat ulang setelah penghentian. Pengujian ini adalah pola umum untuk memulihkan data dari paket.

Perhatikan bahwa kunci yang Anda gunakan di sini (KEY_REVENUE) adalah kunci yang sama dengan yang Anda gunakan untuk putInt(). Cara untuk memastikan Anda menggunakan kunci yang sama setiap saat adalah dengan mendefinisikan kunci tersebut sebagai konstanta. Anda menggunakan getInt() untuk mengeluarkan data dari paket, sama seperti Anda menggunakan putInt() untuk memasukkan data ke dalam paket. Metode getInt() menggunakan dua argumen:

  * String yang berfungsi sebagai kunci, misalnya "key_revenue" untuk nilai pendapatan.
  * Nilai default akan digunakan jika tidak ada nilai untuk kunci tersebut dalam paket.
Bilangan bulat yang Anda dapatkan dari paket tersebut selanjutnya akan ditetapkan ke variabel revenue, dan UI akan menggunakan nilai tersebut.

3. Tambahkan metode getInt() untuk memulihkan pendapatan dan jumlah makanan penutup yang terjual.
```kotlin
if (savedInstanceState != null) {
  revenue = savedInstaceState.getInt(KEY_REVENUE, 0)
  desertSold = savedInstanceState.getInt(KEY_DESSERT_SOLD, 0)
}
```

4. Kompilasi dan jalankan aplikasi. Tekan cupcake minimal lima kali hingga beralih ke donat.
5. Putar perangkat. Perhatikan bahwa kali ini aplikasi menampilkan nilai pendapatan dan makanan penutup terjual yang benar dari paket. Perhatikan juga makanan penutup telah kembali ke cupcake.
<p align="center">
<img src="https://developer.android.com/static/codelabs/basic-android-kotlin-training-activity-lifecycle/img/4179956182ffc634.png" width="35%" height="35%"/>
<p/>

Ada satu hal lagi yang harus dilakukan untuk memastikan bahwa aplikasi kembali ke keadaan terakhir sebelum mati.
6. Dalam MainActivity, periksa metode showCurrentDessert(). Perhatikan bahwa metode ini menentukan gambar makanan penutup yang akan ditampilkan dalam aktivitas berdasarkan jumlah makanan penutup yang terjual saat ini dan daftar makanan penutup di variabel allDesserts.
```kotlin
var newDessert = allDessert[0]
for (dessert in allDesserts) {
  if (dessertsSold >= dessert.startProductionAmount) {
    newDessert = dessert
  }
  else break
}
```
Metode ini mengandalkan jumlah makanan penutup yang terjual untuk memilih gambar yang tepat. Oleh karena itu, Anda tidak perlu melakukan apa pun untuk menyimpan referensi gambar dalam paket di onSaveInstanceState(). Dalam paket tersebut, Anda sudah menyimpan jumlah makanan penutup yang terjual.

7. Dalam onCreate(), di blok yang memulihkan status dari paket, panggil showCurrentDessert():
```kotlin
if (savedInstanceState != null) {
  revenue = savedInstanceState.getInt(KEY_REVENUE, 0)
  dessertSold = savedInstanceState.getInt(KEY_DESSERT_SOLD, 0)
  showCurrentDessert()
}
```

8. kompilasi dan jalankan aplikasi, lalu putar layar. Perlu diperhatikan bahwa nilai makanan penutup yang terjual, pendapatan total, dan gambar makanan penutup telah dipulihkan dengan benar.

# RINGKASAN 
## **Siklus Proses aktivitas**
  * _siklus proses aktivitas_ (activity process cycle) merupakan kumpulan status tempat aktivitas dimigrasikan. Siklus proses aktivitas dimulai saat aktivitas pertama kali dibuat dan berakhir saat aktivitas dimusnahkan.
  * saat pengguna bernavigasi di antara aktivitas, di dalam dan diluar aplikasi, setiap aktivitas bergerak antar keadaan dalam daur hidup aktivitas (activity lifeCycle)
  * setiap status dalam siklus proses aktivitas memiliki metode callback yang sesuai dan dapat anda ganti di class Activity. Rangkaian inti dari metode siklus proses adalah :
  ```kotlin
  onCreate() onstart() onPause() onRestart() onResume() onStop() onDestroy()
  ```
  * untuk menambahkan prilaku yang terjadi saat proses transisi aktivitas anda ke status siklus proses, ganti metode callback status
  * untuk menambahkan metode pengganti kerangka ke class di Android Studio, pilih Code > Override Methods atau CTRL + O

## **Melakukan Logging dengan Log**
  * saat aplikasi anda berpindah ke latar belakang, tepat setalah onStop() dipanggil, data aplikasi dapat disimpan ke paket. beberapa data aplikasi, seperti konten **EditText**, akan otomatis disimpan untuk anda.
  * paket tersebut adalah instance dari Bundle, yang merupakan kumpulan kunci dan nilai. Kunci selalu berupa String
  * gunakan callback **onSavedInstanceState()** untuk menyimpan data lain ke paket yang ingin anda pertahankan, meskipun aplikasi tersebut dimatikan secara otomatis. untuk memasukan data ke dalam paket, gunakan metode paket yang dimulai dengan **put**, seperti **putInt()**.
  * anda bisa mendapatkan kembali data dari paket dalam metode **onRestoreInstanceState()**, atau yang lebih umum di **onCreate()**. metode **onCreate()** memiliki parameter **savedInstanceState** penyimpanan paket.
  * jika variable **savedInstanceState** adalah **null**, aktivitas akan dimulai tanpa paket status dan tidak ada data status yang dapat diambil.
  * untuk mengambil data dari paket menggunakan kunci, gunakan metode **Bundle** yang dimulai dengan **get** seperti **getInt()**.

## Perubahan konfigurasi
  * *perubahan konfigurasi* terjadi saat status perangkat berubah secara drastis sehingga cara termudah bagi sistem untuk menyelesaikan perubahan adalah dengan memusnahkan ulang aktivitas.
  * contoh paling umum dari perubahan konfigurasi adalah bila pengguna memutar perangkat dari mode potret ke mode landskap, atau sebaliknya. perubahan konfigurasi juga dapat terjadi saat bahasa perangkat berubah atau keyboard hardware dicolokkan.
  * saat terjadi perubahan konfigarsi, android akan memanggil semua callback penonaktifan siklus proses aktivitas. kemudian, Android akan memulai ulang aktivitas dari awal, menjalankan semua callback startup siklus hidup.
  * saat android menutup aplikasi karena perubahan konfigurasi, aplikasi akan memulai ulang aktivitas menggunakan paket status yang tersedia untuk **onCreate()**.
  * seperti halnya penonaktifan proses, simpan status aplikasi anda ke paket di **onSavedInstanceState().**
























































