<template>
  <div class="leaflet-map" style="direction: rtl;">
    <l-map ref="map" v-model:zoom="zoom" v-model:center="mapCenter">
      <l-tile-layer
        url="https://{s}.basemaps.cartocdn.com/light_all/{z}/{x}/{y}.png"
        layer-type="base"
        name="OpenStreetMap"
        :max-zoom="MAX_ZOOM"
      />
      <!-- <l-tile-layer
        url="https://s3.amazonaws.com/te512.safecast.org/{z}/{x}/{y}.png"
        attribution="<a href='https://blog.safecast.org/about/'>SafeCast</a> (<a href='https://creativecommons.org/licenses/by-sa/3.0/'>CC-BY-SA</a>"
        :min-zoom="5"
        :max-zoom="7"
      />-->

      <l-marker draggable v-model:lat-lng="markerPos" @moveend="onMoveMarker" />
    </l-map>

    <div class="paramers-editor row">
      <q-input
        class="col-2"
        label="Zoom"
        hint="zoom"
        type="number"
        :max="MAX_ZOOM"
        :min="1"
        v-model.number="zoom"
      />
      <q-input
        class="col-4"
        label="Latitude"
        hint="lat"
        type="number"
        :max="180"
        :min="-180"
        v-model.number="markerPosLat"
      />
      {{ markerPos.lng }}
      <q-input
        class="col-4"
        label="Longitude"
        hint="lng"
        type="number"
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
  components: {
    LMap,
    LTileLayer,
    LMarker,
  },
  data() {
    return {
      zoom: 2,
      markerPosLng: 34.76845,
      markerPosLat: 32.063,
      mapCenter: { lat: 34.76845, lng: 32.063 },
      MAX_ZOOM: 19,
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
  methods: {
    onMoveMarker(evt) {
      console.log('map', this.$refs.map)
      console.log('map center', this.$refs.map?.center)
      console.log('marker moved', evt)
      console.log('target', evt.target._latlng)
      console.log('sourceTarget', evt.sourceTarget._latlng)
      console.log('zoom', this.zoom)
      console.log('markerPos', this.markerPos)
      console.log('mapCenter', this.mapCenter)
      // this.markerPos = evt.target
    }
  }
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