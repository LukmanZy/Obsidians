# Aktivity dan Intent

* intent eksplisit digunakan untuk membuka aktivitas tertentu di aplikasi anda.

* Intent implisit berkaitan dengan dengan tindakan tertentu (seperti membuka link atau berbagi gambar) dan memungkinkan sistem menentukan cara melaksanakan intent.

untuk tindakan yang tidak memerlukan aplikasi saat ini—misalnya, Anda menemukan halaman dokumentasi Android yang menarik dan ingin membagikannya dengan teman—Anda akan menggunakan intent implisit. Anda mungkin melihat menu seperti di bawah ini yang menanyakan aplikasi mana yang akan digunakan untuk membagikan halaman.

Anda menggunakan intent eksplisit untuk melakukan tindakan atau menampilkan layar di aplikasi Anda sendiri dan bertanggung jawab atas keseluruhan proses. Anda biasanya menggunakan intent implisit untuk melakukan tindakan yang melibatkan aplikasi lain dan mengandalkan sistem untuk menentukan hasil akhirnya. Anda akan menggunakan kedua jenis intent di aplikasi Words.
<p align="center">
<img src ="https://developer.android.com/static/codelabs/basic-android-kotlin-training-activities-intents/img/702236c6e2276f91.png" width="70%" height="70%"/>
</p>

## MENYAPKAN INTENT EKSPLISIT
Saatnya menerapkan intent pertama Anda. Di layar pertama, saat mengetuk huruf, pengguna akan diarahkan ke layar kedua yang berisi daftar kata. DetailActivity sudah diterapkan sehingga Anda hanya perlu meluncurkannya dengan intent. Anda menggunakan intent eksplisit karena aplikasi Anda tahu persis aktivitas mana yang harus diluncurkan.

- Membuat dan menggunakan intent hanya memerlukan beberapa langkah:

  * Buka LetterAdapter.kt dan scroll ke bawah ke onBindViewHolder(). Di bawah baris untuk menetapkan teks tombol, tetapkan ke onClickListener untuk holder.button.
  ```kotlin 
  holder.button.setOnClickListener {

  }
  ```
  
  * Kemudian, dapatkan referensi ke context.
  ```kotlin
  val context = holder.itemView.context
  ```
  
  * buat Intent yang akan meneruskan konteks dan nama class aktivitas tujuan. Nama aktivitas yang ingin ditampilkan ditentukan dengan DetailActivity::class.java. Objek DetailActivity sebenarnya dibuat di belakang layar.
  ```kotlin
  val intent = Intent(context, DetailActivity::class.java)
  ```
  
  * Panggil metode putExtra dengan memasukkan "letter" sebagai argumen pertama dan button text sebagai argumen kedua.
  ```kotlin
  intent.putExtra("letter", holder.button.text.toString())
  ```

> Apa yang dimaksud tambahan? Ingat bahwa intent hanyalah serangkaian petunjuk dan belum ada instance aktivitas tujuan. Sebaliknya, tambahan adalah potongan data, seperti angka atau string, yang diberi nama untuk diambil nanti. Hal ini mirip dengan meneruskan argumen saat Anda memanggil fungsi. Anda harus memberi tahu huruf yang akan ditampilkan karena DetailActivity dapat ditampilkan untuk setiap huruf
Selain itu, menurut Anda mengapa perlu memanggil toString()? Teks tombol sudah berupa string, bukan?
Sepertinya saya lumayan paham. Ini sebenarnya dari jenis CharSequence yang disebut antarmuka. Anda tidak harus memahami antarmuka Kotlin untuk saat ini, selain bahwa cara ini digunakan untuk memastikan bahwa jenis, seperti String, menerapkan fungsi dan properti tertentu. Anda dapat menganggap CharSequence sebagai representasi yang lebih umum dari class yang seperti string. Properti text tombol bisa berupa string, atau bisa juga objek yang juga merupakan CharSequence. Namun, metode putExtra() menerima String, bukan hanya CharSequence, sehingga perlu memanggil toString().

  * Panggil metode startActivity()  pada objek konteks, dengan meneruskan intent.
  ```kotlin
  context.startActivity(intent)
  ```

