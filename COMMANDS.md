# 🎯 Essential Commands Reference

## Database Setup Commands

### Create Database
```bash
# Windows (MySQL)
mysql -u root -e "CREATE DATABASE IF NOT EXISTS ipt;"

# Or interactive
mysql -u root -p
mysql> CREATE DATABASE IF NOT EXISTS ipt;
mysql> EXIT;
```

### View Migrations Status
```bash
php artisan migrate:status
```

### Run Migrations
```bash
# Run all pending migrations
php artisan migrate

# Run specific migration
php artisan migrate --path=database/migrations/2026_03_01_000003_create_categories_table.php

# Run migrations with verbose output
php artisan migrate -v
```

### Rollback Migrations
```bash
# Rollback last batch
php artisan migrate:rollback

# Rollback all migrations
php artisan migrate:reset

# Rollback and re-run
php artisan migrate:refresh

# Refresh and seed
php artisan migrate:refresh --seed
```

---

## Development Server Commands

### Start Development Server
```bash
php artisan serve

# Specify host and port
php artisan serve --host=0.0.0.0 --port=8000

# On specific IP
php artisan serve --host=192.168.1.100
```

### Clear Caches
```bash
# Clear application cache
php artisan cache:clear

# Clear route cache
php artisan route:cache

# Clear config cache
php artisan config:cache

# Clear all caches
php artisan optimize:clear
```

---

## Tinker Commands (Interactive Shell)

### Enter Tinker
```bash
php artisan tinker
```

### Create Sample Data (in Tinker)
```php
# Create a category
App\Models\Category::create(['name' => 'Electronics', 'description' => 'Electronic devices']);

# Create multiple categories
foreach (['Electronics', 'Books', 'Clothing'] as $cat) {
    App\Models\Category::create(['name' => $cat]);
}

# Create a product
App\Models\Product::create([
    'name' => 'Laptop',
    'description' => 'High-performance laptop',
    'price' => 999.99,
    'quantity' => 5,
    'category_id' => 1
]);

# Get all categories
App\Models\Category::all();

# Get all products
App\Models\Product::all();

# Find specific product
App\Models\Product::find(1);

# Get product with category
App\Models\Product::with('category')->first();

# Get category with products
App\Models\Category::with('products')->first();

# Count products in category
App\Models\Category::find(1)->products->count();

# Exit Tinker
exit
```

---

## Composer Commands

### Install Dependencies
```bash
composer install

# Update dependencies
composer update

# Update specific package
composer update package-name

# Dump autoloader
composer dump-autoload
```

---

## Artisan Commands Reference

### Serve
```bash
php artisan serve
```

### Tinker (Interactive Shell)
```bash
php artisan tinker
```

### Migrate
```bash
php artisan migrate
php artisan migrate:refresh
php artisan migrate:rollback
```

### Make Commands (Generators)
```bash
# Create a new model
php artisan make:model ProductCategory

# Create model with migration
php artisan make:model ProductCategory -m

# Create controller
php artisan make:controller ItemController

# Create controller with model
php artisan make:controller ItemController -m Item

# Create migration
php artisan make:migration create_items_table

# Create seeder
php artisan make:seeder CategorySeeder
```

### Cache & Optimize
```bash
php artisan cache:clear
php artisan config:cache
php artisan route:cache
php artisan view:cache
php artisan optimize
php artisan optimize:clear
```

### Queue (if configured)
```bash
php artisan queue:work
php artisan queue:listen
php artisan queue:failed
php artisan queue:retry
```

### Debugging
```bash
# Check routes
php artisan route:list

# Check migrations
php artisan migrate:status

# Check config
php artisan config:show

# Tinker (REPL)
php artisan tinker
```

---

## NPM Commands

### Install Node Modules
```bash
npm install
```

### Run Dev Build
```bash
npm run dev
```

### Run Production Build
```bash
npm run build
```

### Watch Files
```bash
npm run watch
```

---

## Git Commands (Version Control)

```bash
# Initialize repository
git init

# Add files
git add .

# Commit changes
git commit -m "Add category and product features"

# Check status
git status

# View log
git log

# Create branch
git branch feature/search

# Switch branch
git checkout feature/search

# Merge branch
git merge feature/search
```

---

## File Permission Commands (Linux/Mac)

```bash
# Make directory writable
chmod 775 storage
chmod 775 bootstrap/cache

# Make file writable
chmod 644 .env

# Owner permissions
chown -R www-data:www-data storage
```

---

## Database Backup Commands

### Backup MySQL Database
```bash
# Backup to file
mysqldump -u root -p ipt > backup_ipt.sql

# Restore from file
mysql -u root -p ipt < backup_ipt.sql
```

---

## Quick Workflow

### First Time Setup
```bash
# 1. Install dependencies
composer install

# 2. Create .env file (if not exists)
cp .env.example .env

# 3. Generate app key
php artisan key:generate

# 4. Create database
mysql -u root -e "CREATE DATABASE ipt;"

# 5. Run migrations
php artisan migrate

# 6. Start server
php artisan serve
```

