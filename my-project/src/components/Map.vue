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
import {toLonLat} from 'ol/proj';

export default {
  name: 'MapComponent',
  props: {
    coordinates: {
      type: Array,
      default: () => [],
    }
  },
  data()
  {
    return {
    map: null,
    vectorLayer: null,
    iconStyle: null,
    mappedCoordinates: [],
    clickedRaaId: null,
    }
  },
  mounted() {
    this.initMap();
  },
  watch: {
    coordinates: {
      deep: true,
      handler() {
        this.coordinatesMapped = this.coordinates.map(result => result.coordinates.coordinates);
      },
    },
    coordinatesMapped: {
      deep: true,
      handler() {
        this.updateCoordinates();
      },
    },
  },
  methods: {
   initMap() {
  this.map = new Map({
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

      this.iconStyle = new Style({
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

      this.vectorLayer = new VectorLayer({
        source: vectorSource
      });

     this.map.addLayer(this.vectorLayer);

    // Add 'click' event listener
this.map.on('click', (event) => {
  this.map.forEachFeatureAtPixel(event.pixel, (feature) => {
    const coordinates = feature.getGeometry().getCoordinates();
    const lonLat = toLonLat(coordinates);
    const raa_id = feature.get('raa_id');
    console.log('Clicked coordinates:', lonLat.reverse());
    console.log('Clicked raa_id:', raa_id);
    this.clickedRaaId = raa_id;
    this.$emit('raa-id-selected', raa_id);
  });
});


},
  updateCoordinates() {
  // creates the new layer for the pins
  const vectorSource = new VectorSource({
    features: this.coordinates.map(result => {
      const coord = result.coordinates.coordinates;
      const feature = new Feature({
        geometry: new Point(fromLonLat([coord[0], coord[1]]))
      });
      feature.set('raa_id', result.raa_id); // Add raa_id as a property of the feature
      feature.setStyle(this.iconStyle); // Apply the iconStyle to the feature
      return feature;
    })
  });

  this.vectorLayer = new VectorLayer({
    source: vectorSource
  });

  this.map.addLayer(this.vectorLayer);
}

  },
  computed: {
    coordinatesMapped: {
      get() {
        return this.coordinates.map(result => result.coordinates.coordinates);
      },
      set(value) {
        this.mappedCoordinates = value;
      },
    },
  }
}
</script>

<style>
#map {
  width: 100%;
  height: 100%;
}
</style>
