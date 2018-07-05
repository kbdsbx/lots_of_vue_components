# Lots of vue components

```
npm install lots_of_vue_components
```

### AMap

```js
import { AMap } from 'lots_of_vue_components';
```

```html
<amap
  :meta=""
  :polygons=""
  :labels=""
  :dots=""
  :pins=""
  v-on:map-move=""
  v-on:click=""
  v-on:dblclick=""
  v-on:click-polygon=""
  v-on:dblclick-polygon=""
  v-on:change-polygon=""
  v-on:click-pin=""
  v-on:dblclick-pin=""
  v-on:change-pin=""
></amap>
```

+ meta : https://lbs.amap.com/api/javascript-api/reference/map
+ polygons : [ polygons,... ] as https://lbs.amap.com/api/javascript-api/reference/overlay#polygon
+ labels : todo
+ dots : todo
+ pins : 
```
{
    id,
    image : 'url or base64',
    lng,
    lat,
}
```

> You must set ``id`` in polygon which is item in polygons, etc.