<template>
  
  <select id="layer-change">
    <option selected value="mapbox://styles/mapbox/streets-v11">Dark</option>
    <option value="mapbox://styles/mapbox/outdoors-v11">outdoors</option>
    <option value="mapbox://styles/mapbox/light-v10">light</option>
    <option value="mapbox://styles/mapbox/dark-v10">dark</option>
    <option value="mapbox://styles/mapbox/satellite-v9">satellite</option>
    <option value="mapbox://styles/mapbox/satellite-streets-v11">
      satellite-streets
    </option>
    <option value="mapbox://styles/mapbox/navigation-day-v1">
      navigation-day
    </option>
    <option value="mapbox://styles/mapbox/navigation-night-v1">
      navigation-night
    </option>
  </select>
  <link rel="stylesheet" href="https://api.mapbox.com/mapbox-gl-js/plugins/mapbox-gl-geocoder/v5.0.0/mapbox-gl-geocoder.css" type="text/css">
  <main class="w-screen h-screen">
    <v-map
      class="w-full h-full"
      :options="data.options"
      @loaded="onMapLoaded"
    />
    
  </main>
</template>
<script setup lang="ts">
import MapboxGeocoder from '@mapbox/mapbox-gl-geocoder';
import '@mapbox/mapbox-gl-geocoder/dist/mapbox-gl-geocoder.css';
import mapboxgl from "mapbox-gl";
import VMap from "v-mapbox";

const data = reactive({
  options: {
    accessToken:
      "pk.eyJ1IjoibWF5dXJ3YWtpa2FyIiwiYSI6ImNsNmdjdGxwbjBiNGMzY282bWh0dng2c2kifQ.y-m4-zQKOeOOnDG5I1u6ng",
    style: "mapbox://styles/mapbox/streets-v11?optimize=true",
    center: [-284.8618412017822, 20.253581321936355],
    zoom: 11,
    maxZoom: 22,
    crossSourceCollisions: false,
    failIfMajorPerformanceCaveat: false,
    attributionControl: false,
    preserveDrawingBuffer: true,
    hash: false,
    minPitch: 0,
    maxPitch: 60,
  } as mapboxgl.MapboxOptions,
});

function onMapLoaded(map: mapboxgl.Map) {
  console.log(map);
  const marker = new mapboxgl.Marker({
    draggable: true,
    color: "#333",
  });

  marker.setLngLat([-286.160888671875, 18.505656663040547]);
  marker.addTo(map);
  map.on("click", (e) => {
    console.log(e.lngLat.lat, e.lngLat.lng);
    new mapboxgl.Marker({
      draggable: true,
      color: getRandomColor(),
    })
      .setLngLat([e.lngLat.lng, e.lngLat.lat])
      .addTo(map);
    new mapboxgl.Popup()
      .setLngLat(e.lngLat)
      .setHTML(`Country name: ${e.features[0].properties.name}`)
      .addTo(map);
  });

  const setStyle: any = document.getElementById("layer-change");
  setStyle.addEventListener("change", (event) => {
    console.log(event);
    map.setStyle(event.target.value);
  });
  map.addLayer({
    id: "points-of-interest",
    source: {
      type: "vector",
      url: "mapbox://mapbox.mapbox-streets-v8",
    },
    "source-layer": "poi_label",
    type: "circle",
    paint: {
    },
    layout: {
    },
  });

  // map.flyTo({
  //   center: [-286.160888671875, 18.505656663040547],
  //   zoom: 9,
  //   speed: 0.7,
  //   curve: 1,
  //   easing(t) {
  //     return t;
  //   },
  // });

  

  function getRandomColor() {
    var letters = "0123456789ABCDEF";
    var color = "#";
    for (var i = 0; i < 6; i++) {
      color += letters[Math.floor(Math.random() * 16)];
    }
    return color;
  }

  // Add geolocate control to the map.
  map.addControl(
    new mapboxgl.GeolocateControl({
      positionOptions: {
        enableHighAccuracy: true,
      },
      // When active the map will receive updates to the device's location as it changes.
      trackUserLocation: true,
      // Draw an arrow next to the location dot to indicate which direction the device is heading.
      showUserHeading: true,
    })
  );
  // Add zoom and rotation controls to the map.
  map.addControl(new mapboxgl.NavigationControl());

  //map controls for saerching  
  // map.addControl(
  //   new MapboxGeocoder({
  // accessToken: mapboxgl.accessToken,
  // mapboxgl: mapboxgl
  // })
  //    );
     map.addControl(
    new MapboxGeocoder({
      accessToken: mapboxgl.accessToken,
      mapboxgl: mapboxgl
    })
  );
}

</script>
<style>
html,
body {
  margin: 0;
}
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}
.w-screen {
  width: 100vw;
}
.h-screen {
  height: 100vh;
}
.h-full {
  height: 100%;
}
.w-full {
  width: 100%;
}
</style>