## MENYAPKAN DETAILACTIVITY
<strong><em>Anda baru saja membuat intent eksplisit Anda yang pertama. Sekarang ke layar detail.</em></strong>

- Di metode onCreate DetailActivity, setelah panggilan ke setContentView, ganti huruf hard code dengan kode untuk mendapatkan letterId yang diteruskan dari intent.
  * Ada banyak hal terjadi di sini, jadi mari kita lihat setiap bagian:
  * Pertama, dari mana properti intent berasal? Ini bukan properti dari DetailActivity, melainkan properti yang dimiliki oleh setiap aktivitas. Properti ini menyimpan referensi ke intent yang digunakan untuk meluncurkan aktivitas.
   ```kotlin
    val letterId = intent?.extra?.getString("letter").toString()
  ```
  > Properti tambahan memiliki jenis Bundle, dan seperti yang mungkin sudah Anda duga, properti ini menyediakan cara untuk mengakses semua tambahan yang diteruskan ke intent.
  Kedua properti ini ditandai dengan tanda tanya. Mengapa seperti itu? Alasannya karena properti intent dan extras adalah nullable, artinya properti tersebut mungkin memiliki nilai ataupun tidak. Terkadang Anda mungkin menginginkan variabel menjadi null. Properti intent mungkin sebenarnya bukan Intent (jika aktivitas tidak diluncurkan dari intent) dan properti tambahan mungkin sebenarnya bukan Bundle, melainkan nilai yang disebut null. Di Kotlin, null berarti tidak ada nilai. Objek mungkin ada atau mungkin berupa null. Jika aplikasi Anda mencoba mengakses properti atau memanggil fungsi di objek null, aplikasi akan mengalami error. Untuk mengakses nilai ini dengan aman, beri tanda ? setelah nama. Jika intent bernilai null, aplikasi Anda bahkan tidak akan dapat mengakses properti tambahan, dan jika extras bernilai null, kode Anda bahkan tidak akan mencoba memanggil getString().

Bagaimana cara mengetahui properti yang memerlukan tanda tanya untuk memastikan keamanan null? Anda dapat mengetahui apakah nama jenis diikuti dengan tanda tanya atau tanda seru.

<p align="center">
<img src="https://developer.android.com/static/codelabs/basic-android-kotlin-training-activities-intents/img/b43155b06a5556e.png"/>
</p>

Hal terakhir yang perlu diperhatikan adalah huruf yang sebenarnya diambil dengan getString yang menampilkan String?. Jadi, toString() dipanggil untuk memastikan hurufnya adalah String, bukan null.

Sekarang, saat menjalankan aplikasi dan membuka layar detail, Anda akan melihat daftar kata untuk setiap huruf.

<p align="center">
<img src="https://developer.android.com/static/codelabs/basic-android-kotlin-training-activities-intents/img/c465ef280fe3792a.png"/>
</p>

## Membersihkan
> Kedua kode digunakan untuk melakukan intent, dan mengambil hard code huruf yang dipilih yang bernama extra, "letter". Meskipun sesuai untuk contoh kecil ini, ini bukan pendekatan terbaik untuk aplikasi besar yang memiliki lebih banyak tambahan intent untuk dilacak.

> Meskipun Anda bisa membuat konstanta yang disebut "letter", konstanta ini bisa menjadi tidak stabil karena penambahan intent tambahan ke aplikasi. Di class manakah Anda akan menempatkan konstanta ini? Ingat bahwa string tersebut digunakan dalam DetailActivity dan MainActivity. Anda memerlukan cara untuk menentukan konstanta yang dapat digunakan di beberapa class sembari menjaga kode Anda tetap teratur.

