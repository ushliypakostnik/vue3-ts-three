<template>
  <div id="scene" class="scene" />
</template>

<script lang="ts">
import { defineComponent, onMounted } from 'vue';

import * as THREE from 'three';
import {
  PerspectiveCamera,
  Scene,
  WebGLRenderer,
  DirectionalLight,
  SpotLight,
  Mesh,
} from 'three';
import { OrbitControls } from 'three/examples/jsm/controls/OrbitControls';

export default defineComponent({
  name: 'Scene',

  setup() {
    let init: () => void;
    let animate: () => void;
    let render: () => void;
    let onWindowResize: () => void;

    let container: HTMLElement;

    let camera: PerspectiveCamera = new THREE.PerspectiveCamera();
    let scene: Scene = new THREE.Scene();
    let renderer: WebGLRenderer = new THREE.WebGLRenderer({ antialias: true });

    let light1: DirectionalLight = new THREE.DirectionalLight(0xffffff);
    let light2: SpotLight = new THREE.SpotLight(0xffffff, 5, 1000);
    let light3: SpotLight = new THREE.SpotLight(0xffffff, 5, 1000);

    let cube: Mesh = new THREE.Mesh();
    let ground: Mesh = new THREE.Mesh();

    init = () => {
      // Core

      container = document.getElementById('scene') as HTMLElement;

      camera = new THREE.PerspectiveCamera(
        50,
        container.clientWidth / container.clientHeight,
        1,
        4000,
      );

      camera.position.x = 0;
      camera.position.y = 150;
      camera.position.z = 400;

      scene.background = new THREE.Color(0x050505);
      scene.fog = new THREE.Fog(0x050505, 50, 1000);

      renderer.setPixelRatio(window.devicePixelRatio);
      renderer.setSize(container.clientWidth, container.clientHeight);
      renderer.shadowMap.enabled = true;
      renderer.shadowMap.type = THREE.PCFSoftShadowMap;
      container.appendChild(renderer.domElement);
      scene.add(camera);

      // Lights
      light1.position.set(0.5, 1.0, 0.5).normalize();
      scene.add(light1);

      scene.add(new THREE.AmbientLight(0x222222));

      light2.position.set(200, 250, 500);
      light2.angle = 0.5;
      light2.penumbra = 0.5;
      light1.castShadow = true;
      light2.shadow.mapSize.width = 1024;
      light2.shadow.mapSize.height = 1024;
      scene.add(light2);

      light3.position.set(-100, 350, 350);
      light3.angle = 0.5;
      light3.penumbra = 0.5;
      light3.castShadow = true;
      light3.shadow.mapSize.width = 1024;
      light3.shadow.mapSize.height = 1024;
      scene.add(light3);

      scene.add(light3);

      // Cube
      let geometry = new THREE.BoxGeometry(100, 100, 100);
      let material = new THREE.MeshPhongMaterial();
      material.color.set(0x000000);
      cube = new THREE.Mesh(geometry, material);
      cube.position.y = 50;
      scene.add(cube);

      // Grass
      const gt = new THREE.TextureLoader().load('./img/grass.jpg');
      const gg = new THREE.PlaneBufferGeometry(4000, 4000);
      const gm = new THREE.MeshPhongMaterial({ color: 0xffaaaa, map: gt });
      const ground = new THREE.Mesh(gg, gm);
      ground.rotation.x = -Math.PI / 2;
      ground.material.map!.repeat.set(16, 16);
      ground.material.map!.wrapS = ground.material.map!.wrapT =
        THREE.RepeatWrapping;
      ground.material.map!.encoding = THREE.sRGBEncoding;
      ground.receiveShadow = true;
      scene.add(ground);

      // Controls
      const controls = new OrbitControls(camera, renderer.domElement);
      controls.addEventListener('change', render);
      controls.update();

      // Resize
      window.addEventListener('resize', onWindowResize, false);
    };

    animate = () => {
      requestAnimationFrame(animate);
      render();
    };

    onWindowResize = () => {
      camera.aspect = window.innerWidth / window.innerHeight;
      camera.updateProjectionMatrix();
      renderer.setSize(window.innerWidth, window.innerHeight);
    };

    render = () => {
      renderer.render(scene, camera);
    };

    onMounted(() => {
      init();
      animate();
    });

    return {
      camera,
      scene,
      renderer,
      light1,
      light2,
      light3,
      init,
      animate,
      onWindowResize,
      render,
      cube,
      ground,
    };
  },
});
</script>

<style scoped lang="stylus">
// @import "~/src/stylus/_stylebase.styl";

.scene
  position absolute
  top 0
  left 0
  right 0
  bottom 0
  width 100vw
  height: 100vh
</style>
