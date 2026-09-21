# Plan B: Three.js 3D Wireframe Car Scanning

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Integrate Three.js on the landing page, replacing the static 2D SVG car diagram with an interactive, animated 3D wireframe car scanning component.

**Architecture:** Initialize a Canvas in the Inspection section. Render a stylized 3D car (using procedural geometries for instant load and reliability, or loading a GLB). Animate a glowing horizontal/vertical laser plane (shader/geometry) passing across the car. Attach click events using Three.js Raycaster to map to the 5 zones, calling the existing `setZone()` JavaScript method.

**Tech Stack:** Three.js, OrbitControls, Vite/JS

## Global Constraints
- Clean responsive layout matching the original landing page design.
- Fallback to the original 2D SVG if WebGL is unavailable.
- Interactive hotspots linking directly to zones 0-4.

---

### Task 1: Add Three.js Scene Setup

**Files:**
- Create: `resources/js/car-scene.js`
- Modify: `resources/views/welcome.blade.php`

**Interfaces:**
- Produces: WebGL canvas inside the Inspection card container, rendering a clean blank 3D grid and controls.

- [ ] **Step 1: Install Three.js package**

```bash
cd D:\Dicky_Setyawan\program-software\claude code\project 9 - inspection car\inspeksi-pro
npm install three
```

- [ ] **Step 2: Create Three.js controller file**

Create `resources/js/car-scene.js`:

```javascript
import * as THREE from 'three';
import { OrbitControls } from 'three/examples/jsm/controls/OrbitControls.js';

export function initCarScene(containerId, onZoneSelect) {
    const container = document.getElementById(containerId);
    if (!container) return;

    // 1. Setup Scene, Camera, Renderer
    const scene = new THREE.Scene();
    scene.background = new THREE.Color(0xffffff); // Matches .carbox background

    const camera = new THREE.PerspectiveCamera(45, container.clientWidth / 240, 0.1, 100);
    camera.position.set(8, 4, 10);

    const renderer = new THREE.WebGLRenderer({ antialias: true });
    renderer.setSize(container.clientWidth, 240);
    renderer.setPixelRatio(Math.min(window.devicePixelRatio, 2));
    container.innerHTML = '';
    container.appendChild(renderer.domElement);

    // 2. Add OrbitControls
    const controls = new OrbitControls(camera, renderer.domElement);
    controls.enableDamping = true;
    controls.dampingFactor = 0.05;
    controls.maxPolarAngle = Math.PI / 2 - 0.05; // Don't go below ground
    controls.minDistance = 5;
    controls.maxDistance = 15;

    // 3. Grid Helper
    const grid = new THREE.GridHelper(20, 20, 0x0d1b24, 0xdde3e6);
    grid.position.y = -1;
    scene.add(grid);

    // 4. Lights
    const ambientLight = new THREE.AmbientLight(0xffffff, 0.6);
    scene.add(ambientLight);

    const dirLight = new THREE.DirectionalLight(0xffffff, 0.8);
    dirLight.position.set(5, 10, 7);
    scene.add(dirLight);

    // 5. Render/Animation Loop
    const clock = new THREE.Clock();
    function animate() {
        requestAnimationFrame(animate);
        controls.update();
        renderer.render(scene, camera);
    }
    animate();

    // 6. Resize listener
    window.addEventListener('resize', () => {
        camera.aspect = container.clientWidth / 240;
        camera.updateProjectionMatrix();
        renderer.setSize(container.clientWidth, 240);
    });

    return { scene, camera, renderer };
}
```

- [ ] **Step 3: Modify resources/js/app.js to load scene**

Add to `resources/js/app.js`:

```javascript
import './bootstrap';
import { initCarScene } from './car-scene.js';

window.addEventListener('DOMContentLoaded', () => {
    // Check if threejs target container is present
    const container = document.getElementById('three-car-container');
    if (container) {
        initCarScene('three-car-container', (zoneId) => {
            if (typeof window.setZone === 'function') {
                window.setZone(zoneId);
            }
        });
    }
});
```

- [ ] **Step 4: Update Blade View**

