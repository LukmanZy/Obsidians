cara membuat resource controller :
```terminal
	php artisan make:controller DashboardPostController --resource
```
cara membuat resource controller langsung terhubung dengan model
```terminal
	php artisan make:controller DashboardPostController --resource --model=Post
```
secara otomatis akan di buatkan method, serta jika membuat resource controller yang terhubung dengan model akan otomatis mengambil model nya :
```php
	  public function index() {
		// untuk menampilkan semua data dengan user tertentu	
	}
	
	public function create() {
		// untuk menampilkan halaman tambah Postingan
	}
	
	public function store(Request) {
		// untuk menjalankan fungsi tambah nya
	}
	
	public function show(Request) {
		// untuk melihat detail dari sebuah postingan
	}
	
	public function edit(Request) {
		// untuk melihat halaman dari ubah data
	}
	
	public function update(Request) {
		// untuk menjalankan proses ubah data nya
	}
	
	public function destroy(Request) {
		// untuk mengahapus data nya
	}
```

------------------------------------
> pada saat kita mengakses sebuah realationship didalam eloquent (belongto, hasMany, dll) maka model nya akan melakukan teknik yang bernama "lazy loaded". artinya data relationship ini tidak di load atau tidak dipanggil sampai akhirnya kita mengakses sebuah property nya pada saat kita looping. Tapi kita bisa mubuat si Eloquent nya membuat "Eiger Load" ketika kita melakukan query kepada parent nya pada saat kita melakukan query di halamannya. jadi nanti dia akan meng query langsung author dan category nya.
> problem example : 
```php
	class Book extends Model{
		public funtion author() {
			return $this->belongTo(Author::class);
		}
	}
```

> klo kita melakukan pengambilan data buku dengan author nya :
```php
	$books = Book::all();
	foreach($books as $book) {
		echo $book->author->name;
	}
```

>jadi jika kita punya 25 buku didalam code akan mengquery 26: 1 query untuk table buku nya dan 25 query untuk buku berdasarkan author nya.
>dengan menggunakan Eiger load kita hanya akan menggunakan 2 query, cara kita tinggal menggunakan method yang bernama with() sebelum kita mengambil data nya.
>jadi example :
```php
	$books = Book::with('author')->get();
	foreach($books as $book) {
		echo $book->author->name;
	}
```

-------------------------------------------------------------------
>kadang-kadang kita butuh melakukan Eiger load pada relationship kita tapi setelah si parent nya di dapatkan. kita akan menggunakan load()
example :
```php
	$book = Book::all();
	if($sameCondition) {
		$book->load('author', 'publisher');
	}

```
---
> in looping we use variable $loop for print number :
 >> $loop->$iteration => for print looping in array number automatic


> Sometimes you may wish to resolve Eloquent Models using a column other than id. To do so, you may specify the column in the route parameter definition :
```php
	Route::get('/post/{post:slug}', function (Post $post){ 
	return $post;
	});
```

>if you wold like model binding to always use a database column other than id when retriving a given model class, you may overide the getRouteKeyName method on the Eloquent model: 
```php
	public function getRouteKeyname() {
	return 'slug';
	}
```
tapi dengan ini membuat semua route kita otomatis mencari dengan slug bukan dengan id.
*******************
karena di route web menggunakan resource maka action yang ada tag form html cukup samakan dengan path url yang di resource :
route web :
```php
	Route::resource('/dashboard/posts', DashboardPostController::class)
		->middleware('auth');
```
di form html :
```html
	<form method="post" action="/dashboard/posts">
```
> jadi klo path url nya di gabung dengan method "post" maka otomatis akan menggunakan method store yang ada di controller nya.
> kalo path url nya digabung dengan method "get" maka otomatis akan menggunakan method index yang ada di controller nya