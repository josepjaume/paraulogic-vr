<template>
  <Renderer
    ref="rendererC"
    antialias
    :orbit-ctrl="{ enableDamping: true, target }"
    resize="window"
    :xr="true"
  >
    <Scene background="#ffffff" ref="sceneC">
      <Logo />
      <Camera :position="{ x: 0, y: 1.5, z: 1 }" ref="cameraC" />
      <PointLight :position="{ x: 0, y: 2, z: 1 }" :intensity="0.5" />
      <AmbientLight :position="{ x: 0, y: 2, z: 0 }" :intensity="0.8" />

      <Group :position="{ x: 0, y: 1.3, z: -0.4 }" :rotation="{ x: Math.PI / 3 }">
        <Tile :position="{ x: 0, y: 0, z: 0 }" letter="N" color="RED" />
        <Tile :position="{ x: 0.2, y: 0, z: 0 }" letter="R" />
        <Tile :position="{ x: 0.099, y: 0, z: 0.175 }" letter="T" />
        <Tile :position="{ x: -0.099, y: 0, z: 0.175 }" letter="A" />
        <Tile :position="{ x: -0.2, y: 0, z: 0 }" letter="L" />
        <Tile :position="{ x: -0.099, y: 0, z: -0.175 }" letter="P" />
        <Tile :position="{ x: 0.099, y: 0, z: -0.175 }" letter="O" />
      </Group>
    </Scene>
    <VRButton ref="vrbuttonC" />
  </Renderer>
</template>

<script setup lang="ts">
import { GridHelper } from 'three';
import { ref, onMounted } from 'vue'
import { AmbientLight, Camera, PointLight, Renderer, RendererPublicInterface, Scene, GltfModel, Group } from 'troisjs'
import VRButton from 'troisjs/src/components/misc/VRButton.vue'
import { XRControllerModelFactory } from 'three/examples/jsm/webxr/XRControllerModelFactory.js';
import { Camera as CameraInterface, Vector3 } from 'three';
import Tile from "./Tile.vue"
import Logo from "./Logo.vue"

const vrbuttonC = ref()
const rendererC = ref<RendererPublicInterface>()
const sceneC = ref()
const paraulogicC = ref()
const cameraC = ref()
let target = new Vector3(0, 1.3, -0.4)

onMounted(() => {
  const renderer = rendererC.value!
  const vrbutton = vrbuttonC.value!
  const scene = sceneC.value.scene
  const camera = cameraC.value.camera as CameraInterface

  let controllerModelFactory = new XRControllerModelFactory()

  const controllerGrip1 = renderer.renderer.xr.getControllerGrip(0)
  const controllerGrip2 = renderer.renderer.xr.getControllerGrip(1)

  const model1 = controllerModelFactory
    .createControllerModel(controllerGrip1);

  const model2 = controllerModelFactory
    .createControllerModel(controllerGrip2);

  controllerGrip1.add(model1);
  controllerGrip2.add(model2);

  scene.add(controllerGrip1);
  scene.add(controllerGrip2);

  const size = 10;
  const divisions = 10;

  const gridHelper = new GridHelper(50, 100, "#ff0000", "#cccccc");
  scene.add(gridHelper);

  vrbutton.init(renderer.renderer)
}
)
</script>

<style>
body,
html {
  margin: 0;
}
canvas {
  display: block;
  background-color: #000;
}
</style>