In `resources/views/welcome.blade.php`, find the `<div class="carbox">` tag (approx line 346) and replace its internal `<svg>` with:

```html
<div class="carbox" style="position: relative;">
  <div id="three-car-container" style="width: 100%; height: 240px; background: #fff;"></div>
  {{-- Hidden backup SVG here --}}
  <div id="fallback-svg" style="display: none;">
     {{-- original svg contents --}}
  </div>
  <p class="legend">Seret untuk memutar mobil 3D · Klik atau ketuk zona mobil untuk melihat detail</p>
</div>
```

Expose the global `setZone` function in the script block:
```javascript
window.setZone = setZone;
```

- [ ] **Step 5: Run dev server and verify scene**

```bash
npm run dev
```
Open `http://localhost:8000`. Inspect the element — verify a white WebGL canvas with a grid appears.

- [ ] **Step 6: Commit**

```bash
git add .
git commit -m "feat: setup Three.js scene and canvas"
```

---

### Task 2: Build Procedural 3D Car Wireframe Model

**Files:**
- Modify: `resources/js/car-scene.js`

**Interfaces:**
- Produces: 3D wireframe car model added to the scene.

- [ ] **Step 1: Define car parts geometries**

Open `resources/js/car-scene.js`. Add a function to build the procedural car wireframe:

```javascript
function createProceduralCar() {
    const carGroup = new THREE.Group();

    // Materials
    const bodyMat = new THREE.MeshBasicMaterial({ color: 0x0D1B24, wireframe: true });
    const wheelMat = new THREE.MeshBasicMaterial({ color: 0xF5A623, wireframe: true });

    // Lower body box
    const bodyGeom = new THREE.BoxGeometry(4.5, 1.0, 1.8);
    const bodyMesh = new THREE.Mesh(bodyGeom, bodyMat);
    bodyMesh.position.y = 0.2;
    carGroup.add(bodyMesh);

    // Upper cabin box
    const cabinGeom = new THREE.BoxGeometry(2.2, 0.8, 1.6);
    const cabinMesh = new THREE.Mesh(cabinGeom, bodyMat);
    cabinMesh.position.set(-0.2, 1.1, 0);
    carGroup.add(cabinMesh);

    // Wheels (4 cylinders)
    const wheelGeom = new THREE.CylinderGeometry(0.5, 0.5, 0.4, 12);
    wheelGeom.rotateX(Math.PI / 2);

    const positions = [
        [-1.3, 0.0, 0.9],  // Front Left
        [1.3, 0.0, 0.9],   // Rear Left
        [-1.3, 0.0, -0.9], // Front Right
        [1.3, 0.0, -0.9]   // Rear Right
    ];

    positions.forEach(pos => {
        const wheel = new THREE.Mesh(wheelGeom, wheelMat);
        wheel.position.set(pos[0], pos[1], pos[2]);
        carGroup.add(wheel);
    });

    return carGroup;
}
```

- [ ] **Step 2: Add car to the scene**

In `initCarScene()`, add the car group to the scene:

```javascript
const car = createProceduralCar();
scene.add(car);
```

- [ ] **Step 3: Test rendering**

Verify the 3D grid has a wireframe car sitting on it.

- [ ] **Step 4: Commit**

```bash
git add .
git commit -m "feat: draw procedural 3D wireframe car"
```

---

### Task 3: Animated Scanning Laser Line

**Files:**
- Modify: `resources/js/car-scene.js`

**Interfaces:**
- Produces: Glowing scanning laser bar traversing the length of the 3D car back and forth.

- [ ] **Step 1: Create scanning laser geometry**

Add a scanning plane to `initCarScene()`:

```javascript
// Red/amber glowing scanner bar
const scanGeom = new THREE.BoxGeometry(0.08, 1.8, 2.0);
const scanMat = new THREE.MeshBasicMaterial({
    color: 0xF5A623,
    transparent: true,
    opacity: 0.8
});
const scanner = new THREE.Mesh(scanGeom, scanMat);
scene.add(scanner);
```

- [ ] **Step 2: Add scanner animation to render loop**

In `animate()`, update scanner position over time:

