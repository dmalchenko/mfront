<template>
  <div>
    <div style="height: 20%; overflow: auto;">
      <h3>Simple map</h3>
      <p>Marker is placed at {{ marker.lat }}, {{ marker.lng }}</p>
      <p> Center is at {{ currentCenter }} and the zoom is: {{ currentZoom }} </p>
      <button @click="showLongText">Toggle Long popup</button>
    </div>
    <l-map
      :zoom="zoom"
      :center="center"
      style="height: 500px; width: 800px"
      @update:center="centerUpdate"
      @update:zoom="zoomUpdate">
      <l-tile-layer
        :url="url"
        :attribution="attribution"/>
      <l-marker :lat-lng="marker">
        <l-popup>
          <div @click="popupClick">
            I am a tooltip
            <p v-show="showParagraph">
              Lorem ipsum dolor sit amet, consectetur adipiscing elit. Quisque sed pretium nisl, ut sagittis sapien. Sed vel sollicitudin nisi. Donec finibus semper metus id malesuada.
            </p>
          </div>
        </l-popup>
      </l-marker>
    </l-map>
  </div>
</template>

<script>
import { LMap, LTileLayer, LMarker, LPopup } from 'vue2-leaflet'
import L from 'leaflet'
import '../../node_modules/leaflet/dist/leaflet.css'

delete L.Icon.Default.prototype._getIconUrl

L.Icon.Default.mergeOptions({
  iconRetinaUrl: require('leaflet/dist/images/marker-icon-2x.png'),
  iconUrl: require('leaflet/dist/images/marker-icon.png'),
  shadowUrl: require('leaflet/dist/images/marker-shadow.png')
})

export default {
  name: 'Example',
  components: {
    LMap,
    LTileLayer,
    LMarker,
    LPopup
  },
  data () {
    return {
      zoom: 13,
      center: L.latLng(47.413220, -1.219482),
      url: 'http://{s}.tile.osm.org/{z}/{x}/{y}.png',
      attribution: '&copy; <a href="http://osm.org/copyright">OpenStreetMap</a> contributors',
      marker: L.latLng(47.413220, -1.219482),
      currentZoom: 13,
      currentCenter: L.latLng(47.413220, -1.219482),
      showParagraph: false
    }
  },
  methods: {
    zoomUpdate (zoom) {
      this.currentZoom = zoom
    },
    centerUpdate (center) {
      this.currentCenter = center
    },
    showLongText () {
      this.showParagraph = !this.showParagraph
    },
    popupClick () {
      alert('Popup Click!')
    }
  }
}
</script>
<style>
  .leaflet-fake-icon-image-2x {
    background-image: url(../../node_modules/leaflet/dist/images/marker-icon-2x.png);
  }
  .leaflet-fake-icon-shadow {
    background-image: url(../../node_modules/leaflet/dist/images/marker-shadow.png);
  }
  body {
    margin: 0px;
    font-family: Helvetica, Verdana, sans-serif;
  }
  #side {
    float:left;
    width:208px;
  }
  #full_div {
    position: absolute;
    overflow-x: auto;
    top: 0;
    right: 0;
    left: 208px;
    bottom: 0;
    padding-left: 8px;
    border-left: 1px solid #ccc;
  }
  ul {
    list-style-type: none;
    margin: 0;
    padding: 0;
  }
  li {
    font: 200 15px/1.5 Helvetica, Verdana, sans-serif;
    border-bottom: 1px solid #ccc;
  }
  li:last-child {
    border: none;
  }
  li a {
    font-size: 15px;
    padding-left: 8px;
    text-decoration: none;
    color: #000;
    display: block;
    -webkit-transition: font-size 0.3s ease, background-color 0.3s ease;
    -moz-transition: font-size 0.3s ease, background-color 0.3s ease;
    -o-transition: font-size 0.3s ease, background-color 0.3s ease;
    -ms-transition: font-size 0.3s ease, background-color 0.3s ease;
    transition: font-size 0.3s ease, background-color 0.3s ease;
  }
  li a:hover {
    font-size: 20px;
    background: #f6f6f6;
  }
</style>
