---
name: 2026-07-29-car-inspection-design
description: Design specification for converting InspeksiPro landing page to Laravel + Filament with a 3D Three.js wireframe car model.
metadata:
  type: project
---

# InspeksiPro - 3D Landing Page & Inspector Backend

Design specification for converting the static `inspeksi-pro.html` into a Laravel 11 monolith, incorporating an interactive 3D wireframe car scanning animation in the inspection section, and building a Filament-based inspector dashboard.

## 1. Scope & Features
- **Public Site**: Laravel Blade layout. Migration of original CSS and markup from `inspeksi-pro.html`.
- **3D Scanning Component (Zone 2 Replacement)**:
  - Replacing the static 2D SVG car diagram with an interactive 3D wireframe car model using Three.js.
  - Features a glowing scanning line shader running back and forth over the car.
  - Interactive clickable hotspots on the 3D car corresponding to the 5 inspection zones (Exterior, Interior, Engine, Undercarriage, Docs).
  - Selecting a zone on the 3D model highlights that region and updates the sidebar to display the corresponding checklist items (syncing with original JavaScript logic).
  - Safe fallback to the static 2D SVG if WebGL is unsupported or fails to load.
- **Booking Form Submission**:
  - The booking form submits asynchronously (AJAX) or via standard POST request.
  - Validates and stores the client name, phone number, vehicle type, location, package, and date.
- **Inspector Dashboard (Filament v3)**:
  - Inspector-only login credentials.
  - Displays a clean list of assigned inspection jobs.
  - Inspector checklist input: filling out the 152 checkpoints categorized into the 5 zones.
  - Simple controls per checklist item: status toggle (OK / Warning / Critical) and optional text notes or photo upload.
  - Completion action generates a public-viewable digital report URL containing the exact findings.

## 2. Technical Stack
- **Backend Framework**: Laravel 11 (PHP 8.2+)
- **Dashboard UI**: Filament PHP v3
- **Database**: MySQL (hosted locally via XAMPP on port 3307 or 3306)
- **Frontend 3D**: Three.js (vanilla CDN import or bundled via Vite), OrbitControls, GLTFLoader.
- **Assets**: Optimized low-poly GLTF/GLB car model (wireframe styled).

## 3. Database Schema

### `bookings`
- `id` (UUID)
- `customer_name` (string)
- `customer_phone` (string)
- `car_details` (string)
- `location` (string)
- `package` (string)
- `inspection_date` (date)
- `notes` (text, nullable)
- `status` (enum: pending, assigned, in_progress, completed)
- `inspector_id` (foreignId to users, nullable)
- `timestamps`

### `reports`
- `id` (UUID)
- `booking_id` (foreignId to bookings)
- `summary` (text, nullable)
- `overall_status` (enum: pass, warning, fail)
- `completed_at` (timestamp)
- `timestamps`

### `report_items`
- `id` (bigint, PK)
- `report_id` (foreignId to reports)
- `zone` (string) - Exterior, Interior, Engine, Undercarriage, Docs
- `point_name` (string) - e.g., "Rangka & sasis"
- `status` (enum: ok, warning, critical)
- `notes` (text, nullable)
- `photo_path` (string, nullable)
- `timestamps`

## 4. 3D Model Implementation Details
- **Rendering**: WebGLRenderer with OrbitControls.
- **Wireframe Styling**: Rendered using a MeshBasicMaterial with `{ wireframe: true, color: 0x1FAF5F }` or a Custom ShaderMaterial generating an animated green/amber scan line glowing over the geometry.
- **Raycasting**: A Three.js Raycaster detects mouse/touch clicks on transparent bounding box meshes mapped to the car zones.
- **Interaction Hook**: Triggers `setZone(zoneIndex)` in the existing JavaScript app framework.

## 5. Directory Structure
```
inspeksi-pro/
├── app/Models/          -> Booking.php, Report.php, ReportItem.php
├── app/Filament/        -> Custom Resources for Inspections
├── resources/views/     -> welcome.blade.php (landing page), report.blade.php (digital report view)
├── resources/js/        -> app.js (loads Three.js scene)
├── public/models/       -> car.glb (wireframe source model)
└── database/migrations/ -> structure setup
```
