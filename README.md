# M2C EDGE‑Link – Universal Machine to Cloud Gateway

[![License](https://img.shields.io/badge/License-MIT-blue.svg)](LICENSE)
[![Platform](https://img.shields.io/badge/platform-Raspberry%20Pi-red)](https://www.raspberrypi.com/)
[![Node-RED](https://img.shields.io/badge/Node--RED-3.x-green)](https://nodered.org/)

**M2C EDGE‑Link** is an industrial IoT edge gateway developed in collaboration with **Central Manufacturing Technology Institute** and **ENMAZ Engineering Services Pvt. Ltd.** It connects CNC machines (SINUMERIK, FANUC, etc.) to the cloud using OPC UA, MQTT, FOCAS, MODBUS, or RS485.

> ⚠️ **This is the public documentation repository.**  
> The complete source code, production scripts, and customer‑specific configurations are kept in a **private repository**.  
> The content here is meant for learning, evaluation, and community contribution.

---

## Table of Contents

- [Overview](#overview)
- [Key Features](#key-features)
- [System Architecture](#system-architecture)
- [Hardware Requirements](#hardware-requirements)
- [Software Stack](#software-stack)
- [Installation Guide (Generic)](#installation-guide-generic)
  - [1. Raspberry Pi OS Setup](#1-raspberry-pi-os-setup)
  - [2. Install Node-RED](#2-install-node-red)
  - [3. MQTT Broker (Mosquitto)](#3-mqtt-broker-mosquitto)
  - [4. MQTT Bridge to Remote Broker (Example)](#4-mqtt-bridge-to-remote-broker-example)
  - [5. Python Virtual Environment & Dummy Bridge Script](#5-python-virtual-environment--dummy-bridge-script)
  - [6. Node-RED Flow Import (Example)](#6-node-red-flow-import-example)
  - [7. Kiosk Mode & Auto‑start](#7-kiosk-mode--auto-start)
- [Running the System](#running-the-system)
- [OEE Calculation](#oee-calculation)
- [Troubleshooting](#troubleshooting)
- [Repository Structure](#repository-structure)
- [Credits](#credits)

---

## Overview

M2C EDGE‑Link acts as a **bridge between shop‑floor machines and cloud/on‑prem servers**. It enables:

- Real‑time data acquisition from CNC controllers (e.g., Siemens SINUMERIK 840D SL)
- High‑frequency polling (down to 100ms intervals)
- Local edge processing (Python + Node‑RED)
- Persistent storage (MySQL/PostgreSQL) and cloud forwarding
- Intuitive dashboards with OEE analytics
- Optional 4‑channel vibration analysis (STM32 + 24‑bit ADC)

---

## Key Features

| Feature                     | Description                                                                 |
|-----------------------------|-----------------------------------------------------------------------------|
| **Multi‑protocol support**  | OPC UA, FOCAS, MODBUS, RS485, MQTT                                          |
| **Real‑time dashboards**    | Node‑RED Dashboard with role‑based access (Operator / Manager)              |
| **OEE engine**              | Automated calculation of Availability, Performance, Quality                |
| **Alarm & ticketing**       | Email‑based support ticketing system                                        |
| **Document upload**         | Attach job drawings and technical specs                                    |
| **Report generation**       | On‑demand OEE and production reports                                        |
| **Vibration analysis**      | STM32 + ADS131A04: time‑domain & FFT spectrum (optional)                   |
| **Kiosk mode**              | Dedicated display for shop‑floor dashboards                                |

---

## System Architecture

The system follows a **layered architecture**:

- **Equipment Layer** – CNC machines, PLCs, sensors  
- **Protocol Bridge** – Python scripts (OPC UA → MQTT)  
- **Message Broker** – Mosquitto (local pub/sub)  
- **Processing Layer** – Node‑RED flows (transformation, alarms, OEE)  
- **Storage Layer** – SQL databases (time‑series & config)  
- **Presentation Layer** – Node‑RED Dashboard / optional Grafana  
- **Cloud Layer** – Remote MQTT broker or REST API

---

## Hardware Requirements

- **Raspberry Pi 4** (4GB or 8GB RAM recommended) or **CM4**  
- MicroSD card (≥32 GB, Class 10)  
- 5V 3A USB‑C power supply  
- Ethernet or Wi‑Fi (static IP recommended)  
- *(Optional)* STM32 + ADS131A04 for vibration analysis

---

## Software Stack

| Component          | Version / Notes                               |
|--------------------|-----------------------------------------------|
| OS                 | Raspberry Pi OS (32‑bit, Debian Bookworm)     |
| Node.js            | 20.x (installed via Node‑RED script)          |
| Node‑RED           | 3.x (latest stable)                           |
| MQTT Broker        | Eclipse Mosquitto                             |
| Python             | 3.11 (with venv)                              |
| Python libs        | `opcua`, `paho-mqtt`, `numpy`                 |
| Database (opt)     | MySQL 8.0 or PostgreSQL 13+                   |
| Browser (kiosk)    | Chromium                                      |

---