> Untungnya, ada fitur Kotlin yang bermanfaat serta dapat digunakan untuk memisahkan konstanta Anda dan menggunakannya tanpa instance tertentu dari class yang disebut objek pendamping. Objek pendamping serupa dengan objek lain, seperti instance class. Namun, hanya satu instance objek pendamping yang akan ada selama durasi program Anda. Itulah sebabnya instance objek ini terkadang disebut pola singleton. Meskipun ada banyak kasus penggunaan untuk singleton di luar cakupan codelab ini, untuk saat ini, Anda akan menggunakan objek pendamping untuk mengatur konstanta dan memungkinkannya diakses di luar DetailActivity. Anda akan mulai dengan menggunakan objek pendamping untuk memfaktorkan ulang kode untuk tambahan "letter".

* Di DetailActivity, tepat di atas onCreate, tambahkan baris berikut:
 > Ini mirip dengan cara menentukan class, kecuali Anda menggunakan kata kunci object. Ada juga kata kunci companion yang berarti kata kunci tersebut berkaitan dengan class DetailActivity, dan kita tidak perlu memberinya nama jenis yang terpisah.

* Dalam tanda kurung kurawal, tambahkan properti untuk konstanta huruf.
```kotlin
companion object {
  const val LETTER = "letter"
}
```
* Untuk menggunakan konstanta baru, perbarui panggilan letter hard code di onCreate() seperti berikut:
```kotlin
val letterId = intent?.extras?.getString(LETTER).toString()
```

> Sekali lagi, perhatikan bahwa Anda mereferensikannya dengan notasi titik seperti biasa, tetapi konstantanya adalah milik DetailActivity.

* Beralih ke LetterAdapter, dan ubah panggilan ke putExtra untuk menggunakan konstanta baru.
```kotlin
inetent.putExtra(DetailActivity.LETTER, holder.button.text.toString())
```

Semua siap. Dengan memfaktorkan ulang, kode menjadi lebih mudah dibaca dan dikelola. Jika ini, atau konstanta lainnya yang Anda tambahkan, perlu diubah, Anda hanya perlu melakukannya di satu tempat.

Untuk mempelajari objek pendamping lebih lanjut, baca dokumentasi Kotlin tentang Ekspresi dan Deklarasi Objek.


## Menyiapkan Intent Implisit
Pada umumnya, Anda akan menampilkan aktivitas tertentu dari aplikasi Anda sendiri. Namun, terkadang mungkin saja Anda tidak mengetahui aktivitas atau aplikasi yang ingin diluncurkan. Pada layar detail, setiap kata merupakan tombol yang akan menampilkan definisi kata dari pengguna.

Untuk contoh, Anda akan menggunakan fungsi kamus yang disediakan oleh penelusuran Google. Anda tidak akan menambahkan aktivitas baru ke aplikasi, melainkan akan meluncurkan browser perangkat untuk menampilkan halaman penelusuran.

Jadi, mungkin Anda perlu intent untuk memuat halaman di Chrome yang merupakan browser default di Android?

Kurang tepat.

Bisa jadi beberapa pengguna lebih menyukai browser pihak ketiga. Atau, ponsel mereka dilengkapi dengan browser yang telah diinstal lebih dulu oleh produsen. Mungkin mereka telah menginstal aplikasi Google Penelusuran, atau bahkan aplikasi kamus pihak ketiga.


Anda tidak dapat mengetahui dengan pasti aplikasi yang telah diinstal pengguna. Anda juga tidak dapat mengasumsikan cara mereka ingin mencari kata. Ini adalah contoh waktu penggunaan intent implisit yang sempurna. Aplikasi Anda memberikan informasi tentang tindakan yang seharusnya dilakukan ke sistem, dan sistem mengetahui apa yang harus dilakukan dengan tindakan tersebut, sehingga meminta informasi tambahan dari pengguna, jika diperlukan.

