# FRAGMENT

Fragment adalah bagian UI aplikasi yang dapat digunakan kembali. Sebuah fragmen menentukan dan mengelola layout sendiri, memiliki siklus proses sendiri, serta dapat menangani peristiwa inputnya sendiri. Fragment tidak dapat berjalan sendiri, mereka harus dihosting oleh activity atau fragmen lain. Hierarki tampilan fragmen menjadi bagian dari, atau dilampirkan ke, hierarki tampilan host.

## Siklus proses fragmen
Seperti aktivitas, fragmen bisa diinisialisasi dan dihapus dari memori, dan seluruh keberadaannya, muncul, hilang, dan muncul kembali di layar. Selain itu, sama seperti aktivitas, fragmen memiliki siklus proses dengan beberapa keadaan, dan menyediakan beberapa metode yang bisa Anda ganti untuk merespons transisi antar-status. Siklus proses fragmen memiliki lima status yang diwakili oleh enum Lifecycle.State.

  * DIINISIALISASI: Instance fragmen baru telah dibuat instance-nya.
  * DIBUAT: Metode siklus proses fragmen pertama dipanggil. Selama status ini, tampilan yang terkait dengan fragmen juga dibuat.
  * DIMULAI: Fragmen terlihat di layar, tetapi tidak memiliki "fokus", artinya fragmen tidak dapat merespons input pengguna.
  * DILANJUTKAN: Fragmen terlihat dan memiliki fokus.
  * DIHANCURKAN: Objek fragmen telah dinonaktifkan.

> Juga mirip dengan aktivitas, class Fragment menyediakan banyak metode yang dapat Anda ganti untuk merespons peristiwa siklus prose

- Juga mirip dengan aktivitas, class Fragment menyediakan banyak metode yang dapat Anda ganti untuk merespons peristiwa siklus proses.
  * onCreate(): Fragmen telah dibuat instance-nya dan berada dalam status CREATED. Namun, tampilan yang sesuai belum dibuat.
  * onCreateView(): Metode ini adalah tempat Anda meluaskan tata letak. Fragmen telah memasuki status CREATED.
  * onViewCreated(): Ini akan dipanggil setelah tampilan dibuat. Dalam metode ini, Anda biasanya akan mengikat tampilan tertentu ke properti dengan memanggil findViewById().
  * onStart(): Fragmen telah memasuki status STARTED.
  * onResume(): Fragmen telah memasuki status RESUMED dan sekarang memiliki fokus (dapat menanggapi input pengguna).
  * onPause(): Fragmen telah memasuki kembali status STARTED. UI terlihat oleh pengguna
  * onStop(): Fragmen telah memasuki kembali status CREATED. Instance objek telah dibuat, tetapi tidak lagi ditampilkan di layar.
  * onDestroyView(): Dipanggil tepat sebelum fragmen memasuki status DESTROYED. Tampilan telah dihapus dari memori, tetapi objek fragmen masih ada.
  * onDestroy(): Fragmen memasuki status DESTROYED

  Diagram di bawah merangkum siklus proses fragmen dan transisi antar-status.
<p align="center">
<img src="https://developer.android.com/static/codelabs/basic-android-kotlin-training-fragments-navigation-component/img/74470aacefa170bd.png" width="40%" height="40%"/>
<p/>

> Status siklus proses dan metode callback cukup mirip dengan yang digunakan untuk aktivitas. Namun, perhatikan perbedaannya dengan metode onCreate(). Dengan aktivitas, Anda akan menggunakan metode ini untuk mengembangkan layout dan mengikat tampilan. Namun, dalam siklus proses fragmen, onCreate() dipanggil sebelum tampilan dibuat sehingga Anda tidak dapat meng-inflate tata letak di sini. Sebagai gantinya, Anda akan melakukannya di onCreateView(). Kemudian, setelah tampilan dibuat, metode onViewCreated() akan dipanggil dan kemudian Anda dapat mengikat properti ke tampilan tertentu.

Satu-satunya perbedaan yang perlu diperhatikan adalah fragmen bukanlah Context (penghubung) karena sifatnya yang tidak seperti activity, jadi tidak dapat meneruskan this (merujuk ke objek fragmen) sebagai context layout(penghubung layout). Namun, fragmen menyediakan properti context yang dapat digunakan sebagai gantinya.


