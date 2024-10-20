<script setup lang="ts">
import { createVNode, render } from 'vue'
import 'leaflet/dist/leaflet.css'
import leaflet from 'leaflet'
import type { Marker } from '~/store/map'
import FloorMapMarker from '~/components/FloorMapMarker.vue'
import FloorMapTooltip from '~/components/FloorMapTooltip.vue'

const props = defineProps<{
  markers: Marker[]
  floorPlanUrl: string
}>()

let map: leaflet.Map
const markerMap = new Map<number, leaflet.Marker>()

onMounted(() => {
  const imageWidth = 2159 // Image width in pixels
  const imageHeight = 1079 // Image height in pixels

  const imageBounds = [
    // Top-Left corner of the image
    [0, 0],
    [imageHeight, imageWidth]
    // Bottom-Right corner of the image
  ] as leaflet.LatLngBoundsExpression

  map = leaflet
    .map('floor-map', {
      crs: leaflet.CRS.Simple, // Use simple CRS for non-geographical maps
      maxZoom: 2,
      minZoom: -2
    })
    .fitBounds(imageBounds)

  leaflet.imageOverlay(props.floorPlanUrl, imageBounds).addTo(map)

  props.markers.forEach((marker) => {
    // Create a container div for the marker content
    const icon = document.createElement('div')

    // Create a virtual node for your CustomMarker, passing any necessary props
    const iconVirtualNnode = createVNode(FloorMapMarker, { marker })

    // Render the virtual node to the div
    render(iconVirtualNnode, icon)

    const customIcon = leaflet.divIcon({ html: icon.innerHTML })
    const leafletMarker = leaflet.marker([marker.y, marker.x], { icon: customIcon }).addTo(map)

    markerMap.set(marker.id, leafletMarker)

    const popup = document.createElement('div')
    const popupVirtualNode = createVNode(FloorMapTooltip, { marker })
    render(popupVirtualNode, popup)

    const customPopup = leaflet.popup({ content: popup.innerHTML, offset: [-4, -20] })

    leafletMarker.bindPopup(customPopup)
  })
})

const zoomIn = (marker: Marker) => {
  if (!props.markers.find((m) => m.id === marker.id)) {
    console.error('Trying to zoom on a marker that is not in the markers array')
    return
  }

  const leafletMarker = markerMap.get(marker.id)
  if (!leafletMarker) {
    console.error('Leaflet marker not found for the given marker ID')
    return
  }

  leafletMarker.openPopup()
  map.setView([marker.y, marker.x], 0)
}

defineExpose({ zoomIn })
</script>

<template>
  <div id="floor-map"></div>
</template>
