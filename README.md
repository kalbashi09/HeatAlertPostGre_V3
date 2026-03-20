# 🌡️ HEATSYNC (Heat Alert System) - Talisay City

**HEALERTSYS** is a backend-driven simulation platform designed to monitor and broadcast heat index alerts for the various barangays of **Talisay City, Cebu**.

Think of this API as a network of **virtual thermometers** scattered across the city, providing real-time simulated data to test emergency response, public health dashboards, and automated alerting systems.

---

## 🚀 System Architecture

- **Backend:** C# / .NET 10 (ASP.NET Core)
- **Database:** PostgreSQL (Hosted on Render)
- **Automation:** Background Worker Simulation (30-second intervals)
- **Alerting:** Telegram Bot Integration for automated broadcasting.
- **Security:** API Key protected endpoints ("The Bouncer" logic).

---

## 📡 API Integration Guide

### 1. Configuration

To prevent sensitive data leaks, store your credentials in a separate `config.js` file.

```javascript
// config.js
export const HEALERTSYS_CONFIG = {
  // Public Endpoint: No API Key required for GET requests
  apiURL: "https://backend-9lv5.onrender.com/api/live-heat-history",

  // Auto-refresh interval (30 seconds)
  refreshRate: 30000,
};
```

### 2. Fetching Live Data

All requests to the backend require an `X-API-KEY` header. Below is the optimized integration pattern for the frontend.

```javascript
/**
 * Fetches real-time heat data from the Public API.
 * No authentication required for GET requests.
 */

async function syncHeatData() {
  try {
    const response = await fetch(HEALERTSYS_CONFIG.apiURL, {
      method: "GET",
      headers: {
        Accept: "application/json",
        "X-Tunnel-Skip-Anti-Phishing-Page": "true",
      },
    });

    if (!response.ok) {
      throw new Error(`Server Error: ${response.status}`);
    }

    const data = await response.json();

    // Ensure we have a valid list of sensors
    if (Array.isArray(data)) {
      console.log("HeatSync Data Synced Successfully.");
      processHeatData(data);
    }
  } catch (error) {
    console.error("Fetch failed:", error.message);
  }
}
```

---

## 📊 Data Specifications

### Response Schema

| Field             | Type      | Description                                       |
| ----------------- | --------- | ------------------------------------------------- |
| `sensorCode`      | `string`  | Unique identifier (e.g., "TAL-01")                |
| `displayName`     | `string`  | Human-readable name of the station                |
| `barangayName`    | `string`  | Name of the location in Talisay City              |
| `environmentType` | `string`  | Classification (e.g., `Indoor`, `Outdoor-Sun`)    |
| `heatIndex`       | `int`     | Current recorded temperature in **°C**            |
| `lat` / `lng`     | `double`  | GPS Coordinates for map plotting                  |
| `isActive`        | `boolean` | Whether the sensor is currently reporting         |
| `date`            | `string`  | Human-readable date (e.g., "Mar 20, 2026")        |
| `time`            | `string`  | Human-readable local time (e.g., "05:56 PM")      |
| `rawTimestamp`    | `string`  | **ISO 8601 UTC timestamp** (Standard for sorting) |

### Alert Thresholds

| Status                 | Range       | Visual Indicator    | CSS Class        |
| ---------------------- | ----------- | ------------------- | ---------------- |
| **🚨 EXTREME DANGER**  | >= 49°C     | Crimson (`#c0392b`) | `extreme-danger` |
| **🔥 DANGER**          | 42°C - 48°C | Red (`#ff4757`)     | `danger`         |
| **⚠️ EXTREME CAUTION** | 39°C - 41°C | Orange (`#ffa502`)  | `caution`        |
| **✅ NORMAL**          | 29°C - 38°C | Green (`#2ed573`)   | `normal`         |
| **❄️ COOL**            | < 29°C      | Blue (`#3498db`)    | `cool`           |

---

## 💡 Implementation Notes for Developers

- **Batch DOM Updates:** When rendering the heat history table, avoid updating `innerHTML` inside a loop. Construct the full HTML string first and inject it once to maintain high browser performance.
- **Polling Frequency:** The simulation updates every 30 seconds. Polling every 10–15 seconds on the frontend is recommended to ensure UI freshness without overtaxing the server.
- **Spatial Interaction:** Use the `lat` and `lng` data to provide a `map.flyTo()` interaction when a user clicks a record in the UI.
- **🧊 Cold Starts:** Since the API is hosted on a free-tier instance, the first request may take up to 50 seconds to respond if the service has been idle. Subsequent requests will be near-instant.

---
