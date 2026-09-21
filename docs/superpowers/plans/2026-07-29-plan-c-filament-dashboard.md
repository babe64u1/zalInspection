# Plan C: Filament Inspector Dashboard

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Create an internal dashboard using Filament v3 for inspectors to log in, view assigned bookings, and fill out 152-point digital reports.

**Architecture:** Install Filament, create User/Admin authentication. Scaffold Filament Resources for `Booking` and `Report`. A `Report` contains multiple `ReportItem` entries representing the inspection checklist. 

**Tech Stack:** Laravel 11, Filament v3, Livewire

## Global Constraints
- Only authenticated users can access `/admin`.
- Filament Panel setup bound to `admin` path.

---

### Task 1: Install Filament

**Files:**
- Modify: `composer.json`
- Create: `app/Providers/Filament/AdminPanelProvider.php`

**Interfaces:**
- Produces: Base Filament administration panel accessible at `/admin`.

- [ ] **Step 1: Install Filament v3 package**

```bash
composer require filament/filament:"^3.2" -W
```

- [ ] **Step 2: Install Filament panel**

```bash
php artisan filament:install --panels
```

- [ ] **Step 3: Create admin user**

```bash
php artisan make:filament-user
# Name: admin, Email: admin@inspeksipro.test, Password: password
```

- [ ] **Step 4: Verify login**

Open `http://localhost:8000/admin`. Log in with credentials.

- [ ] **Step 5: Commit**

```bash
git add .
git commit -m "feat: install Filament admin panel"
```

---

### Task 2: Create Booking Resource

**Files:**
- Create: `app/Filament/Resources/BookingResource.php`

**Interfaces:**
- Consumes: `Booking` model
- Produces: CRUD management for bookings in the admin panel.

- [ ] **Step 1: Generate resource**

```bash
php artisan make:filament-resource Booking
```

- [ ] **Step 2: Configure BookingResource fields**

Edit `app/Filament/Resources/BookingResource.php`. In `form()` and `table()`:

```php
use Filament\Forms\Components\TextInput;
use Filament\Forms\Components\DatePicker;
use Filament\Forms\Components\Select;
use Filament\Forms\Components\Textarea;
use Filament\Tables\Columns\TextColumn;

public static function form(Form $form): Form
{
    return $form->schema([
        TextInput::make('customer_name')->required(),
        TextInput::make('customer_phone')->required(),
        TextInput::make('car_details')->required(),
        TextInput::make('location')->required(),
        Select::make('package')->options([
            'Standar — Rp550rb' => 'Standar',
            'Menyeluruh — Rp750rb' => 'Menyeluruh',
            'Menyeluruh + Garansi — Rp1,9jt' => 'Menyeluruh + Garansi'
        ])->required(),
        DatePicker::make('inspection_date')->required(),
        Select::make('status')->options([
            'pending' => 'Pending',
            'assigned' => 'Assigned',
            'in_progress' => 'In Progress',
            'completed' => 'Completed'
        ])->default('pending'),
        Textarea::make('notes'),
    ]);
}

public static function table(Table $table): Table
{
    return $table->columns([
        TextColumn::make('customer_name')->searchable(),
        TextColumn::make('car_details'),
        TextColumn::make('inspection_date')->date(),
        TextColumn::make('status')->badge(),
    ]);
}
```

- [ ] **Step 3: Test Resource**

Access `/admin/bookings`. Verify submitted landing page bookings appear in the list.

- [ ] **Step 4: Commit**

```bash
git add .
git commit -m "feat: setup Booking filament resource"
```

---

### Task 3: Reporting Migration + Models

**Files:**
- Create: `database/migrations/xxxx_create_reports_table.php`
- Create: `app/Models/Report.php`
- Create: `app/Models/ReportItem.php`

**Interfaces:**
- Produces: `Report` and `ReportItem` relational models linked to `Booking`.

- [ ] **Step 1: Generate models and migrations**

```bash
php artisan make:model Report -m
php artisan make:model ReportItem -m
```

- [ ] **Step 2: Write migrations**

In the Reports migration:

