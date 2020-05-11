<template>
  <vl-map style="height: 90vh">
    <vl-view :zoom.sync="zoom" :center.sync="center"></vl-view>

    <vl-layer-vector :z-index="1">
      <vl-source-vector :features.sync="features" ident="the-source"></vl-source-vector>

      <vl-style-box>
        <vl-style-stroke color="green"></vl-style-stroke>
        <vl-style-fill color="rgba(255,255,255,0.5)"></vl-style-fill>
      </vl-style-box>
    </vl-layer-vector>

    <vl-interaction-draw type="Circle" :geometry-function="squareStyleFunction()" source="the-source">
      <vl-style-box>
        <vl-style-stroke color="blue"></vl-style-stroke>
        <vl-style-fill color="rgba(255,255,255,0.5)"></vl-style-fill>
      </vl-style-box>
    </vl-interaction-draw>
    <vl-interaction-draw type="LineString" source="the-source" max-points="1">
      <vl-style-box>
        <vl-style-stroke color="blue"></vl-style-stroke>
        <vl-style-fill color="rgba(255,255,255,0.5)"></vl-style-fill>
      </vl-style-box>
    </vl-interaction-draw>

    <vl-layer-tile :z-index="0">
      <vl-source-osm></vl-source-osm>
    </vl-layer-tile>
  </vl-map>
</template>

<script>
  import { createRegularPolygon } from 'ol/interaction/Draw';

  export default {
    data () {
      return {
        zoom: 2,
        center: [0, 0],
        features: [],
      }
    },
    methods: {
      squareStyleFunction() {
        return createRegularPolygon(4);
      }
    }
  }
</script>