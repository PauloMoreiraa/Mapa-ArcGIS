<template>
  <div id="viewDiv" class="map-container"></div>
</template>

<script>
import { onMounted } from "vue";
import Map from "@arcgis/core/Map";
import MapView from "@arcgis/core/views/MapView";
import FeatureLayer from "@arcgis/core/layers/FeatureLayer";

export default {
  name: "ArcGISMap",
  setup() {
    onMounted(() => {
      const map = new Map({
        basemap: "streets-navigation-vector", 
      });

      const view = new MapView({
        container: "viewDiv",
        map: map,
        center: [4.4, 51.2], 
        zoom: 12,
      });

      const featureLayer = new FeatureLayer({
        url: "https://geodata.antwerpen.be/arcgissql/rest/services/P_Portal/extern_Deelmobiliteit/MapServer/5",
      });

      map.add(featureLayer);
    });
  },
};
</script>

<style>
.map-container {
  width: 100%;
  height: 100vh;
}
</style>