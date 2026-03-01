# 🏗️ Architecture & Data Flow Diagram

## System Architecture

```
┌─────────────────────────────────────────────────────────────────┐
│                        USER BROWSER                             │
│                    (http://localhost:8000)                      │
└────────────────────────────┬────────────────────────────────────┘
                             │
                   HTTP Request/Response
                             │
┌────────────────────────────▼────────────────────────────────────┐
│                      LARAVEL ROUTES                             │
│  routes/web.php - Resource Routes for Categories & Products    │
└────────────────────────────┬────────────────────────────────────┘
                             │
        ┌────────────────────┴────────────────────┐
        │                                         │
┌───────▼──────────────┐            ┌────────────▼──────────────┐
│ CategoryController   │            │  ProductController        │
│  - index()          │            │  - index() ✨ TABLE       │
│  - create()         │            │  - create()               │
│  - store()          │            │  - store()                │
│  - show()           │            │  - show()                 │
│  - edit()           │            │  - edit()                 │
│  - update()         │            │  - update()               │
│  - destroy()        │            │  - destroy()              │
└───────┬──────────────┘            └────────────┬──────────────┘
        │                                        │
        │ Uses Model                             │ Uses Model
        │                                        │
┌───────▼──────────────┐            ┌────────────▼──────────────┐
│  Category Model      │            │  Product Model            │
│  - $fillable         │            │  - $fillable              │
│  - products()        │            │  - category()             │
│  - hasMany()         │            │  - belongsTo()            │
└───────┬──────────────┘            └────────────┬──────────────┘
        │                                        │
        │ Database Query                         │ Database Query
        │                                        │
┌───────▼────────────────────────────────────────▼──────────────┐
│                      DATABASE (MySQL)                         │
│  ┌──────────────────────┐    ┌──────────────────────┐        │
│  │   categories TABLE   │    │   products TABLE     │        │
│  ├──────────────────────┤    ├──────────────────────┤        │
│  │ id (PK)             │    │ id (PK)             │        │
│  │ name                │    │ name                │        │
│  │ description         │    │ description         │        │
│  │ created_at          │    │ price               │        │
│  │ updated_at          │    │ quantity            │        │
│  │                     │    │ category_id (FK)────┼────────┤
│  └──────────────────────┘    │ created_at          │        │
│                              │ updated_at          │        │
│                              └──────────────────────┘        │
└──────────────────────────────────────────────────────────────┘
```

---

## Request/Response Flow - Products Page

```
USER VISITS: http://localhost:8000/products
        │
        ▼
┌──────────────────────────┐
│ ROUTE: GET /products     │
│ Mapped to:               │
│ ProductController@index  │
└────────────┬─────────────┘
             │
             ▼
┌──────────────────────────────────────────────────┐
│ CONTROLLER METHOD: index()                       │
│                                                  │
│ $products = Product::with('category')            │
│                          .paginate(10);          │
│                                                  │
│ return view('products.index',                    │
│              compact('products'));               │
└────────────┬─────────────────────────────────────┘
             │
             ▼
┌──────────────────────────────────────────────────┐
│ DATABASE QUERY                                   │
│                                                  │
│ SELECT * FROM products                          │
│ JOIN categories ON products.category_id =       │
│                   categories.id                 │
│ LIMIT 10                                        │
└────────────┬─────────────────────────────────────┘
             │
             ▼
┌──────────────────────────────────────────────────┐
│ BLADE TEMPLATE: products/index.blade.php        │
│                                                  │
│ Renders:                                        │
│ ┌──────────────────────────────────────┐        │
│ │ TABLE HEADER                         │        │
│ │ ID│Name│Category│Price│Qty│Actions │        │
│ ├──────────────────────────────────────┤        │
│ │ 1│Laptop│Electronics│$999│5│Edit   │        │
│ │ 2│Phone│Electronics│$599│3│Edit   │        │
│ │ 3│Book│Media│$29│10│Edit   │        │
│ └──────────────────────────────────────┘        │
│ PAGINATION:                                     │
│ [1] [2] [Next]                                 │
└────────────┬─────────────────────────────────────┘
             │
             ▼
┌──────────────────────────────────────┐
│ HTML RESPONSE SENT TO BROWSER        │
│ (Bootstrap 5.3 Styled)               │
└──────────────────────────────────────┘
```

---

## Database Relationships

