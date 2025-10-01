<!doctype html>
<html>
<head>
  <meta charset="utf-8">
  <title>Plane Tracker</title>
  <link rel="stylesheet" href="https://unpkg.com/leaflet/dist/leaflet.css"/>
  <style>
    body, html, #map { height: 100%; margin:0; }
  </style>
</head>
<body>
  <div id="map"></div>

  <script src="https://unpkg.com/leaflet/dist/leaflet.js"></script>
  <script>
    // Make a map
    const map = L.map('map').setView([51.5, -0.12], 5); // Center on Europe, zoom 5
    L.tileLayer('https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png', {
      attribution: '© OpenStreetMap'
    }).addTo(map);

    async function loadPlanes() {
      // Get plane data from OpenSky
      const res = await fetch("https://opensky-network.org/api/states/all");
      const data = await res.json();

      // Remove old markers
      if (window.markers) {
        window.markers.forEach(m => map.removeLayer(m));
      }
      window.markers = [];

      // Go through the plane list
      for (let plane of data.states.slice(0, 50)) { // only show 50 planes
        const lat = plane[6];
        const lon = plane[5];
        const callsign = plane[1];
        const altitude = plane[13];
        if (lat && lon) {
          const marker = L.marker([lat, lon]).addTo(map);
          marker.bindPopup(`<b>${callsign || "Unknown"}</b><br>
            Alt: ${altitude} m`);
          window.markers.push(marker);
        }
      }
    }

    loadPlanes();
    setInterval(loadPlanes, 5000); // refresh every 5 seconds
  </script>
</body>
</html>
