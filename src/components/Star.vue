<script setup lang="ts">
import { ref } from "vue";
import { useGLTF } from "@tresjs/cientos";
import { useRenderLoop } from "@tresjs/core";
import { MeshStandardMaterial } from "three";

const yellowMaterial = new MeshStandardMaterial({
  color: 0xffff00, // 黄色
  metalness: 0.1,
  roughness: 0.8,
});

const { nodes } = await useGLTF(
  "https://raw.githubusercontent.com/wkhr-m/thanks-page/refs/heads/main/public/models/star.glb",
  { draco: true }
);

const model = nodes["star"];

const stars = Array.from({ length: 2000 }, () => ({
  position: [
    (Math.random() - 0.5) * 80,
    (Math.random() - 0.5) * 80,
    (Math.random() - 0.5) * 80,
  ],
  rotation: [
    (Math.random() - 0.5) * 10,
    (Math.random() - 0.5) * 10,
    (Math.random() - 0.5) * 10,
  ],
  scale: [1, 1, 2],
}));

const starsRef = ref(null);

const { onLoop } = useRenderLoop();

onLoop(({ delta }) => {
  if (starsRef.value) {
    for (const star of (starsRef.value as any).children) {
      star.rotation.x += delta * 0.2;
      star.rotation.y += delta * 0.2;
      star.rotation.z += delta * 0.2;

      if (star.position.y < -80) {
        star.position.y = 40;
      }
      if (star.position.x < -80) {
        star.position.x = 40;
      }
      star.position.y -= delta * 0.2;
      star.position.x -= delta * 0.2;
    }
  }
});
</script>
<template>
  <TresGroup ref="starsRef">
    <primitive
      v-for="star in stars"
      :key="star"
      :position="star.position"
      :rotation="star.rotation"
      :scale="star.scale"
      :object="model.clone()"
      :material="yellowMaterial"
    />
  </TresGroup>
</template>
