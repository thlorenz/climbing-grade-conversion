# 🧗 Climbing Grade Conversion Chart

An interactive webapp for converting climbing grades across different international grading systems. Perfect for climbers looking to understand how their local grades compare globally.

## What is it?

This is a lightweight, interactive climbing grade conversion tool that translates between climbing grades from six different countries/systems:

- **Sport Grade** (France) - The most widely used international sport climbing grade
- **British Trad** (UK) - Traditional climbing grades used in British climbing
- **UIAA** (Germany/Alpine) - Used throughout Alpine regions
- **YDS** (USA) - Yosemite Decimal System
- **Norway** - Norwegian climbing grade system
- **Australia** - Australian climbing grade system

## Features

✨ **Interactive Toggle System** - Show/hide any grading system with checkboxes at the bottom. Only see the grades you care about.

📱 **Responsive Design** - Works seamlessly on desktop, tablet, and mobile devices with optimized layouts for each screen size.

📲 **Progressive Web App (PWA)** - Install directly on your phone or desktop. Works completely offline thanks to service worker caching.

💾 **Persistent Preferences** - Your column preferences are automatically saved to browser localStorage, so your setup stays the same next time you visit.

🚀 **Zero Dependencies** - Pure vanilla HTML, CSS, and JavaScript. No frameworks, no build tools, no bloat.

## How to Use

1. **Open the website** - Visit the app in your web browser
2. **View grades** - The Sport Grade and YDS columns are shown by default
3. **Customize columns** - Use the checkboxes at the bottom to show/hide other grading systems
4. **Your preferences save automatically** - Come back anytime and your layout will be the same
5. **Install on your phone** - Most browsers allow you to "Add to Home Screen" to install this as an app

## Offline Usage

This app works **completely offline** thanks to the integrated service worker. Once you've visited the app once, it caches all resources and will work without an internet connection. Perfect for accessing grades while at the crag.

## Local Development

To run this locally:

```bash
cd climbing-table
python3 -m http.server 8000
```

Then open `http://localhost:8000` in your browser.

## Data Source

Climbing grade conversions based on **Rockfax Single Grade Conversion 2021** - a comprehensive climbing grade reference used by climbers worldwide.

## Tech Stack

- **HTML** - Semantic markup and table structure
- **CSS** - Modern responsive design with gradients and hover effects
- **JavaScript (Vanilla)** - No frameworks, pure DOM manipulation
- **Service Worker** - For offline support and PWA functionality

## Implementation Note

⚡ **Entirely vibe coded** - No AI assistance, pure instinct and vibes
