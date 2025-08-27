# 底图渲染

地图的加载要等到 DOM 元素加载完成后，所以加载部分的代码要写入 `onMounted` 内。`target` 内既可以填元素的 ID，也可以填引用的 `value`。`Map` 内最主要的是 `layers` 和 `view`。`layers` 是一个数组。`view` 中注意，ol 默认使用的是 Web Mercator 坐标，要使用 `fromLonLat([Longtitude, Latitude])` 把经纬度 XY 坐标转换成 Web Mercator 坐标，注意是先经度，再纬度。`layers` 可用 `TileLayer`，源可用 `XYZ` 且指定 `url`，或者源直接用 `new OSM()` 使用默认的 Open Street Map。

```vue
<script setup>
import { Map, View } from 'ol';
import TileLayer from 'ol/layer/Tile';
import { fromLonLat } from 'ol/proj';
import { OSM, XYZ } from 'ol/source';
import { onMounted, ref } from 'vue';

const mapContainer = ref(null);

onMounted(() => {
  const map = new Map({
    target: mapContainer.value,
    layers: [
      new TileLayer({
        source: new XYZ({
          // url: 'https://webst0{1-4}.is.autonavi.com/appmaptile?style=6&x={x}&y={y}&z={z}' // 栅格数据
          url: 'http://wprd0{1-4}.is.autonavi.com/appmaptile?lang=zh_cn&size=1&style=7&x={x}&y={y}&z={z}'
        })
      })
    ],
    view: new View({
      center: fromLonLat([108.8750, 34.1450]),
      zoom: 15
    })
  });
});
</script>

<template>
  <h2>MainPage</h2>
  <div ref="mapContainer" id="map"></div>
</template>

<style scoped>
#map {
  height: 600px;
  width: 800px;
}
</style>

```

# GeoJSON 矢量图层加载

有两种 GeoJSON 数据来源，一种是从文件加载，一种是从在线加载。核心都是使用 `VectorLayer` 和 `VectorSource` 并在 `source` 内指明数据类型

```vue
<script setup>
import { Map, View } from 'ol';
import { fromLonLat } from 'ol/proj';
import VectorSource from 'ol/source/Vector';
import { onMounted } from 'vue';
import GeoJSON from 'ol/format/GeoJSON';
import { hangzhou } from '@/assets/hangzhou';
import VectorLayer from 'ol/layer/Vector';
import Style from 'ol/style/Style';
import Stroke from 'ol/style/Stroke';
import Fill from 'ol/style/Fill';

// // 方式一 从文件加载 GeoJSON 数据
const vectorSource = new VectorSource({
  features: new GeoJSON().readFeatures(hangzhou)
});

// 方式二 加载在线 GeoJSON 数据
// const vectorSource = new VectorSource({
//   url: 'https://openlayers.org/data/vector/ecoregions.json',
//   format: new GeoJSON(),
// });

const vectorLayer = new VectorLayer({
  source: vectorSource,
  style: new Style({
    stroke: new Stroke({
      color: 'rgb(35, 176, 189)',
      width: 2
    }),
    fill: new Fill({
      color: 'rgba(255, 255, 0, 0.1)'
    })
  })
});

onMounted (() => {
  const map = new Map({
    target: 'map',
    view: new View({
      center: [0, 0],
      zoom: 2
    }),
    layers: [
      vectorLayer
    ]
  });
  map.getView().fit(vectorSource.getExtent(), { size: map.getSize() });
});
</script>

<template>
  <h2>Add Vector Map</h2>
  <div id="map"></div>
</template>

<style scoped>
#map {
  width: 800px;
  height: 600px;
}
</style>
```

