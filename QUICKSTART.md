# IPT Store - Complete Setup & Run Guide

## 🚀 Quick Start (5 Minutes)

### Step 1: Install Composer Dependencies
```bash
cd c:\Users\Acer\Desktop\niekoipt\ipt
composer install
```

### Step 2: Create Database
Ensure MySQL is running, then create the database:
```bash
mysql -u root -p
# Enter password (press Enter if none)
# Then in MySQL console:
CREATE DATABASE IF NOT EXISTS ipt;
EXIT;
```

OR use this one-liner:
```bash
mysql -u root -e "CREATE DATABASE IF NOT EXISTS ipt;"
```

### Step 3: Run Migrations
```bash
php artisan migrate
```

This creates all necessary tables.

### Step 4: Start Development Server
```bash
php artisan serve
```

### Step 5: Open Browser
Visit: **http://localhost:8000**

---

## 📋 What's Been Created

### ✅ Controllers (2 files)
1. **CategoryController.php** - Manages category CRUD operations
2. **ProductController.php** - Manages product CRUD operations

### ✅ Models (2 files)
1. **Category.php** - Category model with relationships
2. **Product.php** - Product model with relationships

### ✅ Migrations (2 files)
1. **create_categories_table** - Creates categories table
2. **create_products_table** - Creates products table with foreign key

### ✅ Views (8 files)

**Categories:**
- `categories/index.blade.php` - List all categories
- `categories/create.blade.php` - Create category form
- `categories/show.blade.php` - View category details
- `categories/edit.blade.php` - Edit category form

**Products:**
- `products/index.blade.php` - **✨ Display products in professional table**
- `products/create.blade.php` - Create product form
- `products/show.blade.php` - View product details
- `products/edit.blade.php` - Edit product form

**Layout:**
- `layouts/app.blade.php` - Main layout with Bootstrap 5.3

---

## 🗄️ Database Schema

### Categories Table
| Column | Type | Notes |
|--------|------|-------|
| id | integer | Primary key, auto-increment |
| name | string(255) | **Required** |
| description | text | Optional |
| created_at | timestamp | Auto-filled |
| updated_at | timestamp | Auto-filled |

### Products Table
| Column | Type | Notes |
|--------|------|-------|
| id | integer | Primary key, auto-increment |
| name | string(255) | **Required** |
| description | text | Optional |
| price | decimal(10,2) | **Required**, min: 0 |
| quantity | integer | **Required**, min: 0 |
| category_id | integer | **Required**, foreign key to categories |
| created_at | timestamp | Auto-filled |
| updated_at | timestamp | Auto-filled |

---

## 🛣️ API Routes

### Category Routes
```
GET    /categories              → List all categories
GET    /categories/create       → Show create form
POST   /categories              → Store new category
GET    /categories/{id}         → Show category details
GET    /categories/{id}/edit    → Show edit form
PUT    /categories/{id}         → Update category
DELETE /categories/{id}         → Delete category
```

### Product Routes
```
GET    /products                → List all products (TABLE VIEW)
GET    /products/create         → Show create form
POST   /products                → Store new product
GET    /products/{id}           → Show product details
GET    /products/{id}/edit      → Show edit form
PUT    /products/{id}           → Update product
DELETE /products/{id}           → Delete product
```

---

## 💡 Usage Examples

### Example 1: Create a Category
1. Go to: `http://localhost:8000/categories`
2. Click "Add New Category"
3. Fill in:
   - **Category Name**: Electronics
   - **Description**: Electronic devices and gadgets
4. Click "Create Category"
5. Success! Category added to table

### Example 2: Create a Product
1. Go to: `http://localhost:8000/products`
2. Click "Add New Product"
3. Fill in:
   - **Product Name**: Laptop
   - **Description**: High-performance laptop
   - **Price**: 999.99
   - **Quantity**: 5
   - **Category**: Electronics
4. Click "Create Product"
5. Success! Product appears in table

### Example 3: View Products Table
1. Go to: `http://localhost:8000/products`
2. See professional table with:
   - Product ID
   - Product Name
   - Category (as colored badge)
   - Description (truncated)
   - Price (formatted as $X.XX)
   - Quantity (stock status badge)
   - Created Date
   - Action buttons (View, Edit, Delete)

---

## 🎨 Features Showcase

### Products Table
The products index page displays a fully functional table with:

| Feature | Description |
|---------|-------------|
| **Responsive Design** | Adapts to all screen sizes |
| **Pagination** | 10 items per page |
| **Stock Status** | Green "In Stock" or red "Out of Stock" badge |
| **Price Formatting** | Displays as $X.XX |
| **Category Display** | Shows category in info badge |
| **Action Buttons** | View, Edit, Delete per row |
| **Quick Add** | "Add New Product" button |
| **Empty State** | Helpful message when no products |
| **Sorting** | Click column headers (if added) |

