# 🔥 Fire Simulation with React Three Fiber

A realistic, interactive 3D fire simulation built with React Three Fiber (R3F). This project demonstrates a particle system for rendering dynamic fire effects, complete with physics, color transitions, and post-processing glow.

The fire's "energy" is adjustable via a slider, affecting spawn rates, velocity, spread, and lifetime for a natural, flickering flame—from a low ember to a roaring blaze.

---

## 🚀 Features

- **Particle System:** Up to 6,000 particles with position, velocity, lifetime, and color attributes.
- **Gaussian Distribution:** Used for natural spawning (tight jet at high energy, wide base at low).
- **Physics Simulation:** Buoyancy, turbulence, air drag, and random bursts for realistic motion.
- **Color Fading:** Particles transition from white/hot to red/ember based on lifetime.
- **Post-Processing:** Bloom effect for glowing embers and core.
- **Interactive Controls:** Energy slider (10–100%) and orbit camera.
- **Pure JS/Three.js:** No external shaders; everything in JSX and hooks.

---

## 📂 Project Structure
├──index.html              # Vite entry point <br>
├── package.json           # Dependencies and scripts <br>
├── package-lock.json      # Locked dependencies <br>
├── src/
│   ├── App.jsx              # Root component: simply renders the FireSimulation <br>
|   ├── main.jsx             # Entry point for React <br>
|   ├── index.css            # Global styles <br>
|   ├── components/ <br>
│   │   ├── FireParticles.jsx  # Core particle simulation logic <br>
│   │   └── FireSimulation.jsx # Scene setup with Canvas and effects <br>
|   ├── lib/ <br>
│   |   ├── colors.js          # Particle color computation <br>
│   |   ├── createParticle.js  # Particle spawning with energy scaling <br>
│   |   └── gaussian.js        # Gaussian random sampler (Box-Muller) <br>
|   └── <br>
└── README.md              # This file <br>

---

## 🧠 Theoretical Background

This simulation models fire as a collection of particles emitted from a base, rising due to heat, and fading out. Key concepts:

### 1. Particle System
Each particle has:
- **Position & Velocity:** Updated per frame with forces like buoyancy (updraft), turbulence (sine/cos noise for wind), and drag (friction).
- **Lifetime:** Determines when a particle "dies"; center particles live longer for a dense core.
- **Color:** Interpolated based on life fraction (hot white → cool red) with brightness fade.

### 2. Gaussian Distribution (Spawning)
Particles spawn with positions sampled from a normal distribution for organic clustering:
$$ x \sim \mathcal{N}(\mu, \sigma^2) $$
- $\mu = 0$ (burner center).
- $\sigma$ varies with energy: small for focused flames, large for spread-out bases.
Implemented via Box-Muller transform for efficiency.

### 3. Energy Scaling
- **Low Energy (10%):** Wide, short flame with slow particles and quick fade (ember-like).
- **High Energy (100%):** Tall, narrow jet with fast rise and longer life (intense fire).
Formulas ramp quadratically (e.g., `Math.pow(energy, 2)`) for non-linear intensity.

### 4. Rendering
- **Points Material:** Additive blending for glow, vertex colors for per-particle tint.
- **Bloom Post-Processing:** Enhances bright areas:
  $$ I_{bloom} = \max(0, I - threshold)^{smoothing} $$
- Optimizations: Dynamic draw range to render only live particles.

---

## 📦 Installation & Usage

### 1. Prerequisites
- Node.js (v18+ recommended)

### 2. Install Dependencies
```bash
npm install
```
### 3. Run Development Server
```bash
npm run dev
```

