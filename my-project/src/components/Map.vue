<template>
    <div id="map"></div>
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
    mounted() {
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

        const coordinates = [
            [11.9746, 57.7089],
            [11.9709, 57.7066],
            [11.9815, 57.6995]
        ];

        const iconStyle = new Style({
            image: new Icon({
                src: 'https://upload.wikimedia.org/wikipedia/commons/9/93/Map_marker_font_awesome.svg',
                scale: 5,
                anchor: [0.5, 1],
                anchorXUnits: 'fraction',
                anchorYUnits: 'fraction'
            })
        });

        const vectorSource = new VectorSource({
            features: coordinates.map(coord => new Feature({
                geometry: new Point(fromLonLat(coord))
            }))
        });

        const vectorLayer = new VectorLayer({
            source: vectorSource,
            style: iconStyle
        });

        map.addLayer(vectorLayer);
    }
}
</script>
<style>
#map {
    width: 100%;
    height: 100%;
}
</style>
