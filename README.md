# ✈️ Flight Tracker Globe

A real-time flight tracking visualization tool that displays live aircraft positions on an interactive 3D globe. Search for any active flight by code or track celebrity private jets with detailed telemetry data.

**Live Demo:** [https://flight-tracker-liard.vercel.app/](https://flight-tracker-liard.vercel.app/)

![Flight Tracker Globe](https://img.shields.io/badge/status-live-success)
![License](https://img.shields.io/badge/license-MIT-blue)

---

## 📋 Table of Contents

- [Overview](#overview)
- [Features](#features)
- [Demo & Usage](#demo--usage)
- [Technical Architecture](#technical-architecture)
- [API Integration](#api-integration)
- [Installation & Deployment](#installation--deployment)
- [Project Structure](#project-structure)
- [Configuration](#configuration)
- [Development](#development)
- [Limitations & Known Issues](#limitations--known-issues)
- [Future Enhancements](#future-enhancements)
- [Contributing](#contributing)
- [License](#license)

---

## 🌍 Overview

Flight Tracker Globe is a single-page web application that provides real-time aircraft tracking visualization using public flight data APIs. The application renders a 3D interactive globe using WebGL technology, allowing users to search for specific flights and view their current position, trajectory, speed, altitude, and other telemetry data.

### Key Capabilities

- **Real-time Flight Tracking**: Live position updates from OpenSky Network's public API
- **3D Globe Visualization**: Interactive WebGL-powered globe with smooth animations
- **Detailed Flight Information**: Comprehensive telemetry including altitude, speed, position, and status
- **Celebrity Jet Tracking**: Pre-configured mappings for tracking private jets
- **Dark Mode Support**: Automatic theme switching based on system preferences
- **Responsive Design**: Works seamlessly across desktop, tablet, and mobile devices
- **Zero Configuration**: No API keys required for basic functionality

---

## ✨ Features

### Real-Time Flight Data

- **Live Position Tracking**: Updates aircraft coordinates in real-time
- **Flight Status**: In-flight vs. on-ground detection
- **Altitude Display**: Shown in both feet and meters
- **Speed Metrics**: Displays velocity in km/h and knots
- **Vertical Rate**: Shows climbing/descending rate
- **Last Update Timestamp**: Time since last data refresh

### Interactive Visualization

- **3D Globe Rendering**: High-quality Earth texture with graticules
- **Flight Path Display**: Animated trajectory lines showing aircraft course
- **Smooth Camera Transitions**: Auto-focus on searched flights
- **Hover Tooltips**: Interactive labels on aircraft markers
- **Responsive Controls**: Rotate, zoom, and pan the globe

### Search Functionality

- **Flight Code Search**: Enter any ICAO callsign (e.g., `AAL100`, `BA58`)
- **Celebrity Tracking**: Search by name (e.g., "Taylor Swift", "Elon Musk")
- **Fuzzy Matching**: Handles spaces and case variations
- **Auto-retry Logic**: Attempts multiple API calls for better results

### User Experience

- **Dark Mode**: Automatic light/dark theme based on system preferences
- **Status Messages**: Clear feedback for loading states and errors
- **Detailed Info Panel**: Expandable section with comprehensive flight data
- **Clean UI**: Minimalist design with focus on visualization
- **Mobile Optimized**: Touch-friendly controls and responsive layout

---

## 🎮 Demo & Usage

### Live Application

Visit the deployed app: **[https://flight-tracker-liard.vercel.app/](https://flight-tracker-liard.vercel.app/)**

### How to Use

1. **Search for a Flight**
   - Enter a flight code in the search bar (e.g., `UAL2142`, `DLH456`)
   - Or try a celebrity name (e.g., "Taylor Swift")
   - Press Enter or click Search

2. **View Flight Information**
   - The globe automatically centers on the flight location
   - A sphere marker appears at the aircraft's position
   - A trajectory line shows the approximate flight path
   - Detailed telemetry appears in the info panel below

3. **Interact with the Globe**
   - **Click and drag** to rotate the globe
   - **Scroll** to zoom in/out
   - **Hover** over the flight marker for quick info

### Example Flight Codes to Try

- `AAL100` - American Airlines flight
- `BA58` - British Airways flight
- `UAL` - United Airlines flights (any flight number)
- `DLH` - Lufthansa flights
- `AFR` - Air France flights

**Note**: Flight must be currently airborne and tracked by OpenSky Network to appear in search results.

---

## 🏗️ Technical Architecture

### Technology Stack

| Component | Technology | Purpose |
|-----------|-----------|---------|
| **Frontend** | HTML5, CSS3, Vanilla JavaScript | Core application logic and UI |
| **3D Rendering** | Globe.gl (Three.js wrapper) | WebGL-based 3D globe visualization |
| **3D Engine** | Three.js | Low-level 3D graphics rendering |
| **API** | OpenSky Network REST API | Real-time flight telemetry data |
| **Deployment** | Vercel | Edge network hosting and CDN |

### Architecture Overview

┌─────────────────────────────────────────┐
│           User Interface                │
│  (HTML + CSS + JavaScript)              │
└──────────────┬──────────────────────────┘
               │
               ▼
┌─────────────────────────────────────────┐
│       Globe.gl Visualization            │
│  (3D rendering + interaction)           │
└──────────────┬──────────────────────────┘
               │
               ▼
┌─────────────────────────────────────────┐
│          Three.js Engine                │
│  (WebGL graphics processing)            │
└─────────────────────────────────────────┘
               │
               ▼
┌─────────────────────────────────────────┐
│       OpenSky Network API               │
│  (Flight data source)                   │
└─────────────────────────────────────────┘

### Data Flow

1. **User Input**: User enters flight code or celebrity name
2. **Query Processing**: Input is validated and normalized
3. **API Request**: Fetch all active flights from OpenSky Network
4. **Data Filtering**: Match user query against flight callsigns
5. **Visualization**: Render flight position and trajectory on globe
6. **Info Display**: Show detailed telemetry in info panel
7. **Updates**: Repeat fetch on demand (manual refresh via new search)

---

## 🔌 API Integration

### OpenSky Network API

**Base Endpoint**: `https://opensky-network.org/api/states/all`

**Authentication**: None required (public endpoint)

**Rate Limits**: 
- Anonymous: ~10 requests per second
- With account: Higher limits available

**Response Format**: JSON

#### Sample Response Structure

{
  "time": 1700000000,
  "states": [
    [
      "abc123",        // ICAO24 transponder address
      "AAL100  ",      // Callsign
      "United States", // Origin country
      1700000000,      // Time position
      1700000000,      // Last contact
      -118.3456,       // Longitude
      34.0522,         // Latitude
      10668,           // Barometric altitude (meters)
      false,           // On ground
      250.5,           // Velocity (m/s)
      180.0,           // True track (degrees)
      5.2              // Vertical rate (m/s)
    ]
  ]
}

#### Data Fields Used

| Index | Field | Description | Unit |
|-------|-------|-------------|------|
| 0 | ICAO24 | Aircraft transponder code | String |
| 1 | Callsign | Flight identifier | String |
| 2 | Origin Country | Country of registration | String |
| 4 | Last Contact | Unix timestamp | Seconds |
| 5 | Longitude | East-west position | Degrees |
| 6 | Latitude | North-south position | Degrees |
| 7 | Barometric Altitude | Height above sea level | Meters |
| 8 | On Ground | Flight status | Boolean |
| 9 | Velocity | Ground speed | m/s |
| 11 | Vertical Rate | Climb/descent rate | m/s |

### Future API Integrations

The application is designed to support additional APIs for enhanced data:

#### AviationStack (Optional)

- **Purpose**: Departure/arrival times, airport codes, gates
- **Requires**: API key (free tier: 100 requests/month)
- **Endpoint**: `https://api.aviationstack.com/v1/flights`

#### Aviation Edge (Optional)

- **Purpose**: Flight schedules, airline data, routes
- **Requires**: API key
- **Endpoint**: `https://aviation-edge.com/v2/public/flights`

---

## 🚀 Installation & Deployment

### Prerequisites

- Modern web browser with WebGL support
- Internet connection (for CDN resources)
- (Optional) Vercel CLI for deployment

### Local Development

1. **Clone or Download**
   # Download the HTML file
   curl -O https://flight-tracker-liard.vercel.app/index.html

2. **Run Locally**
   # Simple HTTP server (Python 3)
   python -m http.server 8000
   
   # Or use Node.js
   npx http-server -p 8000

3. **Access Application**
   http://localhost:8000

### Deploy to Vercel

#### Method 1: Vercel CLI

# Install Vercel CLI
npm install -g vercel

# Deploy
cd your-project-folder
vercel

# Follow prompts to login and configure

#### Method 2: Vercel Dashboard

1. Create project folder with `index.html`
2. Go to [vercel.com](https://vercel.com)
3. Click "Add New Project"
4. Drag and drop your folder
5. Click "Deploy"

#### Method 3: GitHub Integration

1. Push code to GitHub repository
2. Connect repository to Vercel
3. Auto-deploy on every push

---

## 📁 Project Structure

flight-tracker/
│
├── index.html              # Main application file (single-page app)
├── README.md              # This documentation
├── vercel.json            # (Optional) Vercel configuration
└── .gitignore             # (Optional) Git ignore rules

### Single File Architecture

The application uses a single HTML file containing:

- **HTML Structure**: Semantic markup for layout
- **CSS Styles**: Embedded `<style>` tag with complete styling
- **JavaScript Logic**: Embedded `<script>` tag with all application code

**Why Single File?**
- Simplifies deployment (one file to upload)
- No build process required
- Easy to share and distribute
- Perfect for Vercel/Netlify static hosting

---

## ⚙️ Configuration

### Celebrity Flight Mappings

Edit the `celebrityToFlights` object in the JavaScript section:

const celebrityToFlights = {
  "taylor swift": "TAYLOR1",  // Replace with actual callsign
  "elon musk": "EJM",
  "kylie jenner": "KYLIE1",
  // Add more celebrity mappings here
};

**Finding Real Celebrity Jet Codes:**
1. Visit celebrity jet tracking sites (e.g., CelebJets, JetSpy)
2. Look up aircraft registration (N-number)
3. Convert to ICAO callsign
4. Add to mapping object

### API Configuration

To add Aviation Edge or AviationStack:

// Replace YOUR_API_KEY with actual key
const AVIATION_EDGE_KEY = "YOUR_API_KEY";
const AVIATION_EDGE_ENDPOINT = `https://aviation-edge.com/v2/public/flights?key=${AVIATION_EDGE_KEY}&flight_iata=`;

### Visual Customization

#### Colors

Edit CSS variables in `:root`:

:root {
  --color-primary: #21808d;     /* Primary accent color */
  --color-background: #fcfcf9;  /* Page background */
  --color-surface: #fff;        /* Card backgrounds */
  --color-text: #13343b;        /* Text color */
}

#### Globe Appearance

Modify Globe.gl configuration:

let GlobeInstance = Globe()(globeEl)
  .globeImageUrl('//path/to/your/earth-texture.jpg')
  .backgroundColor('#fcfcf9')
  .showGraticules(true)          // Show/hide latitude/longitude lines
  .showAtmosphere(true);         // Show/hide atmosphere glow

---

## 🛠️ Development

### Code Structure

#### Main Components

1. **Search Handler** (`form.addEventListener`)
   - Input validation
   - Celebrity name mapping
   - API request coordination

2. **Flight Data Fetcher** (`fetchFlightsData`)
   - OpenSky API calls
   - Error handling
   - Data parsing

3. **Flight Matcher** (`findFlightByCode`)
   - Callsign comparison
   - Fuzzy matching logic

4. **Visualization Engine** (`visualizeFlight`)
   - Globe marker rendering
   - Trajectory arc generation
   - Camera positioning

5. **Info Display** (`displayFlightInfo`)
   - Data conversion (m/s to km/h, meters to feet)
   - Status determination
   - UI population

### Adding New Features

#### Example: Add Airport Markers

// After visualizeFlight function
function addAirports() {
  const airports = [
    { lat: 40.6413, lng: -73.7781, name: "JFK" },
    { lat: 51.4700, lng: -0.4543, name: "LHR" },
  ];
  
  GlobeInstance.labelsData(airports)
    .labelLat('lat')
    .labelLng('lng')
    .labelText('name')
    .labelSize(1.5)
    .labelColor(() => 'rgba(255, 165, 0, 0.75)');
}

### Debugging

Enable console logging:

// Add at top of script
const DEBUG = true;

// Use throughout code
if (DEBUG) console.log('Flight data:', flightState);

---

## ⚠️ Limitations & Known Issues

### Current Limitations

1. **No Historical Data**
   - Only shows currently airborne flights
   - Cannot track flights that have landed
   - No historical trajectory replay

2. **Simulated Schedule Data**
   - Departure/arrival times are estimated
   - Requires paid API for real schedule data
   - No gate or terminal information

3. **Limited Celebrity Data**
   - Celebrity mappings are hardcoded examples
   - Requires manual updates for real jet codes
   - No automatic celebrity flight database

4. **API Rate Limits**
   - OpenSky limits anonymous requests
   - May fail during high traffic periods
   - No request caching implemented

5. **Search Limitations**
   - Must know exact flight code or celebrity name
   - No fuzzy search for partial matches
   - No flight number search (only callsigns)

### Known Issues

- **Globe Performance**: May lag on older mobile devices
- **CORS Issues**: Some APIs may require server-side proxy
- **Timezone Display**: Times shown in local browser timezone
- **Trajectory Simulation**: Flight path is approximated, not actual route

---

## 🔮 Future Enhancements

### Planned Features

- [ ] **Real-time Auto-refresh**: Update positions every 10 seconds
- [ ] **Flight Search Autocomplete**: Suggest flights as you type
- [ ] **Multiple Flight Tracking**: Track several flights simultaneously
- [ ] **Historical Playback**: Replay past flight trajectories
- [ ] **Airport Database**: Show major airports on globe
- [ ] **Weather Overlay**: Display real-time weather patterns
- [ ] **Flight Alerts**: Notifications for specific flights
- [ ] **Route Planning**: Calculate optimal flight paths
- [ ] **Share Feature**: Generate shareable links to specific flights
- [ ] **Mobile App**: Native iOS/Android versions

### Technical Improvements

- [ ] Add service worker for offline support
- [ ] Implement request caching to reduce API calls
- [ ] Add WebSocket support for live data streaming
- [ ] Optimize 3D rendering for better performance
- [ ] Add unit tests for core functions
- [ ] Implement CI/CD pipeline
- [ ] Add analytics tracking
- [ ] Create API proxy server for better rate limit handling

---

## 🤝 Contributing

Contributions are welcome! Here's how you can help:

### Reporting Issues

1. Check existing issues first
2. Provide detailed description
3. Include browser/OS information
4. Add screenshots if applicable
---

## 📄 License

This project is licensed under the MIT License.

MIT License

Copyright (c) 2025 Flight Tracker Globe

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.

---

##  Acknowledgments

- **OpenSky Network**: For providing free flight tracking data
- **Globe.gl**: For the excellent 3D globe visualization library
- **Three.js**: For WebGL rendering capabilities
- **Vercel**: For free hosting and deployment

---

##  Support

For questions, issues, or suggestions:

- **Live Demo**: [https://flight-tracker-liard.vercel.app/](https://flight-tracker-liard.vercel.app/)
- **Issues**: Open an issue on GitHub
- **Email**: Contact through GitHub profile

---

**Made with ✈️ by developers who love aviation**

*Last Updated: November 25, 2025*
