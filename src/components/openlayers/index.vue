<!--
 * @Description: 
 * @version: 1.0.0
 * @Author: changhaotian6@163.com
 * @Date: 2022-06-19 11:25:13
 * @LastEditors: changhaotian6@163.com
 * @LastEditTime: 2022-06-19 21:27:28
 * @FilePath: \openlayers-demo\src\components\openlayers\index.vue
-->
<template>
    <div>
        <div class="v-ol-container" ref="openlayerRef"></div>
        <div class="v-ol-layers">
            <span class="icon iconfont3d icon-3d-weixing"></span>
            <ul class="v-ol-layers-list">
                <li
                    class="v-ol-layer-item"
                    v-for="(item, index) in layers"
                    :key="item.url"
                    @click="handleChangeTileLayer(index)"
                >
                    {{ item.title }}
                </li>
            </ul>
        </div>
    </div>
</template>

<script>
import "ol/ol.css";
import Map from "ol/Map";
import OSM from "ol/source/OSM";
import TileLayer from "ol/layer/Tile";
import View from "ol/View";
import Tile from "ol/Tile";
import XYZ from "ol/source/XYZ";
import Style from 'ol/style/Style';
import Icon from 'ol/style/Icon';
import VectorSource from 'ol/source/Vector';
import VectorLayer from 'ol/layer/Vector';
import Feature from 'ol/Feature';
import Point from 'ol/geom/Point';

export default {
    name: "olMap",
    data() {
        return {
            layers: [
                {
                    title: "天地图路网",
                    url: "http://t4.tianditu.com/DataServer?T=vec_w&x={x}&y={y}&l={z}&tk=dea8e3abc0eae3c4427da994b6969ca8",
                    visible: true,
                },
                {
                    title: "天地图卫星影像",
                    url: "http://t3.tianditu.com/DataServer?T=img_w&x={x}&y={y}&l={z}&tk=dea8e3abc0eae3c4427da994b6969ca8",
                    visible: false,
                },
                {
                    title: "天地图文字标注",
                    url: "http://t3.tianditu.com/DataServer?T=cva_w&x={x}&y={y}&l={z}&tk=dea8e3abc0eae3c4427da994b6969ca8",
                    visible: false,
                },
                {
                    title: "高德普通地图",
                    url: "http://webrd03.is.autonavi.com/appmaptile?x={x}&y={y}&z={z}&lang=zh_cn&size=1&scale=1&style=8",
                    visible: false,
                },
                {
                    title: "高德混合地图",
                    url: "http://webst02.is.autonavi.com/appmaptile?style=6&x={x}&y={y}&z={z}",
                    visible: false,
                },
            ],
        };
    },
    mounted() {
        this.initMap();
    },
    methods: {
        initMap() {
            this.map = new Map({
                layers: [],
                target: this.$refs.openlayerRef,
                view: new View({
                    center: [0, 0], //地图中心点位置
                    zoom: 2,
                }),
            });

            this.addLayers(this.map);
            this.addFeature();
        },
        addLayers(map) {
            this.layers.map((item) => {
                const layer = new TileLayer({
                    source: new XYZ({
                        url: item.url,
                    }),
                    visible: item.visible,
                });

                map.addLayer(layer);
            });
        },

        handleChangeTileLayer(index) {
            this.map.getLayers().forEach((item, i) => {
                item.setVisible(index === i);
            });
        },

        addFeature() {
            // 
            let vectorSource = new VectorSource();
            let vectorLayer = new VectorLayer({
                source: vectorSource,
            });
            // 设置标注样式
            let labelStyle = new Style({
                image: new Icon({
                    src: "https://img.alicdn.com/imgextra/i3/O1CN01Mn65HV1FfSEzR6DKv_!!6000000000514-55-tps-228-59.svg",
                    scale: 0.5,
                }),
            });
            // 用于充当标注的要素
            let labelFeature = new Feature({
                geometry: new Point([12956325, 4851028]),
            });
            // 添加标注的样式
            labelFeature.setStyle(labelStyle);


            // 将标注要素添加到矢量图层中
            vectorSource.addFeature(labelFeature);
            // let map = new ol.Map({
            //     target: "map",
            //     layers: [
            //         new ol.layer.Tile({
            //             source: new ol.source.OSM(),
            //         }),
            //         vectorLayer,
            //     ],
            // });

            this.map.addLayer(vectorLayer)
        },
    },
};
</script>

<style lang="scss" scoped>
.v-ol-container {
    width: 100vw;
    height: 100vh;
}

.v-ol-layers {
    position: absolute;
    right: 20px;
    bottom: 20px;
    .icon {
        padding: 2px;
        font-size: 24px;
        color: #000;
        background-color: #fff;
        cursor: pointer;
        &:hover {
            background-color: #eee;
        }
    }
}
.v-ol-layers-list {
    position: absolute;
    right: calc(100% + 10px);
    bottom: 0;
    background-color: #fff;
}
.v-ol-layer-item {
    padding: 0 10px;
    width: auto;
    height: 40px;
    line-height: 40px;
    white-space: nowrap;
    font-size: 13px;
    cursor: pointer;
    &:hover {
        color: #409eff;
        background-color: #ecf5ff;
    }
}
</style>
