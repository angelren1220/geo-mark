# GeoMark

GeoMark is a web application designed to help users effortlessly locate geographical coordinates, search for specific locales, view them on a map, and manage a list of explored places.

!["Full-window size app"](https://github.com/angelren1220/geo-mark/blob/main/docs/full-window-app.png?raw=true)

## Features

- **Acquire Current Location**: Instantly fetch and showcase your current geographical position on the map.
- **Location Search**: Key in the name of any place and watch it materialize on the map.
- **Location Management**: With our table view, keep track of all the places you've ventured. Employ the checkboxes to select and erase multiple entries.
- **Timezone & Local Time**: Stay informed with the timezone and local time of your most recent search.

## Prerequisites

- **Node.js and npm**: Ensure Node.js and npm are in your artillery. If they're missing, enlist them [here](https://nodejs.org/).

- **Vue.js**: Ascertain that Vue.js is on your toolkit. If it's absent, recruit it [here](https://vuejs.org/guide/quick-start.html).

- **Google Maps API Key**: To unfold the maps and geocoding marvels, retrieve your key from the [Google Cloud Console](https://console.cloud.google.com/).

## Getting Started with GeoMark

1. **Summon the Repository**:
   ```bash
   git clone https://github.com/angelren1220/geo-mark.git
   cd geo-mark
   ```

2. **Call in the Troops (Dependencies)**:
   ```bash
   npm install
   ```

3. **Plant Your Google Maps API Key**: Make a .env from .env.example, then swap `YOUR_GOOGLE_MAPS_API_KEY` with your genuine key.

4. **Power Up the Development Server**:
   ```bash
   npm run serve
   ```

Your GeoMark should now be gallivanting on `http://localhost:8080/`.

## Dependencies
- Vue.js (^3.2.13): The beating heart of our application. Vue is a progressive JavaScript framework used to develop interactive web interfaces. More about Vue.js

- Vue Router (^4.2.4): Our navigator across the vast seas of the application's components. It's the official router for Vue.js. More about Vue Router

- Axios (^1.5.0): Our trusted messenger that fetches data from distant lands (APIs). Axios is a promise-based HTTP client for the browser and Node.js. More about Axios

- Core-js (^3.8.3): Ensures that GeoMark's charm works across all lands (browsers). It's a modular standard library for JavaScript, which includes polyfills for ECMAScript up to 2019.

## Styling
- Semantic UI (2.5.0): A development framework that empowers our application with user-friendly high-quality HTML, CSS, and JS components.

## Guided Tour

1. **Spot Current Location**: A single tap on the locate button will beacon your coordinates.
2. **Scout for a Location**: Punch in a name or address, and it will be auto-completed. Select the desired location and tap "Search".
3. **Organize Your Discoveries**: The table is your treasure map. Use the checkboxes wisely to select multiple entries, and swipe away unwanted locations with the "Delete" button.
4. **Time Travels**: Just above the map, the local time and timezone details of your recent discovery await.