```
┌────────────────────────────┐
│      CATEGORIES            │
├────────────────────────────┤
│ id (PK)                    │
│ name                       │
│ description                │
│ created_at                 │
│ updated_at                 │
└────────────────────────────┘
          │ 1
          │
          │ has Many
          │
          │ many
          ▼
┌────────────────────────────┐
│      PRODUCTS              │
├────────────────────────────┤
│ id (PK)                    │
│ name                       │
│ description                │
│ price                      │
│ quantity                   │
│ category_id (FK) ◄─────┐   │
│ created_at             │   │
│ updated_at             │   │
└────────────────────────┼───┘
                         │
                    References
```

**Eloquent Relationships:**
```php
// In Category Model:
public function products() {
    return $this->hasMany(Product::class);
}

// In Product Model:
public function category() {
    return $this->belongsTo(Category::class);
}

// Usage:
$category = Category::find(1);
$category->products;  // Get all products in category

$product = Product::find(1);
$product->category;   // Get product's category
```

---

## View Hierarchy

```
┌─ layouts/app.blade.php (Master Layout)
│  ├─ navbar (with category/product links)
│  ├─ error display section
│  ├─ content @yield
│  └─ footer
│
├─ categories/
│  ├─ index.blade.php (extends app.blade.php)
│  │  └─ Lists all categories in table
│  ├─ create.blade.php (extends app.blade.php)
│  │  └─ Form for new category
│  ├─ show.blade.php (extends app.blade.php)
│  │  └─ Category details
│  └─ edit.blade.php (extends app.blade.php)
│     └─ Form to edit category
│
└─ products/
   ├─ index.blade.php (extends app.blade.php) ✨
   │  └─ TABLE with all products
   ├─ create.blade.php (extends app.blade.php)
   │  └─ Form for new product
   ├─ show.blade.php (extends app.blade.php)
   │  └─ Product details
   └─ edit.blade.php (extends app.blade.php)
      └─ Form to edit product
```

---

## Products Table Structure

```
┌──────────────────────────────────────────────────────────────────────┐
│ PRODUCTS TABLE VIEW (products/index.blade.php)                       │
├──────────────────────────────────────────────────────────────────────┤
│ ID │ Name    │ Category    │ Description    │ Price  │ Qty │ Actions│
├──────────────────────────────────────────────────────────────────────┤
│ 1  │ Laptop  │ Electronics │ High-perf...   │ $999   │ 5   │ View... │
│ 2  │ Mouse   │ Electronics │ Wireless...    │ $29    │ 10  │ View... │
│ 3  │ Book    │ Media       │ Fiction no...  │ $15    │ 0   │ View... │
│ 4  │ Headset │ Electronics │ Studio qual... │ $199   │ 3   │ View... │
└──────────────────────────────────────────────────────────────────────┘

Legend:
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

ID Column
  └─ Product ID from database

Name Column
  └─ Product name (bold)

Category Column
  └─ Category name in blue "info" badge

Description Column
  └─ First 50 characters of description with "..."

Price Column
  └─ Formatted as currency: $X.XX

Qty Column
  └─ Stock quantity with badge
     • Green badge "X in stock" if qty > 0
     • Red badge "Out of Stock" if qty = 0

Actions Column
  └─ Three buttons per row:
     • View (info) - Go to product detail page
     • Edit (warning) - Edit product details
     • Delete (danger) - Remove product (confirmation required)

Pagination
  └─ Shows 10 items per page
     Navigation links: [1] [2] [3] [Next]
```

---

## Form Validation Flow

```
USER SUBMITS PRODUCT FORM
        │
        ▼
┌──────────────────────────────────────────────┐
│ Client-Side Validation (HTML5)               │
│ ✓ Required fields checked                    │
│ ✓ Number type validation                     │
│ ✓ Email format validation (if applicable)    │
└──────────────────────────────────────────────┘
        │
        ▼ (if valid, submit)
        │
┌──────────────────────────────────────────────┐
│ Server Receives: POST /products              │
│ (ProductController@store)                    │
└──────────────────────────────────────────────┘
        │
        ▼
┌──────────────────────────────────────────────┐
│ Server-Side Validation                       │
│                                              │
│ $request->validate([                        │
│   'name' => 'required|string|max:255',      │
│   'price' => 'required|numeric|min:0',      │
│   'quantity' => 'required|integer|min:0',   │
│   'category_id' => 'required|exists:...'    │
│ ]);                                          │
└──────────────────────────────────────────────┘
        │
    ┌───┴────┐
    │        │
    ▼        ▼
PASS     FAIL
    │        │
    │        └──────────────────────┐
    │                               │
    ▼                               ▼
SAVE TO DB                  REDIRECT BACK
    │                               │
    │                               ▼
    │                       SHOW ERROR MESSAGES
    │                       (old form values kept)
    │
    ▼
REDIRECT TO LIST
    │
    ▼
SHOW SUCCESS MESSAGE
```

