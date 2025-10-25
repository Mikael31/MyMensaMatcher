# Mensavoter - TUM Cafeteria Voting System

A web-based voting and information system for Technical University of Munich (TUM) cafeterias, featuring QR code navigation, menu displays, and interactive voting functionality.

## Overview

Mensavoter is a comprehensive web application that allows TUM students and staff to:
- View current menus for different TUM cafeterias
- Vote for their preferred cafeteria
- Navigate between different locations using QR codes
- View an interactive map of cafeteria locations


## Table of Contents

- [Overview](#overview)
- [Project Structure](#project-structure)
- [CPEE Process](#cpee-process)
- [Features](#features)
    - [1. Interactive Map](#1-interactive-map)
    - [2. Menu Display Pages](#2-menu-display-pages)
    - [3. Voting System](#3-voting-system)
    - [4. Menu Data Management](#4-menu-data-management)
    - [5. QR Code Navigation](#5-qr-code-navigation)
- [Setup Instructions](#setup-instructions)
    - [Installation](#installation)
    - [Quick Start](#quick-start)
- [API Endpoints](#api-endpoints)
    - [Voting API](#voting-api)
    - [Menu Data API](#menu-data-api)
    - [CPEE Integration](#cpee-integration)
- [QR Code System](#qr-code-system)
- [MapTiler API Integration](#maptiler-api-integration)
- [Styling and Design](#styling-and-design)
- [Data Sources](#data-sources)
    - [TUM Eat API](#tum-eat-api)
    - [Local Data Storage](#local-data-storage)
- [Supported Cafeterias](#supported-cafeterias)
- [Extendability](#extendability)
- [Development Notes](#development-notes)
    - [File Structure Details](#file-structure-details)


## Project Structure

```
mensavoter/
├── 📁 data/                      # JSON data used by the app
│   ├── mensa_bolzmann.json
│   ├── mensa_garching.json
│   └── mensa_maschinenbau.json
│
├── 📁 display/                   # HTML pages for displaying menus and map
│   ├── map.html
│   ├── mensabolzmann.html
│   ├── mensagarching.html
│   ├── mensamaschinenbau.html
│   └── test.html
│
├── 📁 jsonpasser/                # Handles menu data fetching and processing (PHP/Python logic)
│   ├── index.php
│   ├── mensa_bolzmann.php
│   ├── mensa_garching.php
│   ├── mensa_maschinenbau.py
│   └── (supporting scripts and logic)
│
├── 📁 pictures/                  # Background and asset images
│   ├── mensabolzmann.jpg
│   ├── mensagarching.webp
│   ├── mensamaschinenbau.jpg
│   └── TUM-Bild.jpg
│
├── 📁 qrcodes/                   # Generated QR codes for each mensa and voting page
│   ├── index.png
│   ├── map.png
│   ├── maschinenbau.png
│   ├── mensabolzmann.png
│   ├── mensabolzmannvote.png
│   ├── mensagarching.png
│   ├── mensagarchingvote.png
│   └── mensamaschinenbauvote.png
│
├── 📁 styles/                    # Styling and layout
│   └── style.css
│
├── 📁 votes/                     # Voting backend endpoints
│   ├── data/
│   ├── reset_votes.php
│   └── vote.php
│
├── 📁 waitqr/                    # Handles QR scan callbacks and process triggers
│   ├── callback.php
│   └── intiate.php

```
```
├── 📁 CPEE Files/               # CPEE workflow definitions
│   ├── mensa.xml                # Main/parent workflow
│   └── 📁 subprocesses/
│       ├── map.xml              # Subprocess: open map view
│       ├── mensagarching.xml    # Subprocess: Garching flow
│       ├── mensabolzmann.xml    # Subprocess: Boltzmann flow
│       └── maschinenbau.xml     # Subprocess: Maschinenbau flow
```
## CPEE process

![img.png](img.png)

## Features

### 1. Interactive Map
- **File**: `display/map.html`
- Interactive map showing TUM cafeteria locations
- Uses MapTiler SDK for mapping functionality
- Responsive design with TUM branding
- **MapTiler API Configuration**:
  - API Key location: Line 122 in `display/map.html`
  - Map style: `streets` with terrain and sky layers
  - Center coordinates: `[11.668879,48.265727]` (TUM Garching)
  - Zoom level: 15.8, Pitch: 58.8°, Bearing: -61°
  - Popup anchors configured for QR code displays

### 2. Menu Display Pages
- **Files**: `display/mensa*.html`
- Individual pages for each cafeteria:
    - Mensa Garching (`mensagarching.html`)
    - Mensa Boltzmann (`mensabolzmann.html`)
    - Mensa Maschinenbau (`mensamaschinenbau.html`)
- Real-time menu data from TUM Eat API
- QR code navigation between locations

### 3. Voting System
- **Directory**: `votes/`
- **Main File**: `vote.php`
- Vote counting for three cafeterias
- JSON-based data persistence
- Vote reset functionality
  - With operation for resetting on a daily basis

### 4. Menu Data Management
- **Directory**: `jsonpasser/`
- Fetches current week's menu data from TUM Eat API
- Filters menus to show only current day
- Separate endpoints for each cafeteria

### 5. QR Code Navigation
- **Directory**: `qrcodes/`
- Pre-generated QR codes for each location
- Enables easy navigation between different views
- Integration with CPEE workflow system

## Setup Instructions


### Installation

1. **Clone or download the project**:
   ```bash
   git clone <repository-url>
   cd mensavoter
   ```

2. **Web Server Setup**:
    - Place the project directory in your web server's document root
    - Ensure PHP is enabled and configured
    - Make sure the `votes/data/` and `jsonpasser/data/` directories are writable

3. **MapTiler Configuration** (Required for Map Functionality):
    - Get a free API key from [MapTiler](https://maptiler.com)
    - Replace `KEY` in `display/map.html` line 122 with your API key:
      ```javascript
      const KEY='your-maptiler-api-key-here';  // Line 122
      ```
    - Current demo key: `8na9eBlqJgIviIZxv3kB` (may have usage limits)
    - MapTiler SDK version: 3.0.1 (loaded via CDN)

4. **Directory Permissions**:
   ```bash
   chmod 755 votes/data jsonpasser/data
   chmod 644 votes/data/* jsonpasser/data/*
   ```

### Quick Start

1. **Access the main map page**:
   ```
   http://your-domain/mensavoter/display/map.html
   ```

2. **View individual cafeteria menus**:
   ```
   https://lehre.bpm.in.tum.de/~ge49fag/mensa/display/map.html
   https://lehre.bpm.in.tum.de/~ge49fag/mensa/display/mensagarching.html
   https://lehre.bpm.in.tum.de/~ge49fag/mensa/display/mensabolzmann.html
   https://lehre.bpm.in.tum.de/~ge49fag/mensa/display/mensamaschinenbau.html
   ```


## API Endpoints

### Voting API
- **Vote**: `GET /votes/vote.php?mensa={mensa_name}`
    - Parameters: `mensa` (mensa_garching, mensa_bolzmann, mensa_maschinenbau)
    - Response: JSON with success status and current vote counts

- **Reset Votes**: `GET /votes/reset_votes.php`
    - Resets all vote counts to zero

### Menu Data API
- **Fetch Menu Data**: `GET|POST /jsonpasser/{mensa_file}.php`
    - Available endpoints:
        - `/jsonpasser/index.php` - Generic menu fetcher
        - `/jsonpasser/mensa_garching.php` - Garching-specific
        - `/jsonpasser/mensa_bolzmann.php` - Boltzmann-specific
        - `/jsonpasser/mensa_maschinenbau.py` - Maschinenbau-specific
    - Parameters:
        - `year` (optional): ISO year
        - `week` (optional): ISO week number
    - Response: JSON menu data for the specified week

### CPEE Integration
- **QR Callback**: `/waitqr/intiate.php`
    - Handles CPEE workflow callbacks
    - Stores callback URLs for process continuation
    
- **Navigation Callback**: `/waitqr/callback.php`
    - Processes navigation parameters from QR codes
    - URL structure: `callback.php?navigate={target}`
    - Supported targets: `map`, `garching`, `bolzmann`, `maschinenbau`
    - Integrates with CPEE workflow state management

## QR Code System

The application uses QR codes for seamless navigation between different views and CPEE workflow integration:

### QR Navigation Structure
Each QR code is designed for workflow-driven interaction and includes dynamic navigation:

**Navigation QR Codes with Parameters:**
- Main map navigation:
```
    https://lehre.bpm.in.tum.de/~ge49fag/mensa/waitqr/callback.php?navigate=map`
```
- Mensa-specific navigation:
```
    `https://lehre.bpm.in.tum.de/~ge49fag/mensa/waitqr/callback.php?navigate=garching`
    `https://lehre.bpm.in.tum.de/~ge49fag/mensa/waitqr/callback.php?navigate=bolzmann`
    `https://lehre.bpm.in.tum.de/~ge49fag/mensa/waitqr/callback.php?navigate=maschinenbau`
```

**Voting QR Codes with Mensa Parameter:**
- Direct voting links dynamically generated via api.qrserver.com API:
  - Vote Garching: `https://lehre.bpm.in.tum.de/~ge49fag/mensa/votes/vote.php?mensa=mensa_garching`
  - Vote Boltzmann: `https://lehre.bpm.in.tum.de/~ge49fag/mensa/votes/vote.php?mensa=mensa_bolzmann`
  - Vote Maschinenbau: `https://lehre.bpm.in.tum.de/~ge49fag/mensa/votes/vote.php?mensa=mensa_maschinenbau`

### Static QR Code Files
- **Map QR Code** (`qrcodes/map.png`): Links to main map page
- **Cafeteria QR Codes**: Individual codes for each cafeteria
    - `qrcodes/mensagarching.png`
    - `qrcodes/mensabolzmann.png`
    - `qrcodes/maschinenbau.png`
- **Voting QR Codes**: Enable direct voting functionality
    - `qrcodes/mensagarchingvote.png`
    - `qrcodes/mensabolzmannvote.png`
    - `qrcodes/mensamaschinenbauvote.png`

### Navigation Mapping
**QR Parameter → Target Page:**
- `navigate=map` → `display/map.html`
- `navigate=garching` → `display/mensagarching.html`
- `navigate=bolzmann` → `display/mensabolzmann.html`
- `navigate=maschinenbau` → `display/mensamaschinenbau.html`

### QR Code Generation
- **Static QR Codes**: Pre-generated images stored in `qrcodes/` directory

### Footer Navigation QR Codes
Each page includes footer QR codes for cross-navigation:
- **From any mensa page**: Access other mensas and map
- **From map page**: Direct access to individual mensa pages
- **Workflow integration**: All QR codes can trigger CPEE subprocess transitions

## Styling and Design

- **CSS Framework**: Custom CSS with TUM branding
- **Color Scheme**: TUM Blue (#3070b3) primary colors
- **Responsive Design**: Mobile-friendly layouts
- **Background Images**: TUM campus imagery
- **Typography**: System fonts with professional styling

## Data Sources

### TUM Eat API
- **Base URL**: `https://tum-dev.github.io/eat-api/`
- **Format**: Weekly JSON files with menu data
- **Structure**: Organized by cafeteria, year, and week
- **Update Frequency**: Weekly

### Local Data Storage
- **Vote Data**: `votes/data/votes.json`
- **Menu Cache**: `jsonpasser/data/*.json`
- **Format**: JSON with UTF-8 encoding

## Supported Cafeterias

1. **Mensa Garching**
    - Location: Boltzmannstr. 19, 85748 Garching
    - API Endpoint: `mensa-garching`

2. **Mensa Boltzmann**
    - Location: TUM Campus
    - API Endpoint: `mensa-boltzmann`

3. **Mensa Maschinenbau**
    - Location: TUM Campus
    - API Endpoint: `mensa-maschinenbau`

## Extendability

To add another cafeteria:
1.	Create a new HTML page in display/ (e.g., mensacampus.html).
2.	Add a new data script in jsonpasser/ (e.g., mensa_campus.php).
3.	Extend vote.php and add a new QR code in qrcodes/.
4.	Create a new subprocess in CPEE Files/subprocesses/ (e.g., campus.xml).

## Development Notes

### File Structure Details
- **HTML Pages**: Use semantic markup with accessibility considerations
- **PHP Scripts**: PHP 7.0+ compatible, error handling included
- **JSON Data**: Pretty-printed with Unicode support
- **CSS**: Modular structure with CSS custom properties

## MapTiler API Integration

### Configuration Details
- **API Key Location**: `display/map.html` line 122
- **Current Key**: `8na9eBlqJgIviIZxv3kB` (demo key with potential rate limits)
- **SDK Version**: 3.0.1 (CDN: `https://cdn.maptiler.com/maptiler-sdk-js/v3.0.1/`)

### Map Configuration
```javascript
const KEY='your-api-key-here';  // Replace this line
const STYLE_URL = `https://api.maptiler.com/maps/streets/style.json?key=${KEY}`;
const CENTER=[11.668879,48.265727]; // TUM Garching coordinates
const ZOOM=15.8, PITCH=58.8, BEARING=-61; // 3D viewing angle
```

### Popup Coordinates
- **StuCafé Maschinenbau**: `[11.667724,48.265774]`
- **Current Location**: `[11.6670,48.2627]`
- **StuCafé Garching**: `[11.67174,48.2681]`
- **Mensa Garching**: `[11.67232,48.2684]`

### Features Used
- Terrain elevation with `terrain-rgb` tiles
- Sky atmosphere rendering
- Symbol layer visibility disabled for cleaner look
- Custom popup anchoring (bottom, left, right)




