# Gnome Weather — Design Document
*Generated: February 19, 2026*

---

## 1. App Concept

**Gnome Weather** is a charming iOS weather app where a garden gnome character reacts to real-time weather conditions. Instead of sterile icons and data tables, users see a living scene: their gnome is out in his garden, responding to whatever the weather is doing right now.

**Tagline:** *"Your garden gnome always knows."*

**Target Audience:**
- Cottagecore / cozy aesthetic fans (18–35)
- Casual weather checkers who want something delightful instead of utilitarian
- Gift-givers (it's the kind of app you screenshot and send to your mom)

**Unique Value Prop:**
Every major weather app looks the same. Gnome Weather wins on personality. The gnome *is* the UX — users form an attachment to him, check the app more often, share screenshots. It's a weather app that feels alive.

---

## 2. Feature List

### MVP (v1.0)
- Current weather with animated gnome scene
- Temperature (°F / °C toggle)
- Weather condition label (Sunny, Rainy, etc.)
- High/low for the day
- Location detection (CoreLocation)
- 5-day forecast strip (bottom of screen)
- Light/dark mode support

### V2
- Hourly forecast view
- Weather alerts (gnome waves a warning flag)
- Gnome name customization
- Widget support (home screen gnome!)
- Multiple locations
- Feels-like temperature, humidity, UV index

### Future
- Animated weather transitions
- Seasonal gnome outfits (holiday hats, etc.)
- Apple Watch complication
- Gnome "mood" quotes based on weather

---

## 3. Architecture

### Tech Stack
- **SwiftUI** — full SwiftUI, no UIKit
- **WeatherKit** — Apple's native weather API (free with Apple Developer account, best iOS integration)
- **CoreLocation** — for user location
- **Lottie** (optional) — for gnome animations, or SwiftUI animations

### Key Views
```
ContentView
├── GnomeSceneView       // Main hero — the gnome + weather background
├── CurrentWeatherView   // Temp, condition, high/low
├── ForecastStripView    // 5-day horizontal scroll
├── SettingsView         // Units, location, about
└── LoadingView          // Gnome with question mark while fetching
```

### Data Models
```swift
struct WeatherData {
    let condition: WeatherCondition
    let temperature: Double
    let feelsLike: Double
    let high: Double
    let low: Double
    let humidity: Int
    let forecast: [DayForecast]
}

enum WeatherCondition {
    case sunny, cloudy, partlyCloudy, rainy, stormy, snowy, windy, foggy
}

struct DayForecast {
    let day: String
    let condition: WeatherCondition
    let high: Double
    let low: Double
}
```

### State Management
- `@StateObject WeatherViewModel` — fetches and holds WeatherKit data
- `@Published condition: WeatherCondition` — drives gnome scene updates

---

## 4. Weather API

**Recommendation: WeatherKit (Apple)**

| Option | Cost | Pros | Cons |
|--------|------|------|------|
| WeatherKit | Free (Apple Dev account) | Native iOS, no key management, very accurate | Requires paid Apple Dev account |
| OpenWeatherMap | Free tier (1000 calls/day) | Easy REST API, generous free tier | Extra network layer, key management |
| Open-Meteo | Completely free | No API key, open source | Less brand recognition |

**Decision: WeatherKit for production, Open-Meteo for development/testing.**

WeatherKit integrates directly into SwiftUI, handles privacy natively, and is the most accurate for US locations. For early builds before App Store, use Open-Meteo (no account needed).

---

## 5. Visual Design

### Color Palette

| Condition | Sky Color | Accent | Mood |
|-----------|-----------|--------|------|
| Sunny | `#87CEEB` → `#FFF4B8` | `#FFD700` gold | Warm, cheerful |
| Partly Cloudy | `#B0C4DE` → `#E8E8E8` | `#FFFFFF` | Neutral, soft |
| Cloudy | `#708090` → `#C0C0C0` | `#9B9B9B` | Muted, grey |
| Rainy | `#4A6FA5` → `#708090` | `#87CEEB` | Cool, blue |
| Stormy | `#2C3E50` → `#4A4A4A` | `#FFD700` lightning | Dark, dramatic |
| Snowy | `#E8F4FD` → `#FFFFFF` | `#AED6F1` | Clean, white |
| Windy | `#87CEEB` → `#B8D4E8` | `#96C0CE` | Breezy, light |
| Foggy | `#C8D6DF` → `#E8E8E8` | `#A9A9A9` | Hazy, soft |

### Typography
- **Primary Font:** `Georgia` or custom serif — cozy, storybook feel
- **Temperature Display:** Large, bold, slightly rounded
- **Labels:** Clean sans-serif (`SF Pro Rounded`) for readability

### Design Language
- Rounded corners everywhere
- Soft drop shadows
- Illustrated/painterly style (not flat icons)
- Slight texture on backgrounds (paper or linen feel)

---

## 6. Gnome Characters — Weather States

Each state shows the gnome in his garden. The gnome is classic ceramic-style: red pointed hat, white beard, rosy cheeks, blue or green tunic.

### ☀️ Sunny
Gnome stands tall, smiling broadly, one hand shading his eyes as he looks up at the sun. Small flowers blooming around his feet. Maybe a tiny watering can nearby.

### ⛅ Partly Cloudy
Gnome is gardening — on one knee tending to a flower patch. Relaxed expression, looking content. Light and shadow playing across the scene.

### ☁️ Cloudy
Gnome sitting on a mushroom, arms crossed, looking up at the grey sky with mild displeasure. A cup of tea beside him.

### 🌧️ Rainy
Gnome in a tiny yellow raincoat and matching rain hat. Holding a miniature umbrella. Standing in a puddle, looking cheerful despite the rain. Rain drops pattering around him.

### ⛈️ Stormy
Gnome has retreated to his little cottage doorway — half inside, half out. Peeking around the door frame, eyes wide. Lightning visible in the background. Wind bending the trees.

### ❄️ Snowy
Gnome bundled up in a scarf and mittens. Building a tiny snowman in the garden. Rosy cheeks extra red. Snowflakes on his hat. Pure joy.

### 💨 Windy
Gnome holding onto his red hat with both hands, leaning into the wind. Scarf and beard blown sideways. Leaves and petals flying past. Determined expression.

### 🌫️ Foggy
Gnome barely visible through thick mist. Holding a lantern. Looking around cautiously. Other garden elements fading into the fog behind him.

---

## 7. Screen Flows

### Main Screen (default)
```
┌─────────────────────────────┐
│  📍 New York, NY            │
│                             │
│   [GNOME SCENE — full art]  │
│                             │
│        72°                  │
│      Sunny                  │
│    H:78° L:65°              │
│                             │
│  ▸ Mon Tue Wed Thu Fri      │
│    ☀️  ⛅  🌧️  ❄️  ☀️      │
│    78° 70° 65° 52° 68°      │
└─────────────────────────────┘
```

### Forecast Detail (tap a day)
Full-screen view with gnome scene for that day's condition, hourly breakdown, and extended stats.

### Settings
- Temperature unit (°F / °C)
- Location (auto or manual search)
- Gnome name (rename your gnome!)
- About / credits

---

## 8. File Structure

```
GnomeWeather/
├── GnomeWeatherApp.swift
├── Models/
│   ├── WeatherData.swift
│   ├── WeatherCondition.swift
│   └── DayForecast.swift
├── ViewModels/
│   └── WeatherViewModel.swift
├── Views/
│   ├── ContentView.swift
│   ├── GnomeSceneView.swift
│   ├── CurrentWeatherView.swift
│   ├── ForecastStripView.swift
│   ├── ForecastDetailView.swift
│   ├── SettingsView.swift
│   └── LoadingView.swift
├── Services/
│   ├── WeatherService.swift        // WeatherKit wrapper
│   └── LocationService.swift      // CoreLocation wrapper
├── Assets.xcassets/
│   ├── GnomeAssets/               // Gnome illustrations per condition
│   ├── Backgrounds/               // Sky/scene backgrounds
│   └── AppIcon.appiconset/
└── Preview Content/
    └── PreviewAssets.xcassets/
```

---

## 9. Next Steps

1. **Set up Xcode project** — SwiftUI, WeatherKit capability enabled
2. **Build WeatherService** — wrap WeatherKit, map conditions to `WeatherCondition` enum
3. **Build ContentView skeleton** — layout without gnome art yet (use placeholder)
4. **Commission or generate gnome art** — 8 illustrations (one per condition)
5. **Wire up GnomeSceneView** — swap illustration based on condition
6. **Polish + animations** — SwiftUI transitions between weather states
7. **TestFlight beta** — get it on device

---

*Ready to build. Start with the WeatherService + ContentView skeleton.*
