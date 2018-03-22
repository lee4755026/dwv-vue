<template>
  <div id="dwv">
    <div class="button-row">
      <button value="Scroll" v-on:click="onClick">Scroll</button>
      <button value="WindowLevel" v-on:click="onClick">WindowLevel</button>
      <button value="ZoomAndPan" v-on:click="onClick">ZoomAndPan</button>
    </div>
    <div class="layerContainer">
      <div class="dropBox"></div>
      <canvas class="imageLayer">Only for HTML5 compatible browsers...</canvas>
    </div>
    <div class="legend">{{ legend }}</div>
  </div>
</template>

<script>
// import
import Vue from 'vue'
import dwv from 'dwv'

// gui overrides

// decode query
dwv.utils.decodeQuery = dwv.utils.base.decodeQuery
// progress
dwv.gui.displayProgress = function () {}
// window
dwv.gui.getWindowSize = dwv.gui.base.getWindowSize
// get element
dwv.gui.getElement = dwv.gui.base.getElement
// refresh element
dwv.gui.refreshElement = dwv.gui.base.refreshElement

export default {
  name: 'dwv',
  data: function () {
    return {
      legend: 'Powered by dwv ' + dwv.getVersion() + ' and Vue.js ' + Vue.version,
      dwvApp: null
    }
  },
  mounted () {
    // create app
    this.dwvApp = new dwv.App()
    // initialise app
    this.dwvApp.init({
      'containerDivId': 'dwv',
      'fitToWindow': true,
      'tools': ['Scroll', 'ZoomAndPan', 'WindowLevel'],
      'isMobile': true
    })
  },
  methods: {
    onClick: function (event) {
      this.dwvApp.onChangeTool(event)
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
#dwv { font-family: Arial, Helvetica, sans-serif; }

.button-row {
  text-align: center;
  justify-content: space-around;
}
button {
    margin: 5px; padding: 10px 10px;
    border: 0 none; border-radius: 4px;
    font-size: 14px; font-weight: 600;
    color: #fff; background-color: #444; }
button:hover { background-color: #555; }
button:disabled { background-color: #08b; }

.legend {
  text-align: center;
  font-size: 8pt;
}

/* Layers */
.layerContainer {
    position: relative; padding: 0;
    margin: auto; text-align: center; }
.imageLayer {
    position: absolute;
    left: 0px; }

/* drag&drop */
.dropBox {
    border: 5px dashed #ccc;
    margin: auto; text-align: center; }
.dropBox.hover { border: 5px dashed #cc0; }
</style>