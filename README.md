# MineSight: Fog-Resilient Safety System for Mine Vehicles

> **Smart India Hackathon 2026 Submission**  
> **Problem Statement ID:** 26007  
> **Problem Statement Title:** Safe and Efficient Operation of Mine Vehicles in Fog and Low-Visibility Conditions in Open Cast Iron Ore Mines  
> **Theme:** Smart Automation | **Category:** Hardware  
> **Team Name:** Lost in transmission (Team ID: 176278)  

---

## 📌 Executive Summary

**MineSight** is an edge-based, 3D mmWave radar + thermal fusion safety system featuring V2V cooperative perception. It enables heavy earth-moving machinery (HEMMs) in open-cast iron ore mines to navigate safely and efficiently through severe monsoon fog, dust, and zero-visibility conditions without relying on central networks or optical LiDAR.

---

## 🚩 Problem & Regulatory Context

* **Regulatory Halts:** DGMS mandates force fleet shutdowns during low visibility, delaying millions of tons of ore haulage annually.
* **Sensor Failure:** Standard 3D LiDAR fails in dense fog due to visible light scattering.
* **Operational Paralysis:** Monsoon fog reduces visibility to 3–5 meters, halting daily operations for 4+ hours at a time.
* **Fatal Terrain Hazards:** Low visibility turns unpaved roads with 8–10% gradients and 50–100m drop-off cliffs into high-risk zones.

---

## 🛠️ Key Technical Features

1. **Fog-Independent Obstacle Confirmation:** Cross-validates 60 GHz mmWave radar point clouds with thermal IR signatures to eliminate false positives in dense fog.
2. **Cooperative V2V Hazard Sharing:** Broadcasts real-time obstacle and hazard locations to approaching vehicles over a decentralized LoRa mesh network (1 km range).
3. **Predictive TTC Risk Assessment:** Continuously calculates Time-To-Collision (TTC) using relative velocity and distance to trigger progressive driver warnings or automated emergency braking.
4. **Risk-Adaptive Speed Advisory:** Dynamic dashboard provides safe operating speed guidance (e.g., 28 km/h) rather than simple binary warnings.
5. **Cliff & Ground Trend Prediction:** Uses downward-pointing ultrasonic backup sensors and IMU elevation data to detect cliff edges 20m+ in advance.

---

## 🏗️ System Architecture & Hardware Stack

### On-Vehicle Hardware Components
* **60 GHz mmWave Radar:** Ai-Thinker Rd-60 (3D point cloud, Doppler processing, 120° H/V FOV)
* **Thermal IR Camera:** Melexis MLX90640 ($32 \times 24$ thermal array frame)
* **9-Axis IMU:** TDK InvenSense ICM-20948 (Ego-motion estimation, acceleration, pitch, yaw)
* **Cliff/Ground Ultrasonic Backup:** JSN-SR04T Waterproof Ultrasonic Sensor
* **V2V Transceiver:** LoRa SX127x Transceiver (868/915 MHz, AES-GCM encryption)
* **Real-Time Processing:** Teensy 4.0 MCU (Parallel sensor polling at 10 Hz, hard reflex path for emergency braking)
* **Perception & AI Processing:** Raspberry Pi 4 Compute Module
* **Positioning:** dGPS Module with Patch Antenna

### Processing & AI Pipeline
1. **Ego-Motion Compensation:** Cancels host vehicle movement from radar data using IMU inputs.
2. **Clutter Filtering & DBSCAN:** Groups 3D radar point clouds and rejects static noise.
3. **Thermal-Radar Validation:** Matches thermal azimuth angles with radar targets via OpenCV.
4. **Target State Estimation:** Extended Kalman Filter (EKF) for smoothed range, bearing, and velocity tracking.
5. **Deterministic Safety Engine:**
   * **TTC < 2s / Cliff Edge:** Hard reflex emergency brake trigger.
   * **TTC 2–5s:** Warning + brake assist.
   * **Worker Target (< 10m):** Haptic & audio alert.

---

## 📡 Tech Stack

* **Core Languages:** C / C++, JavaScript
* **Libraries & Frameworks:** OpenCV, WebSockets, React, painlessMesh
* **Hardware Architectures:** ARM Cortex, Teensy, Raspberry Pi
* **Communication Protocols:** LoRa V2V Mesh, TLS 1.3 (Central Gateway)

---

## 📊 Business & Operational Impact

| Metric / Benefit | Operational / Economic Value |
| :--- | :--- |
| **Haulage Continuity** | Prevents complete fleet shutdowns during heavy fog, supporting target ore haulage capacity. |
| **Safety & Compliance** | Aligns with DGMS safety guidelines for heavy earth-moving machinery. |
| **Driver Assistance** | Reduces cognitive fatigue through dynamic 3D obstacle dashboards and early hazard warnings. |
| **Decentralized Reliability** | Local safety mechanisms remain fully operational even if LoRa V2V or central mine servers fail. |

---

## 📖 Research References & Datasheets

* **Ai-Thinker Rd-60 Radar:** 60 GHz FMCW point-cloud radar module.
* **Melexis MLX90640:** Thermal imaging array. [Datasheet](https://www.melexis.com/en/product/mlx90640/)
* **Academic Paper:** *Image Translation from Thermal to RGB for Vehicle Detection in Low-Light Conditions* ([arXiv:2209.09808](https://arxiv.org/pdf/2209.09808))
* **Adverse Weather Perception:** *Enhancing Radar Point Clouds in Adverse Weather* ([arXiv:2404.17229](https://arxiv.org/html/2404.17229v1))
* **LoRa V2V Mesh Framework:** [painlessMesh GitHub Repository](https://github.com/gmag11/painlessMesh)
