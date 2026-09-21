# Plan A: Laravel Scaffold + Blade Landing Page

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Initialize a Laravel 11 project, migrate `inspeksi-pro.html` into a Blade layout, wire up the booking form to save to MySQL, and serve the site locally.

**Architecture:** Laravel monolith. The existing `inspeksi-pro.html` CSS and markup is split into a Blade layout (`layouts/app.blade.php`) and a welcome view (`welcome.blade.php`). The booking form posts to a Laravel route that validates and stores to the `bookings` table.

**Tech Stack:** PHP 8.2+, Laravel 11, MySQL (XAMPP port 3307), Blade, Vite

## Global Constraints
- PHP >= 8.2
- Laravel 11
- MySQL via XAMPP on `127.0.0.1:3307`, database name `inspeksi_pro`
- All text/copy is in Indonesian (id), matching the original HTML exactly
- Project root: `D:\Dicky_Setyawan\program-software\claude code\project 9 - inspection car\inspeksi-pro`

---

### Task 1: Initialize Laravel Project

**Files:**
- Create: `inspeksi-pro/` (entire Laravel scaffold)
- Modify: `inspeksi-pro/.env`

**Interfaces:**
- Produces: running Laravel app at `http://localhost:8000`

- [ ] **Step 1: Create Laravel project**

Open terminal in `D:\Dicky_Setyawan\program-software\claude code\project 9 - inspection car\` and run:

```bash
composer create-project laravel/laravel inspeksi-pro "^11.0"
cd inspeksi-pro
```

- [ ] **Step 2: Configure .env for MySQL**

Edit `inspeksi-pro/.env`:

```env
APP_NAME=InspeksiPro
APP_URL=http://localhost:8000

DB_CONNECTION=mysql
DB_HOST=127.0.0.1
DB_PORT=3307
DB_DATABASE=inspeksi_pro
DB_USERNAME=root
DB_PASSWORD=
```

- [ ] **Step 3: Create the database**

In XAMPP phpMyAdmin or MySQL CLI:
```sql
CREATE DATABASE inspeksi_pro CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;
```

- [ ] **Step 4: Verify Laravel boots**

```bash
php artisan serve
```

Open `http://localhost:8000` — expect the default Laravel welcome page.

- [ ] **Step 5: Commit**

```bash
git init
git add .
git commit -m "feat: initialize Laravel 11 project"
```

---

### Task 2: Create Bookings Migration + Model

**Files:**
- Create: `database/migrations/xxxx_create_bookings_table.php`
- Create: `app/Models/Booking.php`

**Interfaces:**
- Produces: `Booking` Eloquent model with fields: `id`, `customer_name`, `customer_phone`, `car_details`, `location`, `package`, `inspection_date`, `notes`, `status`, `inspector_id`, `timestamps`

- [ ] **Step 1: Generate migration and model**

```bash
php artisan make:model Booking -m
```

- [ ] **Step 2: Write migration**

Open the generated migration file in `database/migrations/` and replace the `up()` method:

```php
public function up(): void
{
    Schema::create('bookings', function (Blueprint $table) {
        $table->uuid('id')->primary();
        $table->string('customer_name');
        $table->string('customer_phone');
        $table->string('car_details');
        $table->string('location');
        $table->string('package');
        $table->date('inspection_date');
        $table->text('notes')->nullable();
        $table->enum('status', ['pending', 'assigned', 'in_progress', 'completed'])->default('pending');
        $table->foreignId('inspector_id')->nullable()->constrained('users')->nullOnDelete();
        $table->timestamps();
    });
}
```

- [ ] **Step 3: Write Booking model**

Replace contents of `app/Models/Booking.php`:

```php
<?php

namespace App\Models;

use Illuminate\Database\Eloquent\Model;
use Illuminate\Database\Eloquent\Concerns\HasUuids;

class Booking extends Model
{
    use HasUuids;

    protected $fillable = [
        'customer_name', 'customer_phone', 'car_details',
        'location', 'package', 'inspection_date', 'notes',
        'status', 'inspector_id',
    ];

    protected $casts = [
        'inspection_date' => 'date',
    ];
}
```

- [ ] **Step 4: Run migration**

```bash
php artisan migrate
```

Expected output includes: `bookings` table created.

- [ ] **Step 5: Commit**

```bash
git add .
git commit -m "feat: add Booking model and migration"
```

---

### Task 3: Migrate Landing Page to Blade

**Files:**
- Create: `resources/views/layouts/app.blade.php`
- Create: `resources/views/welcome.blade.php`
- Modify: `routes/web.php`

**Interfaces:**
- Consumes: original `inspeksi-pro.html` content (CSS, markup, JS)
- Produces: `GET /` renders the full landing page via Blade

- [ ] **Step 1: Create Blade layout**

Create `resources/views/layouts/app.blade.php`:

```html
<!DOCTYPE html>
<html lang="id">
<head>
<meta charset="utf-8">
<meta name="viewport" content="width=device-width, initial-scale=1">
<title>@yield('title', 'InspeksiPro — Inspeksi Mobil Bekas Profesional')</title>
<meta name="description" content="Jasa inspeksi mobil bekas independen. 152 titik pemeriksaan, laporan digital dalam 24 jam, inspektor bersertifikat datang ke lokasi Anda.">
<meta name="csrf-token" content="{{ csrf_token() }}">
<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
<link href="https://fonts.googleapis.com/css2?family=Archivo:wght@500;700;800&family=Manrope:wght@400;500;700&family=JetBrains+Mono:wght@400;700&display=swap" rel="stylesheet">
@vite(['resources/css/app.css', 'resources/js/app.js'])
@stack('head')
</head>
<body>
@yield('content')
@stack('scripts')
</body>
</html>
```

