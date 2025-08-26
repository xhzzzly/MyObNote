# 底图渲染

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

