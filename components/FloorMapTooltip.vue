<script setup lang="ts">
import type { Marker } from '~/store/map'
defineProps<{ marker: Marker }>()
</script>

<template>
  <div class="wrap">
    <h3>Desk {{ marker.id }}</h3>
    <span
      >occupied by <strong>{{ marker.label }}</strong></span
    >
  </div>
</template>

<style scoped>
.wrap {
  --blue-comeen: #3f92cd;
  --white: white;
  --ds-gris: #323d47;
  margin: 1rem 0;
}

.wrap h3 {
  color: var(--blue-comeen);
  font-size: 1.125rem;
  font-weight: 600;
  margin: 0;
  margin-bottom: 0.25rem;
}

.wrap span {
  font-size: 0.875rem;
}

.wrap strong {
  font-weight: 600;
}
</style>
