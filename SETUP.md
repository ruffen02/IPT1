# IPT Store - Setup Instructions

## Quick Start Guide

Follow these steps to get the application running:

### 1. Install Dependencies
```bash
composer install
npm install
```

### 2. Generate Application Key
```bash
php artisan key:generate
```

### 3. Setup Database
The database connection is already configured in `.env` file with:
- **Database**: ipt
- **Host**: 127.0.0.1
- **Username**: root
- **Password**: (empty)

Make sure MySQL is running and the `ipt` database exists. Create it if needed:
```bash
mysql -u root -e "CREATE DATABASE IF NOT EXISTS ipt;"
```

### 4. Run Migrations
```bash
php artisan migrate
```

This will create the following tables:
- `users` - User authentication
- `categories` - Product categories
- `products` - Product inventory
- `cache`, `jobs`, `password_reset_tokens`, `sessions` - Laravel system tables

### 5. Start the Development Server
```bash
php artisan serve
```

The application will be available at: **http://localhost:8000**

## Features

### Navigation
- **Categories**: Browse and manage product categories
- **Products**: Browse and manage products with category assignment
- **Home**: Welcome page

### Available Routes

#### Categories
- `GET /categories` - View all categories
- `GET /categories/create` - Create new category form
- `POST /categories` - Store new category
- `GET /categories/{id}` - View category details
- `GET /categories/{id}/edit` - Edit category form
- `PUT /categories/{id}` - Update category
- `DELETE /categories/{id}` - Delete category

#### Products
- `GET /products` - View all products (with table display)
- `GET /products/create` - Create new product form
- `POST /products` - Store new product
- `GET /products/{id}` - View product details
- `GET /products/{id}/edit` - Edit product form
- `PUT /products/{id}` - Update product
- `DELETE /products/{id}` - Delete product

## Database Schema

### Categories Table
```
- id (Primary Key)
- name (String)
- description (Text, nullable)
- created_at (Timestamp)
- updated_at (Timestamp)
```

### Products Table
```
- id (Primary Key)
- name (String)
- description (Text, nullable)
- price (Decimal: 10,2)
- quantity (Integer)
- category_id (Foreign Key → categories.id)
- created_at (Timestamp)
- updated_at (Timestamp)
```

## File Structure

```
app/
├── Http/
│   └── Controllers/
│       ├── CategoryController.php
│       └── ProductController.php
├── Models/
│   ├── Category.php
│   ├── Product.php
│   └── User.php

database/
├── migrations/
│   ├── 0001_01_01_000000_create_users_table.php
│   ├── 0001_01_01_000001_create_cache_table.php
│   ├── 0001_01_01_000002_create_jobs_table.php
│   ├── 2026_03_01_000003_create_categories_table.php
│   └── 2026_03_01_000004_create_products_table.php

resources/
├── css/
│   └── app.css
├── js/
│   └── app.js
└── views/
    ├── layouts/
    │   └── app.blade.php
    ├── categories/
    │   ├── index.blade.php
    │   ├── create.blade.php
    │   ├── show.blade.php
    │   └── edit.blade.php
    ├── products/
    │   ├── index.blade.php
    │   ├── create.blade.php
    │   ├── show.blade.php
    │   └── edit.blade.php
    └── welcome.blade.php

routes/
└── web.php
```

## Features Implemented

✅ Category Management
- List all categories with product count
- Create new categories
- View category details
- Edit category information
- Delete categories

✅ Product Management
- List all products in a table format
- Create new products with category assignment
- View product details with pricing and stock info
- Edit product information
- Delete products
- Stock status badge (In Stock / Out of Stock)

✅ User Interface
- Bootstrap 5.3 responsive design
- Navigation bar with category and product links
- Footer
- Error handling and validation messages
- Success notifications
- Pagination support (10 items per page)

✅ Data Validation
- Required field validation
- Numeric validation for prices
- Category existence validation
- Error messages displayed on forms

## Troubleshooting

### Database Connection Error
- Check MySQL is running
- Verify `.env` database credentials
- Ensure `ipt` database exists

### Routes Not Found
- Run `php artisan route:cache` to clear route cache
- Ensure you're accessing the correct URL

### Missing Models/Controllers
- Run `composer dump-autoload`
- Verify all files are in correct namespaces

## Support

For any issues or questions, refer to the Laravel documentation at https://laravel.com/docs
