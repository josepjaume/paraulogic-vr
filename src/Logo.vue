<template>
  <Group ref="groupRef" :position="{ x: -0.4, y: 2, z: -0.3 }"></Group>
</template>

<script setup lang="ts">
import { ref, onMounted } from 'vue'
import { SVGLoader } from "three/examples/jsm/loaders/SVGLoader.js"
import { DoubleSide, Group as ThreeGroup, Mesh as ThreeMesh, MeshBasicMaterial, ShapeGeometry } from 'three';
import { Group } from 'troisjs'

let groupRef = ref()
const loader = new SVGLoader();

onMounted(() => {
  loader.load("../logotip.svg",
    data => {

      const paths = data.paths;
      const group = new ThreeGroup();

      group.scale.multiplyScalar(0.001);
      group.scale.y *= - 1;

      for (let i = 0; i < paths.length; i++) {

        const path = paths[i];

        const material = new MeshBasicMaterial({
          color: path.color,
          side: DoubleSide,
          depthWrite: false
        });

        const shapes = SVGLoader.createShapes(path);

        for (let j = 0; j < shapes.length; j++) {
          const shape = shapes[j];
          const geometry = new ShapeGeometry(shape);
          const mesh = new ThreeMesh(geometry, material);
          group.add(mesh);
        }

      }

      groupRef.value.group.add(group);
    }
  )
})

</script>