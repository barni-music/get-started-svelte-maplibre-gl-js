<script>
  import { onMount, onDestroy } from "svelte";
  import { Map, NavigationControl, Marker } from "maplibre-gl";
  import "maplibre-gl/dist/maplibre-gl.css";
  import MapboxLanguage from "@mapbox/mapbox-gl-language";

  let map;
  let mapContainer;

  var geojson = {
    type: "FeatureCollection",
    features: [
      {
        type: "Feature",
        properties: {
          name: "Molikpaq",
        },
        geometry: {
          coordinates: [143.6635, 52.5977],
          type: "Point",
        },
      },
      {
        type: "Feature",
        properties: {
          name: "LUN-A",
        },
        geometry: {
          coordinates: [143.8221, 51.5893],
          type: "Point",
        },
      },
      {
        type: "Feature",
        properties: {
          name: "PA-B",
        },
        geometry: {
          coordinates: [143.6662, 53.0081],
          type: "Point",
        },
      },
    ],
  };

  const language = new MapboxLanguage({
    defaultLanguage: "en",
    // defaultLanguage: "ru",
  });

  // https://www.gibbard.me/openstreetmap/
  // "left=122.4269 bottom=40.9802 right=163.0818 top=55.9088"

  onMount(() => {
    const apiKey = "7URvU9qVvERzegw0nHd2";
    // const accessToken = '08s48uGtCUzpInN46rhF8N44pnN1LfErONeFsnKBYXjJ2HPzofCdLiRmtSfTXaEK';

    const initialState = { lng: 143.8221, lat: 51.5893, zoom: 5 };

    map = new Map({
      container: mapContainer,
      style: `https://api.maptiler.com/maps/bbf74fff-2f81-4c6c-b7e3-3d073500ce52/style.json?key=${apiKey}`,
      // style: `https://api.jawg.io/styles/jawg-dark.json?lang=ru&access-token=${accessToken}`,
      center: [initialState.lng, initialState.lat],
      zoom: initialState.zoom,
    });
    map.addControl(new NavigationControl(), "top-right");
    map.addControl(language);

    map.on("load", function () {
      // map.setLayoutProperty("label_country", "text-field", ["get", "name:ru"]);

      geojson.features.forEach(function (marker) {
        new Marker({ color: "#FF0000" })
          .setLngLat(marker.geometry.coordinates)
          .addTo(map);
      });
    });
  });

  onDestroy(() => {
    map.remove();
  });
</script>

<div class="map-wrap">
  <div class="map" id="map" bind:this={mapContainer} />
</div>

<style>
  .map-wrap {
    position: relative;
    width: 100%;
    height: calc(
      100vh - 77px
    ); /* calculate height of the screen minus the heading */
  }

  .map {
    position: absolute;
    width: 100%;
    height: 100vh;
  }
</style>
