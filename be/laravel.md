---
name: Laravel 指南
description: Laravel 11+ 開發最佳實踐
---

# 🔴 Laravel 指南

## 專案建立

```bash
composer create-project laravel/laravel my-app
cd my-app
php artisan serve
```

---

## 專案結構

```
app/
├── Http/
│   ├── Controllers/
│   ├── Middleware/
│   └── Requests/      # Form Requests
├── Models/
├── Services/          # 商業邏輯
└── Repositories/      # 資料存取
routes/
├── web.php
└── api.php
resources/
├── views/
└── js/
database/
├── migrations/
└── seeders/
```

---

## 路由

```php
// routes/web.php
Route::get('/', [HomeController::class, 'index']);

// 資源路由
Route::resource('posts', PostController::class);

// API 路由
Route::prefix('api')->group(function () {
    Route::apiResource('users', UserController::class);
});

// 認證路由
Route::middleware('auth')->group(function () {
    Route::get('/dashboard', [DashboardController::class, 'index']);
});
```

---

## Controller

```php
class PostController extends Controller
{
    public function __construct(
        private PostService $postService
    ) {}

    public function index()
    {
        $posts = $this->postService->all();
        return view('posts.index', compact('posts'));
    }

    public function store(StorePostRequest $request)
    {
        $post = $this->postService->create($request->validated());
        return redirect()->route('posts.show', $post);
    }
}
```

---

## Model

```php
class Post extends Model
{
    protected $fillable = ['title', 'content', 'user_id'];

    protected $casts = [
        'published_at' => 'datetime',
    ];

    public function user(): BelongsTo
    {
        return $this->belongsTo(User::class);
    }

    public function tags(): BelongsToMany
    {
        return $this->belongsToMany(Tag::class);
    }
}
```

---

## Form Request

```php
class StorePostRequest extends FormRequest
{
    public function authorize(): bool
    {
        return true;
    }

    public function rules(): array
    {
        return [
            'title' => 'required|string|max:255',
            'content' => 'required|string',
            'tags' => 'array',
            'tags.*' => 'exists:tags,id',
        ];
    }
}
```

---

## 最佳實踐

1. **Service Layer 分離邏輯**
2. **Form Request 驗證**
3. **Resource 格式化 API 回應**
4. **使用 Eloquent 關聯**
5. **Queue 處理耗時任務**
6. **Cache 快取頻繁查詢**
