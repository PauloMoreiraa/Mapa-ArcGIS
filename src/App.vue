<template>
  <div id="viewDiv" class="map-container"></div>
  <div class="box">
    <div class="base-map" @click="toggleBaseMapMenu"></div>
    <div v-if="showBaseMapMenu" class="base-map-menu">
      <div v-for="(basemap, index) in baseMaps" :key="index" class="base-map-option" @click="changeBaseMap(basemap)">
        {{ basemap }}
      </div>
    </div>
  </div>
</template>

<script>
import { ref, onMounted } from "vue";
import Map from "@arcgis/core/Map";
import MapView from "@arcgis/core/views/MapView";
import FeatureLayer from "@arcgis/core/layers/FeatureLayer";

export default {
  name: "ArcGISMap",
  setup() {
    const showBaseMapMenu = ref(false);
    const baseMaps = ref(["streets", "satellite", "hybrid", "topo-vector", "dark-gray"]);
    let map;
    let view;

    const toggleBaseMapMenu = () => {
      showBaseMapMenu.value = !showBaseMapMenu.value;
    };

    const changeBaseMap = (basemap) => {
      if (map) {
        map.basemap = basemap;
      }
      showBaseMapMenu.value = false; 
    };

    onMounted(() => {
      map = new Map({
        basemap: "streets-navigation-vector", 
      });

      view = new MapView({
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

    return {
      showBaseMapMenu,
      toggleBaseMapMenu,
      baseMaps,
      changeBaseMap,
    };
  },
};
</script>

<style>
.map-container {
  width: 100%;
  height: 100vh;
  z-index: 1;
  position: absolute;
}

.box {
  z-index: 2;
  position: absolute;
  width: 10vw;
  right: 0;
  height: 100vh;
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 10px;
  flex-direction: column;
}

.base-map {
  width: 70px;
  height: 70px;
  background-color: #f6f6f6;
  border-radius: 50%;
  box-shadow: 0 0 10px #00000028;
  cursor: pointer;
  display: flex;
  align-items: center;
  justify-content: center;
}

.base-map-menu {
  position: absolute;
  right: 80px;
  background: white;
  box-shadow: 0 0 10px #00000028;
  padding: 10px;
  border-radius: 8px;
  display: flex;
  flex-direction: column;
}

.base-map-option {
  padding: 5px 10px;
  cursor: pointer;
  border-bottom: 1px solid #ddd;
}

.base-map-option:last-child {
  border-bottom: none;
}

.base-map-option:hover {
  background: #f0f0f0;
}
</style>