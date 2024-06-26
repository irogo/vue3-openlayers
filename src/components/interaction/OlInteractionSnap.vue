<template>
  <slot></slot>
</template>

<script setup lang="ts">
import type { Ref } from "vue";
import { inject, watch, onMounted, onUnmounted } from "vue";
import Snap from "ol/interaction/Snap";
import type Map from "ol/Map";
import type VectorSource from "ol/source/Vector";
import usePropsAsObjectProperties from "@/composables/usePropsAsObjectProperties";

const props = withDefaults(
  defineProps<{
    vertex?: boolean;
    edge?: boolean;
    pixelTolerance?: number;
  }>(),
  {
    vertex: true,
    edge: true,
    pixelTolerance: 10,
  },
);

const map = inject<Map>("map");
const source = inject<Ref<VectorSource> | null>("vectorSource");

const properties = usePropsAsObjectProperties(props);

const createSnap = () =>
  new Snap({
    ...properties,
    source: source?.value,
  });
let snap = createSnap();

watch(properties, () => {
  map?.removeInteraction(snap);
  snap = createSnap();
  map?.addInteraction(snap);
  map?.changed();
});

onMounted(() => {
  map?.addInteraction(snap);
});

onUnmounted(() => {
  map?.removeInteraction(snap);
});

defineExpose({
  snap,
});
</script>
