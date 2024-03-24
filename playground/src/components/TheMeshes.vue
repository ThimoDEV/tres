<script setup>
import { defineProps, toRefs, ref } from "vue";
import { Text3D } from "@tresjs/cientos";

const props = defineProps({
  dummyComputed: {
    type: Number,
    default: 0,
  },
  num: {
    type: Number,
    default: 0,
  },
});

const fontPath =
  "https://raw.githubusercontent.com/Tresjs/assets/main/fonts/FiraCodeRegular.json";

const { dummyComputed, num } = toRefs(props);
</script>

<template>
  <TresGroup>
    <TresMesh
      ref="boxRef"
      :position="[num, num, 0]"
    >
      <TresBoxGeometry :args="[1, 1, 1]" />
      <TresMeshNormalMaterial />
    </TresMesh>
    <Suspense>
      <!-- This will revert the translateX to 0 when the dummyComputed changes with the timeout -->
      <Text3D
        :font="fontPath"
        text="my 3d text"
        :size="0.8"
        :position="[num, num, num]"
        :translate-x="dummyComputed"
      >
        <!-- This will not revert the translateX if it's wrapped in brackets: -->
        <!-- <Text3D
        :font="fontPath"
        :text="'my 3d text'"
        :size="0.8"
        :position="[num, num, num]"
        :translateX="[5]"
      > -->
        <TresMeshBasicMaterial color="red" />
      </Text3D>
    </Suspense>
  </TresGroup>
</template>
