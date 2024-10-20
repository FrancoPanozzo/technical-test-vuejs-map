<template>
  <div>
    <h1>Floor Map</h1>
    <FloorMap ref="floorMap" :markers="markers" :floorPlanUrl="floorPlanUrl" />
    <div class="buttons">
      <button v-for="marker in markers" class="btn" @click="zoomIn(marker)">Find {{ marker.label }}</button>
    </div>
  </div>
</template>

<script setup lang="ts">
import { useMapStore } from '~/store/map'
import type { Marker } from '~/store/map'

const { markers, floorPlanUrl } = useMapStore()
const floorMap = ref()

const zoomIn = (marker: Marker) => {
  // use a method in FloorMap (exposed by composition API)
  floorMap.value.zoomIn(marker)
}
</script>

<style scoped>
#floor-map {
  width: 90vw;
  aspect-ratio: 2/1;
}

.buttons {
  display: flex;
  gap: 1rem;
  margin-top: 1rem;
}

.btn {
  --blue-comeen: #3f92cd;
  --white: white;
  --ds-gris: #323d47;

  cursor: pointer;
  border: 1px solid var(--blue-comeen);
  background-color: var(--blue-comeen);
  color: var(--white);
  text-align: center;
  white-space: nowrap;
  border-radius: 0.5rem;
  justify-content: center;
  align-items: center;
  padding: 0.625rem 1.125rem;
  font-size: 1rem;
  font-weight: 600;
  line-height: 1.5;
  transition: all 0.3s;
  box-shadow: 0 1px 2px #1018280d;

  margin-bottom: 1rem;
}

.btn:hover {
  background-color: var(--ds-gris);
  border-color: var(--ds-gris);
}
</style>
