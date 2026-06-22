# SAATH CHALO BACKEND SPECIFICATION

## Project Overview

Saath Chalo is an AI-powered mobility companion for visually impaired individuals.

The system provides:

* Real-time obstacle detection
* Environmental awareness
* Voice-first interaction
* Hazard prioritization
* Route memory
* Community intelligence
* Emergency assistance

Frontend is already implemented in Flutter.

The backend should be built using:

* FastAPI
* Python
* Supabase
* PostgreSQL
* WebSockets
* YOLOv8
* OpenCV
* Gemini API
* pgvector
* Firebase Cloud Messaging
* Twilio

Backend architecture should follow clean architecture principles.

---

# Folder Structure

backend/

app/

├── main.py

├── api/
│
│   ├── walk.py
│   ├── ai.py
│   ├── memory.py
│   ├── community.py
│   ├── emergency.py
│
├── services/
│
│   ├── vision_service.py
│   ├── audio_service.py
│   ├── context_service.py
│   ├── navigation_service.py
│   ├── memory_service.py
│   ├── community_service.py
│   ├── emergency_service.py
│
├── websocket/
│
│   ├── manager.py
│
├── database/
│
│   ├── supabase.py
│
├── models/
│
│   ├── user.py
│   ├── route.py
│   ├── landmark.py
│   ├── hazard.py
│   ├── walk_session.py
│
├── schemas/
│
│   ├── walk.py
│   ├── ai.py
│   ├── emergency.py
│
├── core/
│
│   ├── config.py
│
├── utils/
│
│   ├── gps.py
│   ├── helpers.py
│
└── tests/

---

# FILE RESPONSIBILITIES

## main.py

Application entry point.

Responsibilities:

* Initialize FastAPI
* Register routes
* Configure middleware
* Configure websocket manager
* Configure startup and shutdown events

Should import:

* walk router
* ai router
* community router
* emergency router

---

# API LAYER

## api/walk.py

Purpose:

Controls walk sessions.

Endpoints:

POST /walk/start

POST /walk/stop

GET /walk/status

Responsibilities:

* Create walk session
* Start monitoring mode
* Connect websocket stream
* Return session state

Connected Flutter Screen:

Walk Mode

---

## api/ai.py

Purpose:

Voice assistant endpoints.

Endpoints:

POST /ask

POST /detect

Responsibilities:

* Receive user queries
* Call Gemini
* Return natural language response

Example:

Input:

"What's around me?"

Output:

"Bus stop ahead. Moderate traffic."

Connected Flutter Screen:

AI Assistant Screen

---

## api/memory.py

Purpose:

User memory and route learning.

Endpoints:

GET /routes

GET /landmarks

GET /history

Responsibilities:

* Return saved routes
* Return learned landmarks
* Return walk history

Connected Flutter Screen:

Route Memory Screen

---

## api/community.py

Purpose:

Community hazard sharing.

Endpoints:

POST /report-hazard

GET /nearby-hazards

Responsibilities:

* Store reported hazards
* Return nearby hazards

Connected Flutter Screen:

Community Map

---

## api/emergency.py

Purpose:

Emergency actions.

Endpoints:

POST /sos

POST /share-location

Responsibilities:

* Trigger emergency workflow
* Notify contacts
* Share live location

Connected Flutter Screen:

Emergency Screen

---

# SERVICE LAYER

## vision_service.py

Purpose:

Computer Vision Engine.

Input:

Camera frames.

Responsibilities:

Run YOLOv8 detection.

Detect:

* Cars
* Bikes
* Autos
* People
* Animals
* Potholes
* Barricades
* Footpaths
* Crosswalks

Output Example:

{
"object": "bike",
"distance": 5,
"direction": "right"
}

Used By:

context_service.py

This is the most critical service.

---

## audio_service.py

Purpose:

Environmental audio analysis.

Input:

Microphone stream.

Detect:

* Horns
* Sirens
* Traffic
* Emergency vehicles

Models:

YAMNet

TensorFlow

Output Example:

{
"event": "horn"
}

Used By:

context_service.py

---

## context_service.py

Purpose:

Core intelligence engine.

This is the primary innovation layer.

