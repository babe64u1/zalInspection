# Task Brief: Plan A - Task 2: Create Bookings Migration + Model

## Goal
Create the `Booking` model and migration with structural columns inside the `inspeksi-pro` Laravel project.

## Global Constraints
- PHP >= 8.2
- Laravel 11
- MySQL via XAMPP on `127.0.0.1:3306`, database name `inspeksi_pro`
- Project root: `D:\Dicky_Setyawan\program-software\claude code\project 9 - inspection car\inspeksi-pro`

## Files to Create / Modify
- Create: `database/migrations/xxxx_create_bookings_table.php` (auto-generated timestamp)
- Create: `app/Models/Booking.php`

## Steps to Execute
1. Navigate to the project root `D:\Dicky_Setyawan\program-software\claude code\project 9 - inspection car\inspeksi-pro`.
2. Generate the model and migration:
   ```bash
   D:\Dicky_Setyawan\program-software\claude code\project 9 - inspection car\php82\php.exe C:\xampp\php\composer.phar artisan make:model Booking -m
   ```
3. Update the migration file in `database/migrations/` to define the database schema:
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
4. Update the `app/Models/Booking.php` model structure:
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
5. Run the migration to apply changes to the database:
   ```bash
   D:\Dicky_Setyawan\program-software\claude code\project 9 - inspection car\php82\php.exe C:\xampp\php\composer.phar artisan migrate
   ```
6. Commit changes to Git.
