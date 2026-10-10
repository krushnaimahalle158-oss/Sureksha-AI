# Pune Intelligent Emergency Response & Disaster Command Center

An intelligent emergency response and flood disaster mitigation command center for Pune, built in pure vanilla JavaScript with Leaflet and OpenStreetMap.

---

## 🌟 Key Capabilities

### 1. Real Data Integration
- **OpenStreetMap Road Network (Overpass API)**: Queries major arterial and collector highways (`motorway`, `trunk`, `primary`, `secondary`, `tertiary`) across central Pune, building an in-memory routing graph in JavaScript. Automatically caches the graph in `localStorage` for instant loading on subsequent refreshes, and includes a built-in connected network of 80+ Pune arterials as a zero-latency fallback.
- **Real Pune Tertiary Hospitals (Overpass API)**: Real hospitals (Sassoon General, Ruby Hall, Jehangir, KEM Hospital, Deenanath Mangeshkar, Sancheti, Sahyadri, Poona Hospital, Bharati Hospital) with simulated clinical beds, ICU capacities, and specialties (`trauma`, `cardiac`, `maternity`). Includes live clinical turnover drift.
- **Live Weather & Precipitation (Open-Meteo API)**: Fetches live rainfall rates for Pune to drive dynamic flood inundation expansion.

### 2. Hazards & Road Closures
- **Expanding Mula-Mutha Flood Hazard**: Dynamic flood polygons modeled along the Mula and Mutha river corridors (from Vitthalwadi through Lakdi Pul, Sangam, Bund Garden, and Yerawada). Displays an active hazard polygon and a +30 min forecast zone (lighter amber wash).
- **Automated Road Inundation**: Roads inside the active flood zone turn red (closed) and are removed from the routing graph. Roads in the forecast zone turn amber (at risk) and receive routing penalties.
- **Interactive "Block a Road / Hazard" Tool**: Allows manual toggling of individual road segments, or dropping a 300-meter circular hazard blockade anywhere on the map with a click.

### 3. Optimization Engine
- **Dynamic Dijkstra / A* Routing**: Live graph search with real-time in-transit vehicle rerouting whenever a road segment closes ahead.
- **Greedy Max-Coverage Staging**: Repositions idle ambulances to strategic staging hubs across Pune (Shivajinagar, Deccan, Swargate, Camp, Bund Garden, Kothrud) to maximize 8-minute demand reachability while actively avoiding flood zones.
- **Multi-Factor Clinical Triage**: Scores hospitals using:
  $$\text{Score} = (\text{ETA}_{\text{min}} \times 1.6) + (\text{Occupancy}\% \times 0.75) - (\text{SpecialtyBonus}) + (\text{OverloadPenalty}) + (\text{RouteFloodRisk})$$
  Logged with human-readable rationale in the Operations Event Feed.
- **Evacuation Corridor Routing**: Routes at-risk riverbank populations to 4 designated Pune shelters (Balgandharva Kalakshetra, Nehru Stadium, SSPMS Ground, Camp Community Hall).
- **Temporary Medical Facility Identification**: Detects sectors with $>15\text{ min}$ emergency care gaps (due to river bridge closures) and recommends Field Hospital and Mobile Clinic locations.

### 4. Interactive Simulation & Metrics
- **Live KPIs**: Average Response Time, Active Cases, Waiting Calls, Resolved Incidents, Reroutes, and Roads Cut.
- **Smart vs. Naive Mode Comparison**: Toggle between the AI multi-factor optimizer and a naive nearest-hospital baseline with a live comparative impact table.
- **Forecast / Rainfall Slider**: Adjust timeline from T+0 to T+60 min (rainfall intensity scaling).
- **Layer Controls**: Toggle Roads, Hazards, Ambulances, Hospitals, Evacuations, and Field Sites.

---

## 🚀 How to Run Locally

### Option 1: VS Code Live Server (Zero Build Step)
1. Open the project folder in VS Code.
2. Right-click [index.html](file:///C:/Users/USER/OneDrive/Documents/disaster_response_system/index.html) and select **"Open with Live Server"**.
3. The dashboard will load and start running immediately at `http://127.0.0.1:5500/index.html`.

### Option 2: Direct File Open
Double-click [index.html](file:///C:/Users/USER/OneDrive/Documents/disaster_response_system/index.html) in your file explorer to open it in any modern browser.

### Option 3: FastAPI Local Backend (Optional)
```powershell
.\venv\Scripts\python -m uvicorn main:app --host 127.0.0.1 --port 8000 --reload
```
Navigate to `http://127.0.0.1:8000`.

---

## 📂 Code Organization in `index.html`

- **Module 1: Data & APIs**: Open-Meteo weather fetcher, Overpass API road/hospital parsers, local storage caching, and Pune fallback network.
- **Module 2: Hazards & Roads**: Mula-Mutha river flood geometry, buffer generation, interactive click-to-block tool, and road cut state tracker.
- **Module 3: Routing Engine**: Haversine metric calculations, Dijkstra shortest-path solver, and dynamic vehicle rerouting.
- **Module 4: Optimization Engine**: Multi-factor hospital scoring, greedy max-coverage staging, and medical gap detectors.
- **Module 5: Simulation Engine**: 10 FPS animation loop, 108 emergency call generation, ambulance state machines, and KPI trackers.
- **Module 6: UI & Map Visuals**: Leaflet layers, layer toggles, timeline slider, hospital capacity bars, and event feed.
