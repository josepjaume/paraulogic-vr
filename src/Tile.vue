<template>
  <Group :position="position" ref="groupRef">
    <Cylinder
      :radial-segments="6"
      :radius-bottom="0.1"
      :radius-top="0.1"
      :height="0.02"
      :position="{ x: 0, y: -0.01, z: 0 }"
    >
      <StandardMaterial
        :color="color === 'BLUE' ? '#79bac8' : '#D4544E'"
        :props="{ roughness: 0.55, metalness: 0.3, }"
      />
    </Cylinder>
  </Group>
</template>

<script setup lang="ts">
import { Cylinder, ToonMaterial, PhongMaterial, PhysicalMaterial, StandardMaterial, Group, Texture } from 'troisjs'
import { Text } from 'troika-three-text'
import { ref, onMounted } from 'vue'
import { MeshPhysicalMaterial } from 'three'

let groupRef = ref()

interface Vector {
  x: number
  y: number
  z: number
}

interface Props {
  position?: Vector,
  letter: string,
  color?: "BLUE" | "RED"
}

const props = withDefaults(defineProps<Props>(), {
  position: () => ({ x: 0, y: 0, z: 0 }),
  color: "BLUE",
})

onMounted(() => {
  const myText = new Text()
  let group = groupRef.value.group
  group.add(myText)

  const letterMaterial = new MeshPhysicalMaterial({
    metalness: 1,
    roughness: 0.2,
    color: props.color === "BLUE" ? "#000000" : "#ffffff",
    emissive: props.color === "BLUE" ? "#000000" : "#ffffff",
    clearcoat: 0.5,
  })

  // Set properties to configure:
  myText.text = props.letter
  myText.anchorX = "center"
  myText.anchorY = "middle"
  myText.fontSize = 0.06
  myText.position.set(0, 0, 0)
  myText.depthOffset = -1
  myText.rotation.x = -Math.PI / 2
  myText.material = letterMaterial

  // Update the rendering:
  myText.sync()

})
</script>