### Form Validation
- **Client-side**: HTML5 validation
- **Server-side**: Laravel validation rules
- **Error Display**: Shows above problematic field
- **Flash Messages**: Success notifications

---

## 🔧 Troubleshooting

### Issue: "SQLSTATE HY000 2002 No such file or directory"
**Solution**: MySQL is not running
```bash
# Start MySQL on Windows
# Or use Laravel Valet/Homestead
```

### Issue: "Class not found" error
**Solution**: Regenerate autoloader
```bash
composer dump-autoload
```

### Issue: Routes not working
**Solution**: Clear route cache
```bash
php artisan route:cache
```

### Issue: Database table not created
**Solution**: Run migrations
```bash
php artisan migrate
# Or reset and re-run
php artisan migrate:refresh
```

### Issue: 500 Internal Server Error
**Solution**: Check Laravel logs
```bash
# View logs
tail -f storage/logs/laravel.log
# On Windows:
Get-Content storage\logs\laravel.log -Tail 50 -Wait
```

---

## 📂 Project File Structure

```
c:\Users\Acer\Desktop\niekoipt\ipt\
├── app/
│   ├── Http/
│   │   └── Controllers/
│   │       ├── CategoryController.php ✓
│   │       ├── ProductController.php ✓
│   │       └── Controller.php
│   └── Models/
│       ├── Category.php ✓
│       ├── Product.php ✓
│       └── User.php
│
├── database/
│   ├── migrations/
│   │   ├── 0001_01_01_000000_create_users_table.php
│   │   ├── 0001_01_01_000001_create_cache_table.php
│   │   ├── 0001_01_01_000002_create_jobs_table.php
│   │   ├── 2026_03_01_000003_create_categories_table.php ✓
│   │   └── 2026_03_01_000004_create_products_table.php ✓
│   ├── factories/
│   └── seeders/
│
├── resources/
│   ├── views/
│   │   ├── layouts/
│   │   │   └── app.blade.php ✓
│   │   ├── categories/
│   │   │   ├── index.blade.php ✓
│   │   │   ├── create.blade.php ✓
│   │   │   ├── show.blade.php ✓
│   │   │   └── edit.blade.php ✓
│   │   ├── products/
│   │   │   ├── index.blade.php ✓ (TABLE)
│   │   │   ├── create.blade.php ✓
│   │   │   ├── show.blade.php ✓
│   │   │   └── edit.blade.php ✓
│   │   ├── welcome.blade.php
│   ├── css/
│   └── js/
│
├── routes/
│   └── web.php ✓
│
├── .env ✓ (Database configured)
├── composer.json
├── SETUP.md (Setup instructions)
└── IMPLEMENTATION.md (Implementation details)
```

---

## ✨ Key Features Implemented

### ✅ Database Layer
- [x] Category migrations
- [x] Product migrations
- [x] Foreign key relationships
- [x] Timestamps for tracking

### ✅ Model Layer
- [x] Category model with relationships
- [x] Product model with relationships
- [x] Mass assignment protection
- [x] Type casting for decimal prices

### ✅ Controller Layer
- [x] CategoryController with all 7 REST actions
- [x] ProductController with all 7 REST actions
- [x] Form validation
- [x] Error handling

### ✅ View Layer
- [x] Professional Bootstrap 5.3 design
- [x] Responsive layouts
- [x] Form components
- [x] **Table display for products** ✨
- [x] Error messages
- [x] Flash notifications
- [x] Navigation bar
- [x] Footer

### ✅ Routing
- [x] RESTful resource routes
- [x] Named routes for flexibility
- [x] Route binding for models

---

## 🎯 Next Steps After Setup

1. **Create Sample Data**
   ```bash
   php artisan tinker
   # In Tinker:
   App\Models\Category::create(['name' => 'Electronics', 'description' => 'Electronic devices']);
   ```

2. **Explore the UI**
   - Visit each page
   - Test create/edit/delete operations
   - Check validation messages

3. **Customize**
   - Add more fields to products
   - Customize colors and styling
   - Add search functionality
   - Add filtering options

4. **Scale Up**
   - Add user authentication
   - Add product images
   - Add inventory management
   - Add order system

---

## 📞 Support & Resources

- **Laravel Documentation**: https://laravel.com/docs
- **Bootstrap Documentation**: https://getbootstrap.com/docs
- **MySQL Documentation**: https://dev.mysql.com/doc/

---

## ✅ All Systems Ready!

Your application is fully configured and ready to run. Simply:

```bash
php artisan migrate
php artisan serve
```

Then visit **http://localhost:8000** 🎉

**Happy coding!**
