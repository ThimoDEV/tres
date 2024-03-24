<script setup lang="ts">
import { TresCanvas } from "@tresjs/core";
import {
  reactive,
  onMounted,
  ref,
  computed,
} from "vue";
import {
  BasicShadowMap,
  SRGBColorSpace,
  NoToneMapping,
} from "three";
import { OrbitControls } from "@tresjs/cientos";
import TheMeshes from "./TheMeshes.vue";

const gl = reactive({
  clearColor: "#82DBC5",
  shadows: true,
  alpha: false,
  shadowMapType: BasicShadowMap,
  outputColorSpace: SRGBColorSpace,
  toneMapping: NoToneMapping,
});

const canvas = ref(null);

const dummyComputed = computed(() => {
  if (!timeoutValue.value) return 2;
  return timeoutValue.value;
});

const timeoutValue = ref(null);
const units = ref([1, 2, 3]);

onMounted(() => {
  setTimeout(() => {
    timeoutValue.value = 3;
  }, 5000);
});
</script>

<template>
  <TresCanvas v-bind="gl" ref="canvas">
    <!--  Using computed values for camera position breaks everything: -->
    <!-- <TresPerspectiveCamera
      :position="[dummyComputed, dummyComputed, dummyComputed]"
    /> -->
    <TresPerspectiveCamera
      :position="[15, 15, 15]"
    />
    <OrbitControls
      :target="[
        dummyComputed,
        dummyComputed,
        dummyComputed,
      ]"
    />
    <TresAmbientLight
      :intensity="0.5"
      color="red"
    />
    <!-- When :dummyComputed is passed as props to the custom TheMeshes component, it 
          seems to trigger a rerender on the whole thing and reverts the translateX of the 
          Text3D mesh.

          What's weird is that if you wrap the translateX value in array brackets, the problem
          goes away. See inside TheMeshes for an example.
    -->
    <TheMeshes
      v-for="num in units"
      :dummy-computed="dummyComputed"
      :num="num"
    />
    <TresDirectionalLight
      :position="[0, 2, 4]"
      :intensity="1"
      cast-shadow
    />
    <TresAxesHelper />
    <TresGridHelper
      :args="[10, 10, 0x444444, 'teal']"
    />
  </TresCanvas>
</template>
