<script setup>
import { ref, onMounted } from 'vue'
import MapView from '@arcgis/core/views/MapView'
import Map from '@arcgis/core/Map'
import FeatureLayer from '@arcgis/core/layers/FeatureLayer'
import BasemapGallery from '@arcgis/core/widgets/BasemapGallery'
import Expand from '@arcgis/core/widgets/Expand' // Importe o Expand widget

const mapDiv = ref(null)
const showLayer = ref(true)

const featureLayer = new FeatureLayer({
  url: 'https://services3.arcgis.com/cS4GcXNpyMgMVA4J/arcgis/rest/services/Mapa_Teste/FeatureServer/2',
  visible: showLayer.value,
})

let view = null

onMounted(() => {
  const map = new Map({
    basemap: 'streets-vector',
  })

  view = new MapView({
    container: mapDiv.value,
    map: map,
    center: [-46.633, -23.550],
    zoom: 12,
  })

  // Cria o BasemapGallery
  const basemapGallery = new BasemapGallery({
    view: view,
  })

  // Cria o Expand widget e passa o BasemapGallery como conteúdo
  const expand = new Expand({
    view: view,
    content: basemapGallery,
    expanded: false, // Inicia recolhido
    expandIcon: 'basemap', // Ícone personalizado (opcional)
    expandTooltip: 'Mostrar Basemaps', // Tooltip quando recolhido
    collapseTooltip: 'Ocultar Basemaps', // Tooltip quando expandido
  })

  // Adiciona o Expand widget à UI do mapa
  view.ui.add(expand, 'top-right')
})
</script>

<template>
  <div>
    <h1>Mapa do ArcGIS</h1>
    <div ref="mapDiv" style="width: 100vw; height: 70vh;"></div>
  </div>
</template>

<style scoped>
/* Estilos personalizados podem ser adicionados aqui */
</style>