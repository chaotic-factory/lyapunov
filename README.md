# lyapunov

### How to Build & Run**

```bash
 setup.py sdist bdist_wheel
```

1. **Install frontend dependencies & build:**

   ```sh
   cd lyapunov/frontend
   pnpm install
   pnpm run build
   ```

   (This puts built files in `dist/`.)

2. **Install Python package:**

   ```sh
   pip install .
   ```

3. **Run:**
   ```sh
   lyapunov --port 3000
   ```
   (Backend serves frontend at `/`, API at `/api/*`, WebSockets at `/ws/*`.), api docs at `/api/docs`

---

## Key Features

Lyapunov is a **realtime, interactive dashboard** for exploring nonlinear dynamics, recording multichannel time series, and training models. Here's what you can do:

### 🎯 Live Data Visualization

Experience real-time 2D time series and 3D phase-space trajectories with interactive controls.

![Overall playground view showing all features](./assets/lyapunov-light.png)

### 📊 Channel Control & Lyapunov Analysis

Toggle channels (X/Y/Z), adjust scale and interpolation, and monitor the live Lyapunov exponent with state classification (Stable / Periodic / Quasi-Periodic / Chaotic).

![Select channels, recording & playback buttons; Lyapunov exponent readout](./assets/lyapunov-dark.png)

### 📹 Recording & Playback

Record multichannel data with custom parameters (e.g., system parameters like Lorenz σ, ρ, β) and replay with floating playback controls.

### 🧠 Model Training

Train **SINDy** (Sparse Identification of Nonlinear Dynamics) or **Reservoir Computing** models directly from your recordings.

### 🔍 Analysis Tools

Visualize bifurcation diagrams, Poincaré maps, and discover equations from data.

---

**For detailed documentation, images, and walkthroughs, see:**
- 📖 **[Full Documentation](https://github.com/chaotic-factory/lyapunov/tree/main/docs/docs)** — Feature walkthrough with screenshots
- 🚀 **[Quickstart Guide](https://github.com/chaotic-factory/lyapunov/blob/main/docs/docs/index.md)** — Installation and first steps
- 💾 **[Backend API Reference](https://github.com/chaotic-factory/lyapunov/blob/main/docs/docs/index.md#5-backend-api-for-integrators)** — WebSocket streaming, training endpoints

---

### 6. **Device Streaming Example (ESP32 Arduino)**

```cpp
// ESP32 Arduino pseudocode
#include <WiFi.h>
#include <WebSocketsClient.h>

WebSocketsClient ws;

void setup() {
  WiFi.begin("SSID", "PASS");
  while (WiFi.status() != WL_CONNECTED) delay(500);
  ws.begin("backend_ip", 3000, "/ws/ingest/CHAOS-ESP32-01");
}

void loop() {
  int ch[2] = {analogRead(0), analogRead(1)};
  String payload = "{\"ts\": " + String(millis()/1000.0) + ", \"ch\": [" + String(ch[0]) + "," + String(ch[1]) + "], \"fs\": 20000}";
  ws.sendTXT(payload);
  ws.loop();
  delay(50);
}
```

---

### Patching and Releases

```
bump2version patch # → 0.2.2
bump2version minor # → 0.3.0
bump2version major # → 1.0.0
```

## **Summary**

- Just run `lyapunov --port 3000`: backend + frontend, ready for device streaming.
- Frontend served by backend (no need for a separate Node server in prod).
- Devices stream JSON over WebSocket, frontend displays live data.
- Flashing, device discovery, and streaming all included.

**Ask for any extension: graphs, auth, advanced UI, STM32 code, etc!**