<template>
  <div id="map">
    <!-- <li v-for="coordinate in coordinates" :key="coordinate">
      {{ coordinate }}
    </li> -->
  </div>
</template>

<script>
import Map from 'ol/Map';
import View from 'ol/View';
import TileLayer from 'ol/layer/Tile';
import OSM from 'ol/source/OSM';
import { fromLonLat } from 'ol/proj';
import VectorLayer from 'ol/layer/Vector';
import VectorSource from 'ol/source/Vector';
import Feature from 'ol/Feature';
import Point from 'ol/geom/Point';
import Icon from 'ol/style/Icon';
import Style from 'ol/style/Style';

export default {
  name: 'MapComponent',
  props: {
    coordinates: {
      type: Array,
      required: true
    }
  },
  mounted() {
    this.initMap();
  },
  methods: {
    initMap() {
      const map = new Map({
        target: 'map',
        layers: [
          new TileLayer({
            source: new OSM()
          })
        ],
        view: new View({
          center: fromLonLat([11.9746, 57.7089]),
          zoom: 12
        })
      });

      const iconStyle = new Style({
        image: new Icon({
          src: 'https://cdn3.iconfinder.com/data/icons/faticons/32/location-01-512.png',
          scale: 0.05,
          anchor: [0.5, 1],
          anchorXUnits: 'fraction',
          anchorYUnits: 'fraction'
        })
      });

      //creates the new layer for the pins
      const vectorSource = new VectorSource({
        features: this.coordinates.map(coord => new Feature({
          geometry: new Point(fromLonLat([coord[0], coord[1]])) 
        }))
      });

      const vectorLayer = new VectorLayer({
        source: vectorSource
      });

      map.addLayer(vectorLayer);
    }
  }
}
</script>

<style>
#map {
  width: 100%;
  height: 100%;
}
</style>
