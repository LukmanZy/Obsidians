di dalam model create method yang bernama scope
example: 
```php
  public function scopeFilter($query, array $filters) {
    // query for search berdasarkan title dan body
    $query->when($filters['search'] ?? false, function ($query, $search) {
      return $query->where('title', 'like', '%'.$search.'%')
          ->orWhere('body', 'like', '%'.$search.'%');
    });
  }
```

kemudian di dalam controller kita panggil method scopeFilter kemudian masukan field search kita masukan kedalam request
```php
  public function index() {
    return view('post', [
      'posts' => Post::latest()->filter(request(['search']))->get()
    ]);
  }
```
lalu kita ingin melakukan searching dengan category dengan path serperti :
* http://127.0.0.1:8000/posts?category=programing&search=fugit
kita tambahkan request nya beserta category
```php
  public function index() {
    return view('post', [
      'posts' => Post::latest()->filter(request(['search', 'category']))->get()
    ]);
  }
```

dan tambahkan juga di query di model untuk category namun disini kita akan join table dengan Category:
```php
  public function scopeFilter($query, array $filters) {
    // query for search berdasarkan title dan body
    $query->when($filters['search'] ?? false, function ($query, $search) {
      return $query->where('title', 'like', '%'.$search.'%')
          ->orWhere('body', 'like', '%'.$search.'%');
    });

    $query->when($filters['category'] ?? false, function ($query, $category) {
      return $query->whereHas('category', function($query) use ($category) {
        $query->where('slug', $category);
      });
    });
  }
```
> note whereHas digunakan untuk join table karena di model Post ada method untuk join table ke category :
```php
  public function category() {
        return $this->belongsTo(Category::class);
    }
```
> namun link yang di view juga harus di ubah karena disini kita meng query nya secara langsung di path yang asal nya: 
```html
  <a href="/categories/{{ $post->category->slug }}" class="text-decoration-none">{{ $posts[0]->category->name }}</a>

```
> menjadi :
```html
  <a href="/posts?category={{ $posts[0]->category->slug }}" class="text-decoration-none">{{ $posts[0]->author->name }}</a>

```