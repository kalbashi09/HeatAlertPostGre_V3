## Hyper-Local Heat Index Monitoring: A Low-Cost Alert System for Vulnerable Urban Communities

**Subtitle:** _A High-Resolution Hyperlocal Heat Monitoring & Automated Alert Ecosystem_

### 📌 The Core Thesis

**HeAlertSys** is a full-stack environmental monitoring platform specifically engineered to address the "Urban Heat Island" effect in **Talisay City**. Unlike traditional weather apps that provide a single city-wide temperature, this system utilizes a distributed network of independent sensor nodes to provide **barangay-level granularity** in real-time.

---

### 🛠️ Key Architectural Pillars

#### 1. Dynamic Geospatial Visualization

The frontend utilizes **MapLibre GL** with a customized "Dark Matter" engine. It features a reactive **Radar-Node** interface where each sensor is represented by a "Pulsing Heat-Core." The pulse uses dynamic blur and scaling animations to visually communicate the intensity of the Heat Index at a glance, allowing officials to identify danger zones without reading a single line of text.

#### 2. The Cloud-Native Backend (C# & PostgreSQL)

- **Infrastructure:** Migrated from local storage to a high-availability **Render-hosted PostgreSQL** environment.
- **Data Integrity:** Implemented a relational **Sensor Registry** that separates sensor metadata (location, environment type, status) from the raw heat logs, ensuring the database remains lean and performant.
- **Public API Strategy:** Features a "Public-Read" GET endpoint for open-data transparency, while securing administrative actions (POST/PATCH) behind an **X-API-KEY** and BCrypt-hashed authentication.

#### 3. Automated Emergency Broadcasting (Telegram Bot)

The system bridges the gap between data and action via a **C# Telegram Bot**. It features:

- **Smart Subscriptions:** A boolean-state subscription model that allows residents to "mute" or "activate" alerts without losing their historical data.
- **Alarm Logic:** Automated broadcasting that triggers only when the Heat Index hits the "Danger" threshold ($39$°C+), preventing "notification fatigue" for users.
- **Heartbeat Reports:** Periodic summaries of the city's highest-risk locations sent directly to stakeholders.

#### 4. Administrative "Sensor-on-the-Move" Tech

The app includes a "Mobile Surveyor" mode, allowing field personnel to update a sensor’s GPS coordinates in the database directly from their smartphone's Telegram location sharing. This makes the system hardware-agnostic and highly portable for temporary event monitoring.

---

### 🎯 The Goal for the Researcher

"The goal of **HeAlertSys** is to transform environmental data from a passive statistic into an active life-saving tool. By combining **hyperlocal sensing** with **instantaneous broadcasting**, we reduce the response time between a heat spike and public awareness, specifically targeting vulnerable populations in high-density barangays."

---
