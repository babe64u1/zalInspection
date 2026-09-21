# Task Brief: Plan A - Task 1: Initialize Laravel Project

## Goal
Initialize a Laravel 11 project and configure basic database settings inside the project directory.

## Global Constraints
- PHP >= 8.2
- Laravel 11
- MySQL via XAMPP on `127.0.0.1:3307`, database name `inspeksi_pro`
- Project root: `D:\Dicky_Setyawan\program-software\claude code\project 9 - inspection car\inspeksi-pro`

## Files to Create / Modify
- Create: `inspeksi-pro/` (entire Laravel scaffold)
- Modify: `inspeksi-pro/.env`

## Steps to Execute
1. Open a terminal in `D:\Dicky_Setyawan\program-software\claude code\project 9 - inspection car\` and initialize the Laravel 11 application:
   ```bash
   composer create-project laravel/laravel inspeksi-pro "^11.0"
   ```
2. Configure database settings in `inspeksi-pro/.env` to point to port `3307` and DB `inspeksi_pro`:
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
3. Create the database `inspeksi_pro` using `mysql` command or appropriate CLI utility to avoid failure.
4. Verify the application runs using `php artisan serve` and checking response.
5. Initialize git repository in `inspeksi-pro` and commit the initial structure.
