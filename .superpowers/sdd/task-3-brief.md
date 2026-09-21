# Task Brief: Plan A - Task 3: Migrate Landing Page to Blade

## Goal
Migrate the original landing page HTML, styles, and javascript assets from `inspeksi-pro.html` into a Laravel 11 Blade layout (`resources/views/layouts/app.blade.php`) and welcome view (`resources/views/welcome.blade.php`).

## Global Constraints
- PHP >= 8.2
- Laravel 11
- SQLite file-based database configured in `.env`
- Project root: `D:\Dicky_Setyawan\program-software\claude code\project 9 - inspection car\inspeksi-pro`

## Files to Create / Modify
- Create: `resources/views/layouts/app.blade.php`
- Modify: `resources/views/welcome.blade.php`
- Modify: `resources/css/app.css`
- Modify: `routes/web.php`

## Steps to Execute
1. Navigate to the project root `D:\Dicky_Setyawan\program-software\claude code\project 9 - inspection car\inspeksi-pro`.
2. Create the layout folder and file `resources/views/layouts/app.blade.php`:
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
3. Copy the entire `<style>` block content (excluding the `<style>` tag itself) from `D:\Dicky_Setyawan\program-software\claude code\project 9 - inspection car\inspeksi-pro.html` into `resources/css/app.css`.
4. Replace the contents of `resources/views/welcome.blade.php` with:
   - `@extends('layouts.app')`
   - `@section('title', 'InspeksiPro — Inspeksi Mobil Bekas Profesional')`
   - `@section('content')` followed by the original `<body>` inner markup from `inspeksi-pro.html` (change form action to `/booking` and form method to `POST`, add `@csrf`).
   - `@endsection`
   - `@push('scripts')` followed by the script content from `inspeksi-pro.html`, removing the form submit event handler (we will handle it via POST in Task 4).
   - `@endpush`
5. Configure `routes/web.php` to return the `welcome` view:
   ```php
   Route::get('/', fn() => view('welcome'));
   ```
6. Run `npm install` and verify the build works.
7. Commit changes to Git.
