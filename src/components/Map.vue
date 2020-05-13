<template>
    <vl-map style="height: 90vh" @pointermove="showMeasureTooltip">
      <vl-view :zoom.sync="zoom" :center.sync="center"></vl-view>

      <vl-layer-vector :z-index="1">
        <vl-source-vector :features.sync="features" ident="the-source"></vl-source-vector>

        <vl-style-box>
          <vl-style-stroke color="green"></vl-style-stroke>
          <vl-style-fill color="rgba(255,255,255,0.5)"></vl-style-fill>
        </vl-style-box>
      </vl-layer-vector>

      <vl-interaction-draw
        type="Circle"
        source="the-source"
        :geometry-function="squareGeometryFunction()"
      >
        <vl-style-box>
          <vl-style-stroke color="blue"></vl-style-stroke>
          <vl-style-fill color="rgba(255,255,255,0.5)"></vl-style-fill>
        </vl-style-box>
      </vl-interaction-draw>
      <vl-interaction-draw
          type="LineString"
          source="the-source"
          :max-points="1"
          @drawstart="measureDrawnLine"
          @drawend="finishDrawing"
      >
        <vl-style-box>
          <vl-style-stroke color="blue"></vl-style-stroke>
          <vl-style-fill color="rgba(255,255,255,0.5)"></vl-style-fill>
        </vl-style-box>
      </vl-interaction-draw>

      <vl-layer-tile :z-index="0">
        <vl-source-osm></vl-source-osm>
      </vl-layer-tile>
      <MouseMeasureTooltip
      v-if="lineGeometry !== null && currentLength !== null"
      :text="currentLength"
      :positionX="mousePosistionX"
      :positionY="mousePosistionY"
    />
    </vl-map>
</template>

<script>
/*eslint-disable vue/no-unused-components*/
  import { createRegularPolygon } from 'ol/interaction/Draw';
  import { getLength } from 'ol/sphere';
  import MouseMeasureTooltip from './MouseMeasureTooltip';

  export default {
    data () {
      return {
        zoom: 2,
        center: [0, 0],
        features: [],
        currentLength: null,
        lineGeometry: null,
        mousePosistionX: null,
        mousePosistionY: null,
        square: null,
      }
    },
    components: {
      MouseMeasureTooltip
    },
    methods: {
      squareGeometryFunction() {
          if(!this.square){
              this.square = createRegularPolygon(4);
          }
          return this.square;
      },
      formatLength(line) {
        const length = getLength(line);
        return length > 100 ? `${Math.round(length / 1000 * 100) / 100} km`
          : `${Math.round(length * 100) / 100} m`;
      },
      measureDrawnLine(event) {
        this.lineGeometry = event.feature.getGeometry();
      },
      showMeasureTooltip(event) {
        if (this.lineGeometry !== null) {
          this.currentLength = this.formatLength(this.lineGeometry);
          this.mousePosistionX = event.pointerEvent.clientX + 15;
          this.mousePosistionY = event.pointerEvent.clientY;
        }
      },
      finishDrawing() {
        this.lineGeometry = null;
        this.currentLength = null;
      }
    }
  }
</script>
