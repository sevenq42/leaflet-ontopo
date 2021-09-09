<template>
  <q-page class="row items-center justify-evenly">
    LEAFLET:
    <leaflet-map />
  </q-page>
</template>

<script lang="ts">
import { defineComponent, ref } from 'vue';
import LeafletMap from 'src/components/LeafletMap.vue';

export default defineComponent({
  name: 'PageIndex',
  components: { LeafletMap },
  setup() {
    return {};
  }
});
</script>
