# README

Minimal implementation of a javascript client for OGC API - tiles, using leaflet.js. The client is reading two vector tile layers from the [eMOTIONAL Cities catalogue](https://emotional.byteroad.net/catalogue), published using [pygeoapi](https://pygeoapi.io/) with a [elasticsearch provider](https://docs.pygeoapi.io/en/latest/data-publishing/ogcapi-tiles.html#mvt-elastic). 

Metadata elements such as `title` and `bounding box` are read from the [tileset metadata](https://docs.ogc.org/is/20-057/20-057.html#toc32) (see code sample bellow). 

```javascript
 (async () => {

        // collection metadata
        const metadata_cardio = await fetch(getCollection(pbfURL_cardio)).then(response => response.json());
        const metadata_pm10 = await fetch(getCollection(pbfURL_pm10)).then(response => response.json());

      // Retrieve title
        var overlayMaps = {
        [metadata_cardio.title]: mapPbfLayer_cardio,
        [metadata_pm10.title]: mapPbfLayer_pm10
        };

        // Retrieve bounding box
        const x = (metadata_cardio.extent.spatial.bbox[0][0] + metadata_cardio.extent.spatial.bbox[0][2])/ 2.0;
        const y = (metadata_cardio.extent.spatial.bbox[0][1] + metadata_cardio.extent.spatial.bbox[0][3])/ 2.0;

        map.flyTo({ lng: x, lat: y });

        var baseMaps = {
            "OpenStreetMap": OpenStreetMap_Mapnik,
            "Esri World Gray Canvas": World_Light_Gray_Base
        };

        var layerControl = L.control.layers(baseMaps, overlayMaps).addTo(map);

        let timeTaken = Date.now() - start;
        console.log("Total time taken : " + timeTaken + " milliseconds"); //20 milliseconds - 0.29%

    })();
```

There is also code for loading feature layers and benchmarking the application start using either tiles or features. You can read more about this experiment on [doublebyte.net](https://doublebyte.net).

![screenshot](/assets/screenshot.png)

Check this live at: [https://emotional-cities.github.io/vtiles-example/demo-oat.htm](https://emotional-cities.github.io/vtiles-example/demo-oat.htm)

## License

This project is released under an [MIT License](./LICENSE)

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
