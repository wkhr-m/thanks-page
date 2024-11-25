<script setup lang="ts">
import { TresCanvas } from "@tresjs/core";
import { BasicShadowMap, SRGBColorSpace, ACESFilmicToneMapping } from "three";
import { OrbitControls, Text3D } from "@tresjs/cientos";

import ShootingStars from "./components/Star.vue";

const gl = {
  clearColor: "#82DBC5",
  shadows: true,
  alpha: true,
  shadowMapType: BasicShadowMap,
  outputColorSpace: SRGBColorSpace,
  toneMapping: ACESFilmicToneMapping,
  windowSize: true,
};
const text = `Thank you!!!
右上のxを押して回答フォームを消してください。`;
</script>
<template>
  <TresCanvas v-bind="gl">
    <TresPerspectiveCamera :position="[8.48, 2.65, 17.94]" :far="1000" />
    <OrbitControls make-default :min-distance="60" :max-distance="100" />
    <Suspense>
      <ShootingStars />
    </Suspense>
    <Suspense>
      <Text3D
        font="/BIZ_UDPGothic_Regular.json"
        center
        :text="text"
        :size="1"
        :height="0.2"
        :curve-segments="12"
        :bevel-enabled="true"
        :bevel-thickness="0.05"
        :bevel-size="0.03"
        :bevel-offset="0"
        :bevel-segments="4"
      >
      </Text3D>
    </Suspense>
    <TresAmbientLight :intensity="2" />
    <TresDirectionalLight :position="[10, 10, 10]" :intensity="1" />
  </TresCanvas>
</template>

<style>
html,
body,
#app {
  margin: 0;
  padding: 0;
  width: 100%;
  height: 100%;
  overflow: hidden;
  background: #000;
}
</style>
