# Menyiapkan Menu dan Icon

Kini aplikasi Anda telah sepenuhnya dapat dijelajahi dengan menambahkan intent eksplisit dan implisit, dan sekarang saatnya menambahkan opsi menu agar pengguna bisa beralih antara tata letak daftar dan petak untuk huruf tersebut

<p align="center">
<img src="https://developer.android.com/static/codelabs/basic-android-kotlin-training-activities-intents/img/7edb0777b033c159.png" width="20%" height="20%"/>
<img src="https://developer.android.com/static/codelabs/basic-android-kotlin-training-activities-intents/img/ce3474dba2a9c1c8.png" width="20%" height="20%"/>
</p>

Saat ini, Anda mungkin melihat banyak aplikasi memiliki panel ini di bagian atas layar. Inilah yang disebut sebagai panel aplikasi, dan sebagai tambahan untuk menampilkan nama aplikasi, panel aplikasi bisa disesuaikan dan menghosting banyak fungsionalitas yang berguna, seperti pintasan untuk berbagai tindakan penting, atau menu tambahan.
<p align="center"><img src="https://developer.android.com/static/codelabs/basic-android-kotlin-training-activities-intents/img/dfc4095251c1466e.png" width="20%" height="20%"/></p>

> Untuk aplikasi ini, meskipun kami tidak akan menambahkan menu yang lengkap, Anda akan mempelajari cara menambahkan tombol khusus ke panel aplikasi, sehingga pengguna dapat mengubah tata letak.

1. Pertama, Anda harus mengimpor dua ikon untuk mewakili tampilan petak dan daftar. Tambahkan aset vektor gambar klip yang disebut "modul tampilan" (beri nama **ic_grid_layout**) dan "daftar tampilan" (beri nama **ic_linear_layout**). Jika Anda perlu mengingat cara menambahkan ikon material, lihat petunjuk di halaman ini.

<p align="center"><img src="https://developer.android.com/static/codelabs/basic-android-kotlin-training-activities-intents/img/5a01fc03113ac399.png" width="40%" height="40%"/></p>

2. Anda juga memerlukan suatu cara untuk memberi tahu sistem tentang opsi yang ditampilkan di panel aplikasi dan ikon yang digunakan. Untuk melakukannya, tambahkan file resource baru dengan mengklik kanan folder res dan memilih New > Android Resource File. Setel Resource Type ke Menu dan File Name ke layout_menu.

3. Buka res/Menu/layout_menu. Ganti konten layout_menu.xml dengan konten berikut:
```xml
<menu xmlns:android="http://schemas.android.com/apk/res/android"
   xmlns:app="http://schemas.android.com/apk/res-auto">
   <item android:id="@+id/action_switch_layout"
       android:title="@string/action_switch_layout"
       android:icon="@drawable/ic_linear_layout"
       app:showAsAction="always" />
</menu>
```

Struktur file menu tersebut cukup sederhana. Sama seperti tata letak yang dimulai dengan pengelola tata letak untuk menyimpan tampilan individu, file xml menu dimulai dengan tag menu yang berisi opsi individual.

- Menu Anda hanya memiliki satu tombol, tetapi memiliki beberapa properti:
  * id: Seperti tampilan, opsi menu memiliki ID sehingga dapat dirujuk dalam kode.
  * title: Dalam kasus Anda, teks ini tidak akan benar-benar terlihat, tetapi mungkin berguna bagi pembaca layar untuk mengidentifikasi menu
  * icon: Properti default-nya adalah ic_linear_layout. Namun, tombol ini akan diaktifkan dan dinonaktifkan untuk menampilkan ikon petak saat tombol dipilih.
  * showAsAction: Properti ini memberi tahu sistem cara menampilkan tombol. Karena disetel ke always (selalu), tombol ini akan selalu terlihat di panel aplikasi, dan tidak menjadi bagian dari menu tambahan.

Tentu saja, hanya memiliki kumpulan properti ini bukan berarti menu akan benar-benar melakukan semuanya.
Anda tetap perlu menambahkan beberapa kode di MainActivity.kt agar menu berfungsi.


## Mengimplementasikan tombol Menu
Untuk melihat cara kerja tombol menu, ada beberapa hal yang dapat dilakukan di MainActivity.kt.

1. Pertama, sebaiknya buat properti untuk melacak status tata letak tempat aplikasi berada. Cara ini akan memudahkan tombol beralih tata letak. Tetapkan nilai default ke true karena pengelola tata letak linear akan digunakan secara default.
```kotlin
private var isLinearLayoutManager = true 
```

2. Saat pengguna mengalihkan tombol, Anda ingin daftar item berubah menjadi petak item. Jika Anda ingat pelajaran tampilan recycler, ada banyak pengelola tata letak yang berbeda, salah satunya GridLayoutManager yang memungkinkan beberapa item di satu baris.
```kotlin
private fun chooseLayout() {
  if(isLinearLayoutManager) {
    recyclerView.layoutManager = LinearLayoutManager(this)
  } else {
    recyclerView.layoutManager = GridLayoutManager(this, 4)
  } 
  recyclerview.adapter = LetterAdapter()
}
```

