# Anime City Sandbox (3D Web Game)

A complete, fully playable, mobile-first 3D sandbox game built entirely within a single `index.html` file using HTML, CSS, JavaScript, and Three.js (r128). Designed with a vibrant, low-poly anime aesthetic inspired by Tier-2/Tier-3 Indian cities.

---

## 🎮 Gameplay Features

### 1. World & Environment
*   **Procedural City Grid:** A structured grid of light-gray asphalt roads with neat pedestrian footpaths, surrounded by compound walls to keep the player inside.
*   **Hollow Houses:** Buildings are constructed with strict wall colliders but feature open doorways, allowing players and NPCs to walk inside. Houses feature emissive windows and interior floors.
*   **Procedural Trees:** Low-poly trees generated entirely out of stacked, rotated, and scaled `THREE.BoxGeometry` (no spheres). Randomized heights, trunk thicknesses, and vibrant green hues.
*   **Day/Night Cycle:** A toggle button to switch between a bright sunny day and a moody night. Night mode triggers car headlights and building window glows.

### 2. Player Mechanics (On Foot)
*   **First-Person View:** The camera is placed at eye level with a visible floating "fist" attached to it.
*   **Movement:** Virtual joystick movement relative to the camera direction.
*   **Actions:** 
    *   **Sprint (Toggle):** Infinite run toggle.
    *   **Jump:** Gravity-based jumping mechanics.
    *   **Punch:** Strikes forward, plays a sound, and knocks back NPCs with a physics impulse.
*   **Dialogues:** The player character occasionally says contextual phrases while walking or driving.

### 3. Vehicle Mechanics (Driving)
*   **Real-time Transition:** Walking near the car spawns a "Drive" button. Pressing it seamlessly transitions the player into the driver's seat.
*   **Controls:** 
    *   **Gas Pedal:** Hold to accelerate forward.
    *   **Brake/Reverse Pedal:** Hold to brake. If stopped, holding it reverses the car.
    *   **Steering Wheel:** A dedicated, draggable steering wheel UI on the right side of the screen dictates the car's turning angle.
*   **Car Physics:** Gradual acceleration, natural friction, speed-based steering scaling, and functional brake/reverse tail lights.
*   **Headlights:** Translucent, low-poly cone beams project from the car at night.
*   **Camera:** A locked, leveled 3rd-person follow camera that matches the car's orientation without tilting/rolling.

### 4. NPC AI & Physics
*   **Cube-Based Design:** All NPCs (students/pedestrians) are built strictly from low-poly boxes (head, torso, limbs) with walk cycle animations.
*   **Wandering AI:** NPCs pick random targets and walk around the city and inside houses.
*   **Interaction:** The single closest NPC will approach the player if they get within 30 meters. Upon reaching the player, the NPC stops, looks at them, and plays a talking animation with a floating dialogue bubble for 10 seconds.
*   **Impact Physics:** If hit by a car, NPCs are launched upward and backward, arching through the air under gravity, and land cleanly on the ground before resuming their AI.

### 5. UI & Customization
*   **Multi-Touch Support:** Flawless handling of simultaneous joystick, steering wheel, and camera look-drag inputs using strict Touch ID tracking.
*   **Glassmorphism UI:** Sleek, translucent buttons with SVG icons.
*   **Custom Control Panel:** A gear icon opens a settings menu where players can:
    *   Scale all UI buttons from 50% to 150%.
    *   Toggle "Edit Mode" to drag and place buttons anywhere on the screen for both On-Foot and Driving layouts.
*   **Dynamic Speedometer:** A digital speedometer appears while driving.

---

## 🛠️ Technical Challenges & Solutions Faced

### 1. Performance & Lag
*   **Problem:** Severe FPS drops due to multiple dynamic `PointLights` inside houses and heavy collision calculations looping over every wall for every NPC.
*   **Solution:** Removed all `PointLights` and replaced them with cheap `emissive` materials. Optimized collision detection to only check bounding boxes within a 40-unit radius of the entity.

### 2. Multi-Touch Conflicts
*   **Problem:** The joystick, camera look-drag, and steering wheel were fighting for control. Touching the joystick sometimes triggered the camera, and buttons were stealing touch events.
*   **Solution:** Rewrote the global touch handler to track specific `touch.identifier` IDs. The joystick, steering wheel, and look-area now exclusively bind to the first finger that touches them, allowing simultaneous use. Action buttons use isolated `touchstart`/`touchend` listeners that stop propagation.

### 3. Camera Clipping & Rolling
*   **Problem:** The camera would clip into the ground when looking down, and the horizon would tilt (roll) when rotating.
*   **Solution:** Enforced a strict `camera.up.set(0, 1, 0)` and clamped the pitch (`camPitch`) to a safe range. Added a minimum height clamp so the camera Y-position never drops below 1.0.

### 4. Invisible Buttons & UI State
*   **Problem:** Buttons were disappearing or not appearing when switching between driving and walking states.
*   **Solution:** Created strict `showFootUI()` and `showCarUI()` functions that explicitly set `display: flex` or `display: none` for every UI element upon state transition.

### 5. Trees Spawning Inside Houses
*   **Problem:** Procedurally scattered trees were sometimes spawning inside the hollow houses.
*   **Solution:** Implemented an `isInsideHouse(x, z)` bounds check using `THREE.Box3` before generating a tree, ensuring trees only spawn on open ground or designated park areas.

---

## 🚀 How to Run
1. Ensure you have the `index.html` file in a local directory.
2. If using Termux, start a local server: `python3 -m http.server 8080`
3. Open your mobile/browser and navigate to `http://localhost:8080` (or your PC's local IP if testing on a separate mobile device).
4. Tap the screen to enter Fullscreen mode for the best experience.
