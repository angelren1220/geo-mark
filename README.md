# GeoMark

GeoMark is a web application designed to help users effortlessly locate geographical coordinates, search for specific locales, view them on a map, and manage a list of explored places.

## Features

- **Acquire Current Location**: Instantly fetch and showcase your current geographical position on the map.
- **Location Search**: Key in the name of any place and watch it materialize on the map.
- **Location Management**: With our table view, keep track of all the places you've ventured. Employ the checkboxes to select and erase multiple entries.
- **Timezone & Local Time**: Stay informed with the timezone and local time of your most recent search.

## Prerequisites

- **Node.js and npm**: Ensure Node.js and npm are in your artillery. If they're missing, enlist them [here](https://nodejs.org/).
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

3. **Plant Your Google Maps API Key**: Unravel the specific file, then swap `YOUR_GOOGLE_MAPS_API_KEY` with your genuine key.

4. **Power Up the Development Server**:
   ```bash
   npm run serve
   ```

Your GeoMark should now be gallivanting on `http://localhost:8080/`.

## Guided Tour

1. **Spot Current Location**: A single tap on "Get Current Location" will beacon your coordinates.
2. **Scout for a Location**: Punch in a name and either strike Enter or tap "Search".
3. **Organize Your Discoveries**: The table is your treasure map. Use the checkboxes wisely and swipe away locations with "Delete".
4. **Time Travels**: Just below the map, the local time and timezone details of your recent discovery await.

## Join the Adventure (Contributing)

Should the winds of adventure beckon you, fork this repository and make your mark. All treasure maps (pull requests) are most welcome.

## Treasure Map (License)

MIT License. The [LICENSE](LICENSE) parchment holds the intricate details.