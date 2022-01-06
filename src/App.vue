<template>
  <Renderer
    ref="rendererC"
    antialias
    :orbit-ctrl="{ enableDamping: true, target }"
    resize="window"
    :xr="true"
  >
    <Camera :position="{ x: 0, y: 1.5, z: 1 }" ref="cameraC" />
    <Scene background="#ffffff" ref="sceneC">
      <PointLight :position="{ x: 0, y: 0.5, z: -1 }" :intensity="1.5" />
      <PointLight :position="{ x: 0, y: 2, z: 1 }" :intensity="1.5" />
      <Cylinder :position="{ x: 0, y: 1.3, z: -0.4 }" />
      <GltfModel
        src="../models/paraulogic.gltf"
        :position="{ x: 0, y: 1.3, z: -0.4 }"
        :rotation="{ x: -0.5 }"
        ref="paraulogicC"
        cast-shadow
      />
    </Scene>
    <VRButton ref="vrbuttonC" />
  </Renderer>
</template>

<script setup lang="ts">
import { RepeatWrapping, GridHelper } from 'three';
import { ref, onMounted } from 'vue'
import { Box, Camera, LambertMaterial, MeshPublicInterface, PointLight, Renderer, RendererPublicInterface, Scene, GltfModel, Texture, StandardMaterial, Plane, Cylinder } from 'troisjs'
import VRButton from 'troisjs/src/components/misc/VRButton.vue'
import { XRControllerModelFactory } from 'three/examples/jsm/webxr/XRControllerModelFactory.js';
import { Camera as CameraInterface, Vector3 } from 'three';

const vrbuttonC = ref()
const rendererC = ref<RendererPublicInterface>()
const sceneC = ref()
const paraulogicC = ref()
const cameraC = ref()

let target = new Vector3(0, 1.4, 0)

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