---

## Middleware & Security Flow

```
REQUEST ARRIVES
    │
    ▼
┌──────────────────────────────────────────┐
│ Kernel Middleware Stack                  │
│ ├─ VerifyCsrfToken                      │
│ ├─ Authenticate (if needed)             │
│ ├─ TrimStrings                          │
│ └─ ConvertEmptyStringsToNull            │
└──────────────────────────────────────────┘
    │
    ▼ (if all pass)
    │
┌──────────────────────────────────────────┐
│ Route Matching & Dispatch                │
│ to appropriate Controller@method          │
└──────────────────────────────────────────┘
    │
    ▼
┌──────────────────────────────────────────┐
│ Controller Execution                     │
│ (CategoryController or ProductController)│
└──────────────────────────────────────────┘
    │
    ▼
┌──────────────────────────────────────────┐
│ Response Generation                      │
│ (View render or redirect)                │
└──────────────────────────────────────────┘
```

---

## File Dependencies

```
routes/web.php
  ├─ uses→ CategoryController
  │          ├─ uses→ Category Model
  │          │          └─ queries→ categories table
  │          ├─ renders→ categories/index.blade.php
  │          ├─ renders→ categories/create.blade.php
  │          └─ etc...
  │
  └─ uses→ ProductController
             ├─ uses→ Product Model
             │          └─ queries→ products table
             ├─ uses→ Category Model
             │          └─ for dropdown in create form
             ├─ renders→ products/index.blade.php ✨
             ├─ renders→ products/create.blade.php
             └─ etc...

All Blade templates
  └─ extends→ layouts/app.blade.php
                ├─ includes→ Bootstrap 5.3
                ├─ includes→ Font Awesome
                └─ styles→ custom CSS
```

---

## Complete Request Examples

### Example 1: View All Products
```
GET /products HTTP/1.1
    │
    ▼ Routes to:
ProductController::index()
    │
    ▼ Executes:
$products = Product::with('category')->paginate(10);
    │
    ▼ Queries:
SELECT * FROM products
JOIN categories ON products.category_id = categories.id
LIMIT 10 OFFSET 0
    │
    ▼ Returns View:
products.index with $products data
    │
    ▼ Renders:
HTML Table with 10 products, pagination links
```

### Example 2: Create Product
```
POST /products HTTP/1.1
Content: { name, description, price, quantity, category_id }
    │
    ▼ Routes to:
ProductController::store()
    │
    ▼ Validates:
- name: required, string, max 255
- price: required, numeric, min 0
- quantity: required, integer, min 0
- category_id: required, exists in categories
    │
    ├─ If Invalid: Redirect back with errors
    │
    └─ If Valid: Create and redirect
        │
        ▼ Executes:
INSERT INTO products (name, description, price, quantity, category_id)
VALUES (...)
        │
        ▼ Redirects to:
GET /products with success message
```

### Example 3: Update Product
```
PUT /products/1 HTTP/1.1
Content: { name, description, price, quantity, category_id }
    │
    ▼ Routes to:
ProductController::update($id=1)
    │
    ▼ Validates & Updates:
UPDATE products SET ... WHERE id = 1
    │
    ▼ Redirects to:
GET /products with success message
```

### Example 4: Delete Product
```
DELETE /products/1 HTTP/1.1
    │
    ▼ Routes to:
ProductController::destroy($id=1)
    │
    ▼ Executes:
DELETE FROM products WHERE id = 1
    │
    ▼ Redirects to:
GET /products with success message
```

---

## Summary

✅ **Complete MVC Architecture** - Models, Views, Controllers all properly structured
✅ **RESTful Routes** - Standard REST conventions followed
✅ **Database Relationships** - hasMany and belongsTo relationships
✅ **Form Validation** - Both client-side and server-side
✅ **Professional UI** - Bootstrap 5.3 responsive design
✅ **Error Handling** - Comprehensive error messages
✅ **Pagination** - Multiple items per page
✅ **Flash Messages** - Success/error notifications
✅ **Security** - CSRF protection, validation, sanitization

**Status**: ✅ **READY TO RUN**
