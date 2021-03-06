# osm2geojson

[![Build Status](https://travis-ci.org/rclark/osm2geojson.svg?branch=master)](https://travis-ci.org/rclark/osm2geojson)

Streams OSM data in any format to GeoJSON

### Install it globally
```
npm install -g osm2geojson
```

### Use it from the command line
```
Usage: osm2geojson [options] <filepath>

Options:
  -w, --warn	On invalid data, log warnings to stderr instead of failing
  -n, --nodes	Create GeoJSON for nodes which are not POIs
```

### Use it in another Node.js module

```js
var filepath = '/path/to/osm/data';
var osm2geojson = require('osm2geojson')(filepath);
osm2geojson.pipe(aWritableStream);
```

### Optional warnings

By default, the stream will fail hard and emit an error event if it encounters
bad data in the file which cannot be converted to GeoJSON. If you wish to ignore
these failures, you can specify the `--warn` option at the command-line, or
provide the `failEvents` option in JavaScript. The stream will then emit `fail`
events, and listeners will be handed an error object that you can inspect.

```js
var filepath = '/path/to/osm/data';
var osm2geojson = require('osm2geojson')(filepath, { failEvents: true });

osm2geojson.on('fail', function(failure) {
  console.log(failure.message);
  // way!8914650!3 | location of at least one of the nodes in this way not set
});

osm2geojson.pipe(aWritableStream);
```

### Want every node?

By default, nodes that are not points-of-interest (POIs) will not be included in
the output. A POI is defined as a node that has tags other than `source` and
`created_by`. If you wish to include every node, you can use the `--node` flag
at the command-line, or provide the `allNodes` option in JavaScript.
