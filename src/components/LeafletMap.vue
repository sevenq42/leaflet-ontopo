<template>
  <div class="leaflet-map" style="direction: rtl;">
    <l-map ref="map" v-model:zoom="zoom" v-model:center="mapCenter">
      <l-tile-layer
        url="https://{s}.basemaps.cartocdn.com/light_all/{z}/{x}/{y}.png"
        layer-type="base"
        name="OpenStreetMap"
        :max-zoom="MAX_ZOOM"
      />
      <l-marker draggable v-model:lat-lng="markerPos" />
    </l-map>

    <div class="paramers-editor column q-ma-md">
      <q-input
        outlined
        class="col-2"
        label="Zoom"
        hint="zoom"
        type="number"
        :max="MAX_ZOOM"
        :min="1"
        v-model.number="zoom"
      />
      <q-input
        outlined
        class="col-4"
        label="Latitude"
        hint="lat"
        type="number"
        :step="COORDS_STEP"
        :max="180"
        :min="-180"
        v-model.number="markerPosLat"
      />
      {{ markerPos.lng }}
      <q-input
        outlined
        class="col-4"
        label="Longitude"
        hint="lng"
        type="number"
        :step="COORDS_STEP"
        :max="180"
        :min="-180"
        v-model.number="markerPosLng"
      />
    </div>
  </div>
</template>

<script>
import { defineComponent } from "vue"
import 'leaflet/dist/leaflet.css'
import L from 'leaflet'
import { LMap, LTileLayer, LMarker } from "@vue-leaflet/vue-leaflet"
export default defineComponent({
  name: 'LeafletMap',
  props: {
    initLat: {
      type: Number,
    },
    initLng: {
      type: Number,
    },
    initZoom: {
      type: Number,
    },
    initMapCenter: {
      type: [Object, Array],
    },

  },
  components: {
    LMap,
    LTileLayer,
    LMarker,
  },
  emits: ['update'],
  data(vm) {
    return {
      zoom: vm.initZoom ?? 2,
      markerPosLat: vm.initLat ?? 32.063,
      markerPosLng: vm.initLng ?? 34.76845,
      mapCenter: vm.initMapCenter ?? { lat: 34.76845, lng: 32.063 },
      MAX_ZOOM: 19,
      COORDS_STEP: 0.01,
    };
  },
  computed: {
    markerPos: {
      get() {
        return {
          lat: this.markerPosLat,
          lng: this.markerPosLng,
        }
      },
      set(newVal) {
        if (newVal.lng && this.markerPosLng !== newVal.lng) {
          this.markerPosLng = newVal.lng
        }
        if (newVal.lat && this.markerPosLat !== newVal.lat) {
          this.markerPosLat = newVal.lat
        }
      }
    }
  },
})
</script>

      // markerIcon: new L.icon({
      //   "iconUrl": "marker-icon.png",
      //   "iconRetinaUrl": "marker-icon-2x.png",
      //   "shadowUrl": "marker-shadow.png",
      //   "iconSize": [
      //     25,
      //     41
      //   ],
      //   "iconAnchor": [
      //     12,
      //     41
      //   ],
      //   "popupAnchor": [
      //     1,
      //     -34
      //   ],
      //   "tooltipAnchor": [
      //     16,
      //     -28
      //   ],
      //   "shadowSize": [
      //     41,
      //     41
      //   ]
      // })
<style lang="scss" scoped>
.leaflet-map {
  height: 600px;
  width: 800px;
}
</style>