# 🛠️ HEAT ALERT SYSTEM (HEALERTSYS) | CHANGELOG

All notable changes to this project will be documented in this file.
This project adheres to a custom iterative development cycle.

---

## [3.0.0] - 2026-03-20

### 🟠 CLOUD MIGRATION & PRO ADMIN (v2.0)

### ☁️ Infrastructure & Hosting

- **Render Deployment:** Migrated the C# Backend and Database from local XAMPP/MySQL to **Render (Web Services & Managed PostgreSQL)**.
- **PostgreSQL Transition:** Rewrote `DatabaseManager.cs` to use `Npgsql`, replacing MySQL syntax with Postgres-compatible queries and schemas.
- **SSL Enforcement:** Configured mandatory SSL connections for secure remote database access via pgAdmin 4.

### 🏢 Admin Dashboard (The "Dark Update")

- **UI Redesign:** Implemented a high-contrast **Slate/Navy Dark Theme** using Tailwind CSS for professional monitoring.
- **Sensor Management Hub:** Added a "Flexible Edit" sidebar allowing real-time updates to sensor metadata (Location, Barangay, Baseline Temp).
- **Environment Tagging:** Introduced specific environment classifications (`Indoor`, `Outdoor-Sun`, `Outdoor-Shade`, etc.) to improve heat index accuracy.
- **Dynamic State Toggling:** Integrated "Active/Inactive" switches to remotely enable or disable specific sensors in the registry.

### 🛡️ Security & Data Integrity

- **BCrypt Integration:** Switched from plain-text API keys to **BCrypt.Net-Next** password hashing for personnel authentication.
- **Partial Updates (PATCH):** Implemented a dynamic SQL builder for `UpdateSensorFlexible`, ensuring only modified fields are sent to the DB.
- **Registry Synchronization:** Implemented a Serial ID Sequence reset script to ensure local-to-cloud data migration doesn't cause primary key conflicts.

### 🗄️ Database Schema Updates

- **`authpersonnel` Table:** Created a new table for secure admin logins with `passcodehash` and `lastlogin` tracking.
- **`sensor_registry` Expansion:** Added `environment_type` and `is_active` columns to support the new Admin UI features.
- **Log Retention:** Upgraded the `CleanupOldLogs()` logic to handle a 300-record threshold (increased from 100).

---

## [2.0.0] - 2026-03-14

### 🔵 ADVANCED / INTEGRATED (C# Migration Complete)

### 🔒 Security & Connectivity

- **VS Code Dev Tunnels:** Integrated persistent public URLs (`.devtunnels.ms`) for remote frontend collaboration.
- **"The Bouncer" Auth:** Implemented `X-API-KEY` validation and Anti-Phishing bypass headers (`X-Tunnel-Skip-AntiPhishing-Page`).
- **CORS Policy:** Enabled Cross-Origin Resource Sharing for seamless browser-to-API communication.

### 🗺️ Geo-Spatial Refinement

- **Centroid + Bounding Box:** Replaced vertex-picking with Bounding Box center logic to ensure markers land in the "heart" of Barangays.
- **Safe-Zone Jitter:** Added a 20% random offset to prevent marker stacking on identical coordinates.
- **Ray-Casting Gatekeeper:** Maintained `IsPointInPolygon` as a final validation layer for coordinate accuracy.

### 🗄️ Database & Optimization

- **Anomalous Filtering:** Configured DB-level filters to ignore normal temperatures (29°C–38°C), archiving only danger/cool anomalies.
- **Auto-Rotation:** Implemented `CleanupOldLogs()` using LIMIT/OFFSET to maintain a 100-record cap, optimizing performance for i7-4700MQ hardware.
- **Manual Simulation:** Added a `/api/log-heat` POST endpoint to allow manual alert injections from the web.

---

## [1.1.0] - 2026-02-07

### 🟢 FUNCTIONAL / STABLE

### 🧠 Simulation Engine

- **Geo-Spatial Randomization:** First implementation of Long/Lat/Temp simulation within Talisay parameters.
- **Barangay Mapping:** Initial integration of `talisaycitycebu.json` for Min/Max coordinate bounds.
- **CPU Optimization:** Configured smart throttling for `while` loops to prevent local hardware lag.

### 🤖 Telegram Bot & UI

- **Rich-Text Notifications:** Added severity labels (Caution to Extreme Danger) and human-readable locations (e.g., "North-West of Bulacao").
- **Subscriber Analytics:** Added SQL-driven tracker to monitor active notification users.

---

## [1.0.0] - 2026-02-02

### 🚀 INITIAL RELEASE (XAMPP/C# Foundation)

### 🏗️ System Architecture

- **Dependency Setup:** Integrated `MySqlConnector`, `Telegram.Bot`, and `Newtonsoft.Json`.
- **Decoupled Logic:** Separated `BotAlertSender.cs` (Messaging) and `DatabaseManager.cs` (Persistence).

### ✅ Core Features

- **Bot Handshake:** Full C# Engine to Telegram API communication.
- **Subscription Service:** Implemented `/subscriberservice` and `/unsubscriberservice` commands.
- **Broadcast Engine:** Initial logic to fetch SQL chat IDs and broadcast high-heat triggers.

---