```javascript
let direction = 1;
// In animate loop:
const elapsed = clock.getElapsedTime();
scanner.position.x = Math.sin(elapsed * 1.5) * 2.3; // Moves from -2.3 to 2.3
```

- [ ] **Step 3: Verify animation**

Refresh client view. Check that a glowing yellow vertical slice oscillates back and forth along the wireframe car.

- [ ] **Step 4: Commit**

```bash
git add .
git commit -m "feat: animate scanning laser plane"
```

---

### Task 4: Interactive Hotspots & Raycasting

**Files:**
- Modify: `resources/js/car-scene.js`

**Interfaces:**
- Produces: Click events on zones mapping to `onZoneSelect(zoneId)`.

- [ ] **Step 1: Create interactive bounding boxes**

Add invisible bounding boxes representing the 5 zones of the car:

```javascript
const hotspots = [];

function addHotspot(scene, name, position, size, zoneIndex) {
    const geom = new THREE.BoxGeometry(size[0], size[1], size[2]);
    const mat = new THREE.MeshBasicMaterial({
        color: 0x1FAF5F,
        wireframe: true,
        visible: false // Hidden by default, made visible on hover
    });
    const mesh = new THREE.Mesh(geom, mat);
    mesh.position.set(position[0], position[1], position[2]);
    mesh.userData = { zoneIndex, name };
    scene.add(mesh);
    hotspots.push(mesh);
}

// In initCarScene():
// Zone 0: Eksterior & Rangka (entire body wrapper)
addHotspot(scene, "Eksterior", [0, 0.4, 0], [4.6, 1.1, 1.9], 0);

// Zone 1: Interior (cabin)
addHotspot(scene, "Interior", [-0.2, 1.1, 0], [2.3, 0.9, 1.7], 1);

// Zone 2: Mesin (engine compartment - front side)
addHotspot(scene, "Mesin", [-1.8, 0.5, 0], [1.0, 0.9, 1.7], 2);

// Zone 3: Kaki-kaki (wheels area)
addHotspot(scene, "Kaki-kaki", [0, -0.1, 0], [4.6, 0.5, 2.1], 3);

// Zone 4: Surat (floating book/plate symbol above car)
addHotspot(scene, "Dokumen", [0, 2.0, 0], [0.8, 0.6, 0.8], 4);
```

- [ ] **Step 2: Wire up Raycaster click events**

Implement selection detection:

```javascript
const raycaster = new THREE.Raycaster();
const mouse = new THREE.Vector2();

renderer.domElement.addEventListener('click', (event) => {
    // Calculate mouse position in normalized device coordinates
    const rect = renderer.domElement.getBoundingClientRect();
    mouse.x = ((event.clientX - rect.left) / rect.width) * 2 - 1;
    mouse.y = -((event.clientY - rect.top) / rect.height) * 2 + 1;

    raycaster.setFromCamera(mouse, camera);
    const intersects = raycaster.intersectObjects(hotspots);

    if (intersects.length > 0) {
        const clickedZone = intersects[0].object.userData.zoneIndex;
        // Highlight logic
        hotspots.forEach(h => h.material.visible = false);
        intersects[0].object.material.visible = true; // highlight clicked
        onZoneSelect(clickedZone);
    }
});
```

- [ ] **Step 3: Connect cursor styles**

Add hover styles:

```javascript
renderer.domElement.addEventListener('mousemove', (event) => {
    const rect = renderer.domElement.getBoundingClientRect();
    mouse.x = ((event.clientX - rect.left) / rect.width) * 2 - 1;
    mouse.y = -((event.clientY - rect.top) / rect.height) * 2 + 1;

    raycaster.setFromCamera(mouse, camera);
    const intersects = raycaster.intersectObjects(hotspots);
    renderer.domElement.style.cursor = intersects.length > 0 ? 'pointer' : 'default';
});
```

- [ ] **Step 4: Test click behavior**

Clicking different parts of the 3D car model should trigger `setZone()` and switch the inspection list sidebar on the right side of the screen.

- [ ] **Step 5: Commit**

```bash
git add .
git commit -m "feat: raycasting and zone interactions for 3D car"
```