```php
Schema::create('reports', function (Blueprint $table) {
    $table->uuid('id')->primary();
    $table->foreignUuid('booking_id')->constrained('bookings')->cascadeOnDelete();
    $table->text('summary')->nullable();
    $table->enum('overall_status', ['pass', 'warning', 'fail'])->nullable();
    $table->timestamp('completed_at')->nullable();
    $table->timestamps();
});
```

In the ReportItems migration:

```php
Schema::create('report_items', function (Blueprint $table) {
    $table->id();
    $table->foreignUuid('report_id')->constrained('reports')->cascadeOnDelete();
    $table->string('zone');
    $table->string('point_name');
    $table->enum('status', ['ok', 'warning', 'critical'])->default('ok');
    $table->text('notes')->nullable();
    $table->string('photo_path')->nullable();
    $table->timestamps();
});
```

- [ ] **Step 3: Apply migrations**

```bash
php artisan migrate
```

- [ ] **Step 4: Add relationships**

In `app/Models/Report.php`:
```php
class Report extends Model {
    use \Illuminate\Database\Eloquent\Concerns\HasUuids;
    protected $guarded = [];
    public function booking() { return $this->belongsTo(Booking::class); }
    public function items() { return $this->hasMany(ReportItem::class); }
}
```

In `app/Models/ReportItem.php`:
```php
class ReportItem extends Model {
    protected $guarded = [];
    public function report() { return $this->belongsTo(Report::class); }
}
```

In `app/Models/Booking.php`:
```php
public function report() { return $this->hasOne(Report::class); }
```

- [ ] **Step 5: Commit**

```bash
git add .
git commit -m "feat: add Report and ReportItem models"
```

---

### Task 4: Create Report Resource & Checklist (Repeater)

**Files:**
- Create: `app/Filament/Resources/ReportResource.php`

**Interfaces:**
- Consumes: `Report` and `ReportItem`
- Produces: Admin panel UI for filling out the inspection points using a Repeater field.

- [ ] **Step 1: Generate resource**

```bash
php artisan make:filament-resource Report
```

- [ ] **Step 2: Build the Checklist form with Repeater**

Edit `app/Filament/Resources/ReportResource.php`:

```php
use Filament\Forms\Components\Select;
use Filament\Forms\Components\Textarea;
use Filament\Forms\Components\Repeater;
use Filament\Forms\Components\TextInput;
use Filament\Forms\Components\FileUpload;

public static function form(Form $form): Form
{
    return $form->schema([
        Select::make('booking_id')
            ->relationship('booking', 'customer_name')
            ->required(),
        Select::make('overall_status')->options([
            'pass' => 'Lulus',
            'warning' => 'Catatan',
            'fail' => 'Gagal'
        ]),
        Textarea::make('summary')->columnSpanFull(),
        
        Repeater::make('items')
            ->relationship()
            ->schema([
                Select::make('zone')->options([
                    '0' => 'Eksterior & Rangka',
                    '1' => 'Interior & Kabin',
                    '2' => 'Mesin & Transmisi',
                    '3' => 'Kaki-kaki & Uji Jalan',
                    '4' => 'Dokumen'
                ])->required(),
                TextInput::make('point_name')->required(),
                Select::make('status')->options([
                    'ok' => 'OK',
                    'warning' => 'Warning',
                    'critical' => 'Critical'
                ])->default('ok')->required(),
                TextInput::make('notes'),
                FileUpload::make('photo_path')->image()
            ])
            ->columns(2)
            ->columnSpanFull()
            ->itemLabel(fn (array $state): ?string => $state['point_name'] ?? null),
    ]);
}
```

- [ ] **Step 3: Add Table Columns**

```php
use Filament\Tables\Columns\TextColumn;

public static function table(Table $table): Table
{
    return $table->columns([
        TextColumn::make('booking.customer_name')->label('Customer'),
        TextColumn::make('overall_status')->badge(),
        TextColumn::make('created_at')->dateTime(),
    ]);
}
```

- [ ] **Step 4: Test Form**

Open `/admin/reports/create`. Verify that a Repeater appears allowing multiple inspection items to be dynamically added. Save a test report.

- [ ] **Step 5: Commit**

```bash
git add .
git commit -m "feat: add Report filament resource with checklist repeater"
```
