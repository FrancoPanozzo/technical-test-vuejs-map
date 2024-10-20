<script setup lang="ts">
import type { Marker } from '~/store/map'
defineProps<{ marker: Marker }>()
</script>

<template>
  <div class="wrap">
    <img v-if="marker.imageUrl" :src="marker.imageUrl" :alt="'Image of ' + marker.label" />
    <template v-else>
      {{ marker.label[0] }}
    </template>
  </div>
</template>

<style scoped>
.wrap {
  --blue-comeen: #3f92cd;
  --white: white;
  --ds-gris: #323d47;

  position: relative;
  transform: translate(-50%, -50%);
  background-color: var(--blue-comeen);
  border: 3px solid var(--blue-comeen);
  color: var(--white);
  font-weight: 500;
  border-radius: 100%;
  width: 50px;
  height: 50px;
  display: flex;
  justify-content: center;
  align-items: center;
  transition: all 0.3s;
  box-shadow: 0 1px 2px #1018280d;
  overflow: hidden;
}

.wrap:hover {
  background-color: var(--ds-gris);
}

.wrap img {
  width: 100%;
  height: 100%;
  object-fit: cover;
}
</style>