Input:

Vision output.

Audio output.

GPS data.

Responsibilities:

* Threat prioritization
* Hazard ranking
* Context generation
* Information filtering

Example:

Input:

Bike detected.

Distance 5 meters.

Direction right.

Output:

"Bike approaching from your right."

The service should decide:

* What should be spoken
* What should be ignored
* Priority levels

Priority Levels:

HIGH

MEDIUM

LOW

Only HIGH should be immediately announced.

---

## navigation_service.py

Purpose:

Location awareness.

Responsibilities:

* Route guidance
* Landmark lookup
* Bus stop detection
* Crossing awareness

Integrations:

Google Maps API

Places API

OpenStreetMap

Output Example:

{
"landmark":"Bus Stop",
"distance":"50m"
}

---

## memory_service.py

Purpose:

Personal AI memory.

Responsibilities:

Store:

* Frequent routes
* Learned landmarks
* Walking preferences
* User habits

Example:

User often visits tea stall.

User usually travels Home → College.

Database:

Supabase

PostgreSQL

pgvector

---

## community_service.py

Purpose:

Accessibility network.

Responsibilities:

Store:

* Construction reports
* Broken footpaths
* Open drains
* Accessibility ratings

Return nearby community hazards.

Future Goal:

Build accessibility heatmap.

---

## emergency_service.py

Purpose:

Panic and emergency workflows.

Responsibilities:

* SOS
* Location sharing
* Contact notifications

Integrations:

Twilio

Firebase Cloud Messaging

Example:

User presses SOS.

Location shared instantly.

Contacts notified.

---

# WEBSOCKET LAYER

## websocket/manager.py

Purpose:

Real-time communication.

Responsibilities:

* Manage active connections
* Broadcast hazard alerts
* Send immediate notifications

Example Event:

{
"type":"hazard",
"message":"Pothole ahead"
}

Flutter should maintain websocket connection while walk mode is active.

No polling should be used.

All alerts must be pushed.

---

# DATABASE LAYER

## database/supabase.py

Purpose:

Single Supabase client.

Responsibilities:

* Database connection
* Authentication access
* Query helpers

All services should use this file.

---

# MODELS

## user.py

Fields:

id

name

email

language

voice_preference

created_at

---

## route.py

Fields:

id

user_id

source

destination

frequency

last_used

---

## landmark.py

Fields:

id

user_id

name

latitude

longitude

category

---

## hazard.py

Fields:

id

hazard_type

latitude

longitude

reported_by

created_at

---

## walk_session.py

Fields:

id

user_id

started_at

ended_at

distance

hazards_detected

---

# SCHEMAS

## walk.py

Request and response validation.

Walk Start Request.

Walk Status Response.

---

## ai.py

AI Request.

AI Response.

---

## emergency.py

SOS Request.

Location Share Request.

---

# CORE

## config.py

Responsibilities:

Environment variables.

API keys.

Supabase keys.

Twilio keys.

Gemini keys.

Google Maps keys.

---

# UTILS

## gps.py

GPS helper methods.

Distance calculations.

Location utilities.

---

## helpers.py

Reusable utility functions.

Formatting.

Validation.

Conversions.

---

# DATABASE TABLES

users

routes

landmarks

walk_sessions

hazards

community_reports

emergency_contacts

emergency_events

user_preferences

memory_embeddings

---

# FLUTTER INTEGRATION

Home Screen

POST /walk/start

Walk Screen

WebSocket connection

Voice Assistant

POST /ask

Route Memory

GET /routes

Community Map

GET /nearby-hazards

SOS Screen

POST /sos

---

# MVP IMPLEMENTATION PRIORITY

Phase 1

1. FastAPI Setup
2. Supabase Integration
3. WebSocket Manager
4. Vision Service
5. Context Service
6. Walk API
7. Flutter Integration

Phase 2

1. Audio Service
2. Navigation Service
3. Memory Service

Phase 3

1. Community Intelligence
2. Emergency Layer

Phase 4

1. Smart Cane Integration
2. Smart Pendant Integration
3. Smart Glasses Integration

Code should be modular, production-ready, type-safe, documented, and follow clean architecture principles.