- [ ] **Step 2: Extract CSS**

Create `resources/css/app.css` and paste the entire `<style>` block contents from `inspeksi-pro.html` (lines 12–269, everything between `<style>` and `</style>`).

- [ ] **Step 3: Create welcome view**

Create `resources/views/welcome.blade.php`:

```blade
@extends('layouts.app')

@section('title', 'InspeksiPro — Inspeksi Mobil Bekas Profesional')

@section('content')
{{-- Paste the full <body> content from inspeksi-pro.html here --}}
{{-- Replace the booking form action and add CSRF token --}}
{{-- Change: <form class="form" id="bookForm" novalidate> --}}
{{-- To:     <form class="form" id="bookForm" method="POST" action="/booking" novalidate> --}}
{{--           @csrf --}}
@endsection

@push('scripts')
<script>
// Paste the full <script> block from inspeksi-pro.html here (lines 676–827)
// Remove the form submit JS handler — Laravel will handle form POST
// Keep: zones data, scan loop, count-up stats, testimonials, reveal, mobile nav
</script>
@endpush
```

> **Note:** Open `inspeksi-pro.html` and copy the body content between `<body>` and `</body>` (excluding the `<script>` block) into `@section('content')`. Copy the `<script>` block into `@push('scripts')`. Replace the form tag with the version shown above.

- [ ] **Step 4: Add route**

Edit `routes/web.php`:

```php
<?php

use Illuminate\Support\Facades\Route;

Route::get('/', fn() => view('welcome'));
```

- [ ] **Step 5: Boot Vite**

```bash
npm install
npm run dev
```

Open `http://localhost:8000` — expect the full InspeksiPro landing page.

- [ ] **Step 6: Commit**

```bash
git add .
git commit -m "feat: migrate landing page to Blade"
```

---

### Task 4: Booking Form Submission

**Files:**
- Create: `app/Http/Controllers/BookingController.php`
- Modify: `routes/web.php`
- Modify: `resources/views/welcome.blade.php` (form action already set in Task 3)

**Interfaces:**
- Consumes: `Booking` model from Task 2
- Produces: `POST /booking` validates and saves booking, returns JSON `{success: true}` for AJAX or redirects with flash

- [ ] **Step 1: Create controller**

```bash
php artisan make:controller BookingController
```

- [ ] **Step 2: Write store method**

Replace `app/Http/Controllers/BookingController.php`:

```php
<?php

namespace App\Http\Controllers;

use App\Models\Booking;
use Illuminate\Http\Request;

class BookingController extends Controller
{
    public function store(Request $request)
    {
        $data = $request->validate([
            'customer_name'   => 'required|string|max:255',
            'customer_phone'  => ['required', 'string', 'regex:/^0\d{8,13}$/'],
            'car_details'     => 'required|string|max:255',
            'location'        => 'required|string|max:500',
            'package'         => 'required|string|max:100',
            'inspection_date' => 'required|date|after_or_equal:today',
            'notes'           => 'nullable|string|max:1000',
        ]);

        Booking::create($data);

        return response()->json(['success' => true]);
    }
}
```

- [ ] **Step 3: Add route**

Edit `routes/web.php`:

```php
<?php

use Illuminate\Support\Facades\Route;
use App\Http\Controllers\BookingController;

Route::get('/', fn() => view('welcome'));
Route::post('/booking', [BookingController::class, 'store']);
```

- [ ] **Step 4: Wire form JS to POST via fetch**

In `resources/views/welcome.blade.php`, replace the form submit handler in `@push('scripts')` with:

```javascript
const form = document.getElementById('bookForm');
form.addEventListener('submit', async e => {
    e.preventDefault();
    const fields = [...form.querySelectorAll('.field[data-req]')];
    const allOk = fields.map(validate).every(Boolean);
    if (!allOk) { form.querySelector('.field.bad input,.field.bad select').focus(); return; }

    const body = new FormData(form);
    // Map field ids to expected names
    const payload = {
        customer_name:   form.querySelector('#nama').value,
        customer_phone:  form.querySelector('#hp').value,
        car_details:     form.querySelector('#mobil').value,
        location:        form.querySelector('#lokasi').value,
        package:         form.querySelector('#paket').value,
        inspection_date: form.querySelector('#tgl').value,
        notes:           form.querySelector('#note').value,
        _token:          document.querySelector('meta[name="csrf-token"]').content,
    };

    const res = await fetch('/booking', {
        method: 'POST',
        headers: { 'Content-Type': 'application/json', 'Accept': 'application/json',
                   'X-CSRF-TOKEN': payload._token },
        body: JSON.stringify(payload),
    });

    if (res.ok) {
        document.getElementById('okMsg').style.display = 'block';
        form.querySelector('button[type=submit]').disabled = true;
        form.querySelector('button[type=submit]').textContent = 'Terkirim ✓';
    }
});
```

- [ ] **Step 5: Test the form**

Open `http://localhost:8000`, fill in the booking form, submit. Check database:

```bash
php artisan tinker
>>> App\Models\Booking::latest()->first()
```

Expect the submitted data to appear.

- [ ] **Step 6: Commit**

```bash
git add .
git commit -m "feat: booking form stores to database"
```
