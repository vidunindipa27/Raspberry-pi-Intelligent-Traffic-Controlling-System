# Intelligent Traffic Controlling System (ITCS)

An adaptive, distributed traffic signal management prototype designed to minimize urban intersection congestion using **Edge Computer Vision (YOLOv8n)**, **MQTT networking**, and **Deep Reinforcement Learning (PPO)**.

*Department of Electronic and Telecommunication Engineering, University of Moratuwa (Group 16)*

---

## 📌 Overview

Traditional traffic lights operate on fixed-time, pre-programmed cycles that allocate equal green-light durations regardless of real-time lane occupancy—forcing drivers in congested lanes to wait while empty lanes hold a green signal. 

This system solves that bottleneck by measuring real-time vehicle density across intersection approaches and dynamically optimizing signal phases to minimize overall vehicle waiting time.


---

## 📂 Project Documentation & Media
* **Full Project Report:** [View PDF Report](docs)
* **Video Demonstration:** [Watch System Walkthrough](https://drive.google.com/file/d/15IVnpSH51FrOGG6u1i-EfTBYHr95rVaP/view?usp=sharing)

---

## 🏗️ System Architecture

The system uses a **Master–Slave distributed architecture** over TCP/IP to decouple heavy edge video processing from central decision-making:

1. **Observe (Slave / Edge Nodes):** Dedicated Raspberry Pi 4 units capture live lane feeds via USB cameras and run **YOLOv8n** locally to count vehicles (cars, buses, trucks, bikes). Counts are published to an MQTT topic (e.g., `traffic/sensors/north`).
2. **Decide (Master Node):** A central Raspberry Pi 4 hosts the **MQTT Broker** and runs a pre-trained **Proximal Policy Optimization (PPO)** Reinforcement Learning agent. It aggregates lane densities and selects the optimal next signal phase, publishing commands to `control/target_phase`.
3. **Act (Hardware Actuation):** Slave nodes subscribe to the control topic and drive a 9-LED directional traffic signal array (Red/Amber/Green across 3 lanes) via daisy-chained **74HC595 Shift Registers** to conserve GPIO pins.

---

## 🛠️ Hardware & Tech Stack

### Hardware
* **Compute:** Raspberry Pi 4 (Master & Slave Nodes)
* **Vision:** USB Cameras
* **Actuation & Enclosure:** 9-LED directional array controlled via daisy-chained **SN74HC595** 8-bit shift registers inside a custom 3D-printed housing

### Software & Simulation
* **Computer Vision:** Ultralytics YOLOv8n (Nano)
* **Reinforcement Learning:** Stable Baselines 3 (PPO) & Gymnasium
* **Traffic Simulation:** SUMO (Simulation of Urban Mobility)
* **Messaging & Networking:** MQTT over TCP/IP

---

## 📊 Implementation & Validation

* **Edge Vision Validation:** Verified real-time multi-vehicle detection and queue estimation using YOLOv8n on both live traffic video feeds and a physical scaled testbed.
* **Hardware Actuation:** Validated low-pin-count serial-to-parallel control of the 3-lane, 9-LED traffic display using 74HC595 shift registers.
* **SUMO Simulation Results:** Trained the PPO agent for **100,000 steps** in a custom 3-lane crossroad SUMO environment under stochastic traffic flows. In evaluation against unseen varying flow datasets, the RL controller dynamically adapted to traffic spikes and reduced average waiting times compared to fixed-cycle baselines.

---

## 🚀 Future Scope
* **Emergency Vehicle Prioritization:** Retraining the vision pipeline to detect ambulances/fire trucks and override RL phases to create an immediate green corridor.
* **Violation Detection:** Integrating Automatic Number Plate Recognition (ANPR) and virtual stop-lines for automated red-light enforcement.
* **Multi-Agent Reinforcement Learning (MARL):** Scaling from an isolated intersection to coordinated multi-junction city grids using long-range protocols (LoRaWAN / 5G).

---

## 👥 Team & Supervision
* **Team Members (Group 16):** V. I. Pelendagamage, W. A. C. Nirodha, P. R. S. D. N. Palihawadana, A. Y. D. Perera
* **Supervisor:** Dr. Sampath Perera
