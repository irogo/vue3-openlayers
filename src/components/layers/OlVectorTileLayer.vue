<template>
  <div>
    <slot></slot>
  </div>
</template>

<script setup lang="ts">
import { provide } from "vue";
import VectorTileLayer, {
  type VectorTileRenderType,
} from "ol/layer/VectorTile";
import useLayer from "@/composables/useLayer";
import {
  layersCommonDefaultProps,
  type LayersCommonProps,
} from "@/components/layers/LayersCommonProps";
import type { StyleLike } from "ol/style/Style";
import type { FlatStyleLike } from "ol/style/flat";

const props = withDefaults(
  defineProps<
    LayersCommonProps & {
      renderBuffer?: number;
      updateWhileAnimating?: boolean;
      styles?: StyleLike | FlatStyleLike | null;
      updateWhileInteracting?: boolean;
      renderMode?: VectorTileRenderType;
    }
  >(),
  {
    ...layersCommonDefaultProps,
    renderBuffer: 100,
    updateWhileAnimating: false,
    updateWhileInteracting: false,
    renderMode: "hybrid",
  },
);

const { layer } = useLayer(VectorTileLayer, props);

provide("vectorTileLayer", layer);
provide("stylable", layer);

defineExpose({
  vectorTileLayer: layer,
});
</script>
