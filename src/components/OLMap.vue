<template>
  <div id="map">
    <div class="location" id="location"></div>
  </div>
</template>

<script>
import "ol/ol.css";
import { Map, View, Feature } from "ol";
import TileLayer from "ol/layer/Tile";
import VectorLayer from "ol/layer/Vector";
import { OSM, Vector as VectorSource } from "ol/source";
import { Polygon, MultiPolygon, Point, Geometry } from "ol/geom";
import chinaMap from "../assets/中华人民共和国.json";
import MousePositionControl from "ol/control/MousePosition";
import { createStringXY } from "ol/coordinate";
import { transformExtent, transform } from "ol/proj";
import { Style, Stroke, Fill } from "ol/style";
import TextStyle from "ol/style/Text";

let areaFeatureStyle = new Style({
  fill: new Fill({ color: "#4e98f444" }),
  stroke: new Stroke({
    width: 3,
    color: [71, 137, 227, 1],
  }),
});

let selectedStyle = new Style({
  fill: new Fill({ color: "#4e98f444" }),
  stroke: new Stroke({
    width: 3,
    color: "gary",
  }),
});

let szLayer = new VectorLayer({
  source: new VectorSource({
    features: [],
  }),
});

export default {
  data() {
    return {
      map: null,
      zoom: 10,
    };
  },
  methods: {
    initMap() {
      var mousePositionControl = new MousePositionControl({
        coordinateFormat: createStringXY(4),
        projection: "ESPG:4326",
        className: "custom-mouse-position",
        target: "location",
        undefinedHTML: "&nbsp;",
      });

      this.map = new Map({
        target: "map",
        layers: [
          new TileLayer({
            source: new OSM(),
          }),
        ],
        view: new View({
          center: transform([114.085947, 22.547], "EPSG:4326", "EPSG:3857"),
          zoom: 10,
          // minZoom: 20.5,
          //   extends: transformExtent(this.extent, "EPSG:4326", "EPSG:3857"),
        }),
      });
      this.map.addControl(mousePositionControl);
      // var _this = this;
      // this.map.on("moveend", function (e) {
      //   _this.zoom = e.map.getView().getZoom();
      // });
    },
    /**
     * 设置区域
     */
    addArea(geoFeatures = []) {
      if (geoFeatures.length == 0) return false;

      //转换
      var transformGeoJSON = require("coordtransform-cli");
      var wgs84json = transformGeoJSON(chinaMap, "gcj02towgs84");

      let areaFeatures = [];
      let areaFeature = null;

     
      // 添加图层
      this.map.addLayer(this.szLayer);
      wgs84json.features.forEach((feature) => {
        this.addCityName(feature);
        if (feature.geometry.type == "MultiPolygon") {
          areaFeature = new Feature({
            geometry: new MultiPolygon(feature.geometry.coordinates).transform(
              "EPSG:4326",
              "EPSG:3857"
            ),
          });
        } else if (feature.geometry.type == "Polygon") {
          areaFeature = new Feature({
            geometry: new Polygon(feature.geometry.coordinates).transform(
              "EPSG:4326",
              "EPSG:3857"
            ),
          });
        }

        areaFeature.setStyle(areaFeatureStyle);
        areaFeatures.push(areaFeature);
      });

      this.szLayer.getSource().addFeatures(areaFeatures);
      this.map.on(["click"], this.onMapVectorClick);
    },
    addCityName(feature) {
      let mark = new Feature({
        type: "icon",
        geometry: new Point(feature.properties.center).transform(
          "EPSG:4326",
          "EPSG:3857"
        ),
      });
      this.map.addLayer(
        new VectorLayer({
          source: new VectorSource({
            features: [mark],
          }),
          style: new Style({
            text: new TextStyle({
              text: feature.properties.name,
              font: `12px font-size`, // 设置字体大小
              fill: new Fill({
                // 设置字体颜色
                color: "#1CAF9A",
              }),
            }),
          }),
        })
      );
    },
    onMapVectorClick(event) {
      szLayer.getFeatures(event.pixel).then((feature) => {
        
      });
    },
  },
  mounted() {
    this.initMap(); //初始化地图方法
    this.addArea(chinaMap); //添加区域图层方法
  },
};
</script>

<style>
#map {
  height: 100vh;
  width: 100vw;
}
</style>