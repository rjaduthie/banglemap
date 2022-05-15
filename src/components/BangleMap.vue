<template>
  <div id="map-container">
    <l-map
        :center="center"
        :zoom="zoom"
        style="height: 70pc; width:80%; margin: auto;"
    >
      <l-tile-layer
          :attribution="attribution"
          :url="url"
      />
      <l-polyline
          ref="path"
          :color="polyline.color"
          :lat-lngs="[]"
      />
    </l-map>
  </div>
</template>

<script>
import "leaflet/dist/leaflet.css"
import {LMap, LPolyline, LTileLayer} from "@vue-leaflet/vue-leaflet";
import {ref} from "vue";


export default {
  name: "BangleMap",
  props: {
    runData: Array
  },
  components: {
    LMap,
    LTileLayer,
    LPolyline
  },
  setup() {
    // Not convinced I understand all this ref stuff
    const path = ref();
    return {path};
  },
  methods: {
    runDataLatLongs() {
      let newData = []
      if (this.runData != null) {
        newData = this.runData.map(
            row => [row.latitude, row.longitude, row.altitude]
        ).filter(row => row.every(element => element !== undefined))
      }
      this.$refs.path.leafletObject.setLatLngs(newData)
      this.polyline.latLongs = newData
    }
  },
  data() {
    return {
      zoom: 14,
      center: [52.2851, 0.1247],  // take mean of initial data
      polyline: {
        color: "red",
        latLongs: []
      },
      url: "https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png",
      attribution: "&copy; <a href=\"http://osm.org/copyright\">OpenStreetMap</a> contributors"
    };
  },
  watch: {
    // Watch is apparently not the only way: https://dev.to/vcpablo/vuejs-2-different-ways-to-implement-v-model-1mjf
    runData() {
      this.runDataLatLongs()
    }
  }
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
