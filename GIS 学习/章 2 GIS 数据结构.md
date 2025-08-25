# GIS 数据介绍及分类

* TIFF 数据：也称影像或遥感影像，类似于「拍摄的照片」，但比 jpg 这些数据多了经纬信息
* 矢量数据
* 栅格数据

## 栅格数据

一切的图像都是栅格数据

## 矢量数据

来源于测绘或由栅格数据得出，主要有 shape 和 geojson 两种格式

### Shape 格式文件

* `.shp` 图形文件
* `.dbf` 数据库文件
* `.prj` 坐标系信息文件
* `.shx` 索引文件
* `.sbn` 或 `.sbx` 空间索引文件（非必要）

### GeoJSON

```json
{
	"type": "Feature",
	"properties": {"name": "杭州市"},
	"geomerty": {"type": "Point", "coordinates": [120.3422342, 29.23423]}
}
```

`type` 字段中的 `Feature` 表示「要素」，要素即一个点、一条线、一个面等。该字段一般是固定的

`geometry` 中 `type` 的类型有：

* `Point`
* `MultiPoint`
* `LineString`
* `MultiLineString`
* `Polygon`
* `MultiPolygon`

要素集合

```json
{
	"type": "FeatureCollection",
	"features": [
		{
			"type": "Feature",
			"properties": {"name": "宁波市"},
			"geometry": {
				"type": "Point",
				"coordinates": [120, 29]
			}
		},
		{
			"type": "Feature",
			"properties": {"name": 杭州市},
			"geometry": {
				"type": "Point",
				"coordinates": [120, 29]
			}
		}
	]
}
```


### 其他的格式

`kml`、`gml` 都使用 `xml`，但不够便捷慢慢淘汰了

`wkt` 无法记录属性信息

## 三维数据

* 倾斜摄影：软件合成航天飞机拍的上、前、后、左、右五个面的照片生成
* 手工模型：通常为 `.gltf` 或 `.glb`
* 建筑信息模型：来自专业建模软件，其数据可以携带经纬度信息
* 点去数据：通过激光雷达取得的数据