- Lakukan hal berikut untuk membuat intent implisit:
  * Untuk aplikasi ini, Anda akan melakukan penelusuran di Google. Hasil penelusuran pertama merupakan definisi kamus dari kata tersebut. Karena URL dasar yang sama digunakan untuk setiap penelusuran, sebaiknya Anda mendefinisikan ini sebagai konstantanya sendiri. Di DetailActivity, ubah objek pendamping untuk menambahkan konstanta baru, SEARCH_PREFIX. Berikut adalah URL dasar untuk penelusuran Google.
  ```kotlin
    companion object {
      const val LETTER = "letter"
      const val SEARCH_PREVIX = "https://www.google.com/search?q="
    }
  ```
  * Lalu, buka WordAdapter dan dengan metode onBindViewHolder(), panggil setOnClickListener() di tombol. Mulai dengan membuat URI untuk kueri penelusuran. Saat memanggil parse() untuk membuat URI dari String, Anda harus menggunakan pemformatan string sehingga kata tersebut ditambahkan ke SEARCH_PREFIX.

  ```kotlin
    holder.button.setOnClickListener {
      val queryUrl: Uri = Uri.parse("${DetailActivity.SEARCH_PRIFIX}${item}")
    }
  ```

  > Mungkin Anda belum tahu apa itu URI, dan ini tidak salah ketik, tetapi merupakan singkatan dari Uniform Resource Identifier. Anda mungkin sudah mengetahui bahwa URL, atau Uniform Resource Locator, adalah suatu string yang mengarah ke halaman web. URI adalah istilah yang lebih umum untuk format tersebut. Semua URL adalah URI, tetapi tidak semua URI adalah URL. URI lain, misalnya, alamat untuk nomor telepon, akan dimulai dengan tel:, tetapi ini dianggap sebagai URN atau Uniform Resource Name, bukan URL. Jenis data yang digunakan untuk mewakili keduanya disebut URI.
  <p align="center">
  <img src="https://developer.android.com/static/codelabs/basic-android-kotlin-training-activities-intents/img/828cef3fdcfdaed.png" width="70%" height="70%"/>
  </p>

  > Perhatikan bahwa tidak ada referensi ke aktivitas apa pun dalam aplikasi Anda di sini. Anda hanya memberikan URI, tanpa indikasi cara menggunakannya.

    * Setelah menentukan queryUrl, inisialisasi objek intent baru:
    ```kotlin
    val intent = Intent(Intent.ACTION_VIEW, queryUri)
    ```

  > Anda tidak akan menyampaikan dalam konteks dan aktivitas, melainkan akan meneruskan Intent.ACTION_VIEW bersama dengan URI.
  ACTION_VIEW adalah intent umum yang menggunakan URI, dan dalam hal ini yang digunakan ialah alamat web. Sistem akan mengetahui cara memproses intent ini dengan membuka URI di browser web pengguna. Beberapa jenis intent lainnya meliputi:
  * CATEGORY_APP_MAPS - meluncurkan aplikasi peta
  * CATEGORY_APP_EMAIL - meluncurkan aplikasi email
  * CATEGORY_APP_GALLERY - meluncurkan aplikasi galeri (foto)
  * ACTION_SET_ALARM - menyetel alarm di latar belakang
  * ACTION_DIAL – melakukan panggilan telepon

Untuk mempelajari lebih lanjut, buka dokumentasi untuk beberapa intent yang umum digunakan.

  * Terakhir, meskipun tidak meluncurkan aktivitas tertentu di aplikasi, Anda memberi tahu sistem untuk meluncurkan aplikasi lain, dengan memanggil startActivity() dan meneruskan intent.
  ```kotlin
  context.startActivity(intent)
  ```

  Sekarang, saat Anda meluncurkan aplikasi, membuka daftar kata, dan mengetuk salah satu kata, perangkat Anda harus membuka URL (atau menampilkan daftar opsi bergantung pada aplikasi yang terinstal).

Perilaku yang tepat akan berbeda-beda di antara pengguna sehingga semua orang akan mendapatkan pengalaman yang lancar tanpa mempersulit kode Anda.

































