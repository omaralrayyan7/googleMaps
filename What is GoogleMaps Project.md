# What is GoogleMaps Project

## Overview

A **single-page Google Maps web app** that detects or defaults the user's location, then lets them search for nearby points of interest by category (restaurants, cafes, hospitals, pharmacies, ATMs, supermarkets) within a configurable radius. Results appear as map markers with custom info-card overlays on hover.

**Tech stack:** Vanilla HTML/CSS/JS · Google Maps JavaScript API · Places API (Nearby Search)

---

## Key Code Segments

### Map Initialisation
Sets the default center to Amman, Jordan, and creates a `PlacesService` for nearby search.

```js
function initMap() {
  map = new google.maps.Map(document.getElementById("map"), {
    center: { lat: 31.9454, lng: 35.9284 }, zoom: 15
  });
  service = new google.maps.places.PlacesService(map);
}
```

### Geolocation — Locate Me
Requests the browser's GPS position and re-centers the map on the real user location.

```js
function locateMe() {
  navigator.geolocation.getCurrentPosition(pos => {
    userLocation = { lat: pos.coords.latitude, lng: pos.coords.longitude };
    map.setCenter(userLocation);
    meMarker.setPosition(userLocation);
  });
}
```

### Nearby Search by Category
Calls the Places API and drops a pin for each result within the chosen radius.

```js
function searchNearby(type) {
  clearMarkers();
  service.nearbySearch(
    { location: userLocation, radius: parseInt(rangeInput.value), type },
    (results, status) => {
      if (status === google.maps.places.PlacesServiceStatus.OK) {
        results.forEach(place => addMarker(place));
      }
    }
  );
}
```