### Daily Development
```bash
# 1. Start server
php artisan serve

# 2. (In another terminal) Run frontend build
npm run dev

# 3. Access http://localhost:8000
```

### Before Deployment
```bash
# 1. Optimize for production
php artisan optimize

# 2. Clear all caches
php artisan optimize:clear

# 3. Set production mode in .env
APP_ENV=production
APP_DEBUG=false

# 4. Generate app key
php artisan key:generate --force
```

---

## Testing Commands

### Run All Tests
```bash
php artisan test
```

### Run Specific Test
```bash
php artisan test tests/Unit/CategoryTest.php
```

### Run with Coverage
```bash
php artisan test --coverage
```

---

## Useful Laravel Facades/Helpers

### In Tinker or Code

```php
# DB Queries
DB::select('SELECT * FROM products');
DB::insert('INSERT INTO products ...');
DB::update('UPDATE products SET ...');
DB::delete('DELETE FROM products WHERE id = ?', [1]);

# Models
Product::all();
Product::find(1);
Product::where('price', '>', 100)->get();
Product::create(['name' => 'Item']);
Product::update(['price' => 50]);
Product::delete();

# Collections
Product::all()->count();
Product::all()->sum('price');
Product::all()->average('price');
Product::all()->pluck('name');

# Auth
Auth::user();
Auth::check();
Auth::logout();

# Sessions
session()->put('key', 'value');
session()->get('key');
session()->forget('key');

# Redirects
redirect('/products');
redirect()->back();
redirect()->route('products.index');

# Views
view('products.index', $data);
view('products.index')->with('products', $products);
```

---

## Common Issues & Fixes

### Issue: "No application encryption key has been specified"
```bash
php artisan key:generate
```

### Issue: "Class not found" error
```bash
composer dump-autoload
```

### Issue: Routes not working
```bash
php artisan route:cache
# Or clear all caches:
php artisan optimize:clear
```

### Issue: Views not updating
```bash
php artisan view:cache
php artisan optimize:clear
```

### Issue: Database connection error
```bash
# Check .env file settings
# Verify MySQL is running
# Test connection:
php artisan tinker
# Then: DB::connection()->getPdo()
```

---

## Useful URL Patterns

### Application URLs
```
Home:           http://localhost:8000
Categories:     http://localhost:8000/categories
Products:       http://localhost:8000/products (✨ TABLE VIEW)
New Category:   http://localhost:8000/categories/create
New Product:    http://localhost:8000/products/create
View Product:   http://localhost:8000/products/1
Edit Product:   http://localhost:8000/products/1/edit
```

### Debug/Info URLs (if enabled)
```
Routes List:    php artisan route:list
Config:         php artisan config:show
```

---

## Performance Optimization

### Production Checklist
```bash
# 1. Optimize autoloader
composer install --optimize-autoloader --no-dev

# 2. Cache configuration
php artisan config:cache

# 3. Cache routes
php artisan route:cache

# 4. Cache views
php artisan view:cache

# 5. Set production mode
APP_ENV=production
APP_DEBUG=false

# 6. Generate app key
php artisan key:generate --force

# 7. Run migrations
php artisan migrate --force
```

---

## Helpful Development Tools

### Laravel Debugbar (Optional)
```bash
# Install
composer require barryvdh/laravel-debugbar --dev

# The debugbar will appear at bottom of page in dev mode
```

### Laravel IDE Helper (Optional)
```bash
# Install
composer require --dev barryvdh/laravel-ide-helper

# Generate helpers
php artisan ide-helper:generate
```

---

## Quick Reference Cheat Sheet

| Command | Purpose |
|---------|---------|
| `php artisan serve` | Start development server |
| `php artisan migrate` | Run database migrations |
| `php artisan tinker` | Interactive shell |
| `composer install` | Install dependencies |
| `npm run dev` | Build frontend assets |
| `php artisan cache:clear` | Clear all caches |
| `php artisan optimize:clear` | Clear everything |
| `php artisan route:list` | View all routes |
| `php artisan make:model X` | Create model X |
| `php artisan make:migration` | Create migration |
| `php artisan migrate:rollback` | Undo migrations |

---

## Tips & Tricks

1. **Use Tinker for testing queries**
   ```bash
   php artisan tinker
   # Test queries without creating actual code
   ```

2. **Clear caches when things behave oddly**
   ```bash
   php artisan optimize:clear
   ```

3. **Use `php artisan route:list` to debug routes**
   ```bash
   php artisan route:list | grep products
   ```

4. **Check migrations status before deploying**
   ```bash
   php artisan migrate:status
   ```

5. **Always use `->paginate()` for large datasets**
   ```php
   $products = Product::paginate(10); // Better than all()
   ```

---

**Status**: ✅ All systems ready!

Use these commands to manage, develop, and deploy your Laravel application successfully.
