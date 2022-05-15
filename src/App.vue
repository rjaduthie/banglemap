<template>
  <div>
    <p>My File Selector:
      <bangle-run-data-selector v-model="file"
                                @change="fileSelected($event)"></bangle-run-data-selector>
    </p>
    <p v-if="file">{{ file.name }}</p>
    <p v-else>No file selected.</p>
  </div>
  <BangleMap v-model:run-data="runData"/>
</template>

<script>
import BangleMap from './components/BangleMap.vue'
import BangleRunDataSelector from "@/components/BangleRunDataSelector";
import Papa from "papaparse";

export default {
  name: 'App',
  components: {
    BangleMap,
    BangleRunDataSelector
  },
  data() {
    return {file: null, runData: null}
  },
  methods: {
    fileSelected: function (event) {
      Papa.parse(event.target.files[0], {
        header: true, complete: (results) => {
          this.runData = results.data
        }
      })
    }
  }
}
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
