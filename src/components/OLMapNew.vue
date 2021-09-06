<template>
  <div id="map"></div>
</template>

<script>
import { Map, View, Feature } from "ol";
import VectorLayer from "ol/layer/Vector";
import { Fill, Stroke, Style, Text } from "ol/style";
import mapJson from "../assets/100000_full.json";
import { Polygon, MultiPolygon, Point } from "ol/geom";
import { transform } from "ol/proj";
import TileLayer from "ol/layer/Tile";
import { OSM, Vector as VectorSource } from "ol/source";

//转换
var transformGeoJSON = require("coordtransform-cli");
var wgs84json = transformGeoJSON(mapJson, "gcj02towgs84");
//初始化深圳地图
let szFeatures = [];
let areaFeature = null;
wgs84json.features.forEach((feature) => {
  if (feature.geometry.type == "MultiPolygon") {
    areaFeature = new Feature({
      adcode: feature.properties.adcode,
      geometry: new MultiPolygon(feature.geometry.coordinates).transform(
        "EPSG:4326",
        "EPSG:3857"
      ),
      name: feature.properties.name,
    });
  } else if (feature.geometry.type == "Polygon") {
    areaFeature = new Feature({
      adcode: feature.properties.adcode,
      geometry: new Polygon(feature.geometry.coordinates).transform(
        "EPSG:4326",
        "EPSG:3857"
      ),
      name: feature.properties.name,
    });
  }
  szFeatures.push(areaFeature);
});

let selection = {};
const style = new Style({
  fill: new Fill({
    color: "rgba(255, 255, 255, 0.6)",
  }),
  stroke: new Stroke({
    color: "#319FD3",
    width: 1,
  }),
  text: new Text({
    font: "12px Calibri,sans-serif",
    fill: new Fill({
      color: "#000",
    }),
    stroke: new Stroke({
      color: "#fff",
      width: 3,
    }),
  }),
});

const selectedCountry = new Style({
  stroke: new Stroke({
    color: "rgba(200,20,20,0.8)",
    width: 2,
  }),
  fill: new Fill({
    color: "rgba(200,20,20,0.4)",
  }),
});

const vectorLayer = new VectorLayer({
  source: new VectorSource({
    features: szFeatures,
  }),
  style: (feature) => {
    style.getText().setText(feature.get("name"));
    return style;
  },
});

export default {
  data() {
    return {
      map: null,
      selectionLayer: null,
    };
  },
  methods: {
    initMap() {
      this.map = new Map({
        layers: [
          new TileLayer({
            source: new OSM(),
          }),
          vectorLayer,
        ],
        target: "map",
        view: new View({
          center: transform([114.085947, 22.547], "EPSG:4326", "EPSG:3857"),
          zoom: 10,
        }),
      });
      this.map.on("click", this.onMapClick);

      this.selectionLayer = new VectorLayer({
        map: this.map,
        renderMode: "vector",
        source: vectorLayer.getSource(),
        style: function (feature) {
          if (feature.values_.adcode in selection) {
            return selectedCountry;
          }
        },
      });
    },
    onMapClick(event) {
      vectorLayer.getFeatures(event.pixel).then((features) => {
        if (!features.length) {
          selection = {};
          this.selectionLayer.changed();
          return;
        }
        const feature = features[0];
        if (!feature) {
          return;
        }
        selection = {};
        selection[feature.values_.adcode] = feature;
        this.selectionLayer.changed();
        console.log(selection);
      });
    },
  },
  mounted() {
    this.initMap();
  },
};
</script>

<style>
#map {
  height: 100vh;
  width: 100vw;
}
</style>