> Di sini, Anda menggunakan pernyataan if untuk menetapkan pengelola tata letak. Selain menyetel layoutManager, kode ini juga menetapkan adaptor. LetterAdapter digunakan untuk tata letak list layout dan grid layout.

3. Saat pertama kali menyiapkan menu dalam xml, Anda memberikannya ikon statis. Akan tetapi, setelah mengalihkan tata letak, Anda harus memperbarui ikon untuk mencerminkan fungsi barunya dengan beralih kembali ke tata letak daftar. Cukup setel ikon tata letak linear dan petak, dan berdasarkan tata letak, tombol akan beralih kembali saat Anda mengetuknya di lain waktu.
```kotlin
private fun setIcon(menuItem: MenuItem?) {
  if (menuItem == null)
  return
   // Set the drawable for the menu icon based on which LayoutManager is currently in use

   // An if-clause can be used on the right side of an assignment if all paths return a value.
   // The following code is equivalent to
   // if (isLinearLayoutManager)
   //     menu.icon = ContextCompat.getDrawable(this, R.drawable.ic_grid_layout)
   // else menu.icon = ContextCompat.getDrawable(this, R.drawable.ic_linear_layout)
   menuItem.icon = 
   if (isLinearLayoutManager)
   ContextCompat.getDrawable(this, R.drawable.ic_grid_layout)
   else ContextCompat.getDrawable(this, R.drawable.ic_linear_layout)
}
```

Ikon ditetapkan dengan syarat berdasarkan properti isLinearLayoutManager.

- Agar aplikasi benar-benar menggunakan menu tersebut, Anda perlu mengganti dua metode lainnya.
  * onCreateOptionsMenu: tempat Anda meng-inflate menu opsi dan melakukan penyiapan tambahan
  * onOptionsItemSelected: tempat Anda akan memanggil chooseLayout() saat tombol dipilih.
1. Ganti onCreateOptionsMenu sebagai berikut:
```kotlin
override fun onCreateOptionMenu(menu: Menu?): Boolean {
  menuInflater.infalte(R.menu.layout_menu, menu)

  val layoutBotton = menu?.findItem(R.id.action_switch_layout)
  // Calls code to set the icon based on the LinearLayoutManager of the RecyclerView
  setIcon(layoutButton)

  return true
}
```

Semuanya biasa saja di sini. Setelah meng-inflate tata letak, panggil setIcon() untuk memastikan ikon sudah benar berdasarkan tata letaknya. Metode ini akan menampilkan Boolean—Anda menampilkan true di sini karena ingin membuat menu opsi.

2. Terapkan seperti yang ditunjukkan onOptionsItemSelected hanya dengan beberapa baris kode.
```kotlin
override fun onOptionSelected(item: MenuItem): Boolean {
  return when (item.itemId) {
    R.id.action_switch_layout -> {
      // Sets isLinearLayoutManager (a Boolean) to the opposite value
      isLinearLayoutManage = !isLinearLayoutManager
      // set layout and icon
      chooseLayout()
      setIcon(item)

      return true
    }
    //  Otherwise, do nothing and use the core event handling

       // when clauses require that all possible paths be accounted for explicitly,
       //  for instance both the true and false cases if the value is a Boolean,
       //  or an else to catch all unhandled cases.
    else -> super.onOptionSelected(item)
  }
}
```

Tata letak ini akan dipanggil setiap kali item menu diketuk sehingga Anda harus memastikan item menu mana yang diketuk. Anda menggunakan pernyataan when di atas. Jika id cocok dengan item menu action_switch_layout, Anda akan meniadakan nilai isLinearLayoutManager. Kemudian, panggil chooseLayout() dan setIcon() untuk mengupdate UI yang sesuai.

Satu hal lagi, sebelum Anda menjalankan aplikasi. Anda harus mengganti kode tersebut di onCreate() untuk memanggil metode baru karena pengelola tata letak dan adaptor sekarang ditetapkan dalam chooseLayout(). onCreate() akan terlihat seperti berikut setelah diubah.
```kotlin
override fun onCreate(savedInstanceState: Bundle?) {
   super.onCreate(savedInstanceState)

   val binding = ActivityMainBinding.inflate(layoutInflater)
   setContentView(binding.root)

   recyclerView = binding.recyclerView
   // Sets the LinearLayoutManager of the recyclerview
   chooseLayout()
}
```
Sekarang jalankan aplikasi dan Anda dapat beralih antara tampilan daftar dan petak menggunakan tombol menu.

# Ringkasan
Intent eksplisit digunakan untuk membuka aktivitas tertentu di aplikasi Anda.
Intent implisit berkaitan dengan tindakan tertentu (seperti membuka link atau berbagi gambar) dan memungkinkan sistem menentukan cara melaksanakan intent.
Dengan opsi menu, Anda dapat menambahkan tombol dan menu ke panel aplikasi.
Objek pendamping menyediakan cara untuk menghubungkan konstanta yang dapat digunakan kembali dengan jenis, bukan dengan instance dari jenis itu.
Untuk menjalankan intent:

Dapatkan referensi untuk konteks.
Buat objek Intent yang memberikan aktivitas atau jenis intent (bergantung pada jenis objek, apakah eksplisit atau implisit).
Teruskan semua data yang diperlukan dengan memanggil putExtra().
Panggil startActivity() yang meneruskan objek intent.





























