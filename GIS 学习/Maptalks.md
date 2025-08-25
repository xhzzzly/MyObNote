# 新建高德地图基图

```vue
<script setup>
import { onMounted, ref } from 'vue';
import * as maptalks from 'maptalks';

const mapContainer = ref(null);

onMounted(() => {
  const map = new maptalks.Map(mapContainer.value, {
    center: [108.8750, 34.1450],
    zoom: 15,
    baseLayer : new maptalks.TileLayer('base', {
      renderer : 'canvas', // set TileLayer's renderer to canvas
      crossOrigin : 'anonymous',
      urlTemplate: 'http://wprd{s}.is.autonavi.com/appmaptile?lang=zh_cn&size=1&style=7&x={x}&y={y}&z={z}',  // 矢量数据
      // urlTemplate: 'https://webst{s}.is.autonavi.com/appmaptile?style=6&x={x}&y={y}&z={z}',  // 栅格数据
      subdomains: ['01', '02', '03', '04']
    })
  });
});
</script>

<template>
<h1>Container:</h1>
<div ref="mapContainer" style="width:800px;height:600px;"></div>
</template>

<style scoped></style>
```



