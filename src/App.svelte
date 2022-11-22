<script>
  import { onMount } from "svelte";
  import { Map } from "@onsvisual/svelte-maps";
  import maplibre, { Marker } from "maplibre-gl";
  // import marker_icon from "$lib/assets/icons/marker_icon.png";

  let map;
  // State
  let zoom;
  let center = {};

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

  onMount(() => {
    map.addControl(new maplibre.NavigationControl(), "top-left");

    geojson.features.forEach(function (marker) {
      // create a DOM element for the marker
      // var el = document.createElement("div");
      // el.className = "marker";
      // el.style.backgroundImage = "url('/icons/p3.png')";
      // el.style.backgroundSize = "cover";
      // el.style.backgroundRepeat = "no-repeat";
      // el.style.backgroundPosition = "center center";
      // el.style.width = "80px";
      // el.style.height = "80px";

      // add marker to map
      new Marker({ color: "#FF0000" })
        .setLngLat(marker.geometry.coordinates)
        .addTo(map);
      // map.on("idle", () => {
      //   // map.getCanvas().toDataURL()
      //   map.setLayoutProperty("label_country", "text-field", [
      //     "get",
      //     "name:" + "es",
      //   ]);
      // });
    });

    document
      .getElementById("buttons")
      .addEventListener("click", function (event) {
        var language = event.target.id.substr("button-".length);
        // Use setLayoutProperty to set the value of a layout property in a style layer.
        // The three arguments are the id of the layer, the name of the layout property,
        // and the new property value.
        map.setLayoutProperty("label_country", "text-field", [
          "get",
          "name:" + language,
        ]);
      });
  });

  // map.on("style.load", () => {
  //   const waiting = () => {
  //     if (!map.isStyleLoaded()) {
  //       setTimeout(waiting, 200);
  //     } else {
  //       map.setLayoutProperty("label_country", "text-field", [
  //         "get",
  //         "name:" + "es",
  //       ]);
  //       return;
  //     }
  //   };
  //   waiting();
  // });

  // map.on("idle", () => {
  //   // map.getCanvas().toDataURL()
  //   map.setLayoutProperty("label_country", "text-field", [
  //     "get",
  //     "name:" + "es",
  //   ]);
  // });
</script>

<main>
  <ul id="buttons">
    <li id="button-fr" class="button">French</li>
    <li id="button-ru" class="button">Russian</li>
    <li id="button-de" class="button">German</li>
    <li id="button-es" class="button">Spanish</li>
  </ul>
  <Map
    id="map"
    style="https://api.maptiler.com/maps/basic/style.json?key=7URvU9qVvERzegw0nHd2"
    location={{ lng: 143.77, lat: 52.87, zoom: 5 }}
    bind:map
    bind:zoom
    bind:center
  />
</main>

<style>
  #buttons {
    width: 90%;
    margin: 0 auto;
  }
  .button {
    display: inline-block;
    position: relative;
    cursor: pointer;
    width: 20%;
    padding: 8px;
    border-radius: 3px;
    margin-top: 10px;
    font-size: 12px;
    text-align: center;
    color: #fff;
    background: #ee8a65;
    font-family: sans-serif;
    font-weight: bold;
  }
  main {
    width: 100%;
    height: 100vh;
    position: relative;
  }
  .marker {
    display: block;
    border: none;
    border-radius: 50%;
    cursor: pointer;
    padding: 0;
  }
  /* body { margin: 0; padding: 0; } */
  /* main { position: absolute; top: 0; bottom: 0; width: 100%; } */
</style>
