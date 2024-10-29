<script setup>
import * as THREE from "three";
import { OrbitControls } from "three/examples/jsm/Addons.js";

const canvasContainer = ref(null);

// function initiateThree() {

// }

onMounted(() => {
  // Set up the scene, camera, and renderer
  const scene = new THREE.Scene();
  scene.background = new THREE.Color(0.5, 0.5, 0.5);

  const sizes = {
    width: window.innerWidth,
    height: window.innerHeight,
  };

  window.addEventListener("resize", () => {

    console.log('runnn')
    // Update sizes
    sizes.width = window.innerWidth;
    sizes.height = window.innerHeight;

    // Update camera
    camera.aspect = sizes.width / sizes.height;
    camera.updateProjectionMatrix();

    // Update renderer
    renderer.setSize(sizes.width, sizes.height);
    renderer.setPixelRatio(Math.min(window.devicePixelRatio, 2));
  });

  // camera setting
  const camera = new THREE.PerspectiveCamera(
    75,
    sizes.width / sizes.height,
    0.1,
    1000
  );
  camera.position.z = 15;

  // lights
  const ambientLight = new THREE.AmbientLight(0xffffff, 0.05);
  scene.add(ambientLight);

  const pointLight = new THREE.PointLight(0xffffff, 50, 10000);
  pointLight.position.set(6, 0, 5);
  scene.add(pointLight);

  // cube making
  const geometry = new THREE.SphereGeometry(5, 32, 32);
  const texture1 = new THREE.TextureLoader().load("/marbel.jpg");
  const texture2 = new THREE.TextureLoader().load("/blw.jpg");

  // now color the cube material
  const material = new THREE.MeshStandardMaterial({
    color: "white",
    map: texture1,
    bumpMap: texture2,
    displacementMap: texture2,
    displacementScale: 0.3,
    roughness: 0.6,
    metalness: 0.5,
  });

  // now takes  geometry, and applies  material to it
  const cube = new THREE.Mesh(geometry, material);

  // insert the cube to the scene
  scene.add(cube);

  // light source making
  const lightGeometry = new THREE.SphereGeometry(1, 32, 16);
  // now color the cube material
  const lightMaterial = new THREE.MeshBasicMaterial({ color: "white" });

  // now takes  geometry, and applies  material to it
  const lightSphere = new THREE.Mesh(lightGeometry, lightMaterial);
  lightSphere.position.set(6, 0, 5);
  scene.add(lightSphere);

  const renderer = new THREE.WebGLRenderer();

  console.log(renderer, window.innerWidth, window.innerHeight);

  renderer.setSize(window.innerWidth, window.innerHeight);
  canvasContainer.value.appendChild(renderer.domElement);
  const controls = new OrbitControls(camera, renderer.domElement);

  let q = 0;
  animate();
  function animate() {
    controls.update();
    //   q+=0.01;
    //   let qSin = Math.sin(q);
    //   let qCos = Math.cos(q);

    //     cube.position.x = 3*qSin

    // let scaleCos = 30 * qCos
    //     pointLight.position.set(scaleCos, 0 , 20 * qSin);
    //     lightSphere.position.set(scaleCos, 0 , 20 * qSin);
    //     cube.rotation.x += 0.01;
    //     cube.rotation.y += 0.01;
    //     cube.rotation.z += 0.01;

    renderer.render(scene, camera);
    requestAnimationFrame(animate);
  }
});
</script>

<template>
  <div ref="canvasContainer" class="canvas-container"></div>
</template>

<style scoped>
.canvas-container {
  width: 100vw;
  height: 100vh;
  overflow: hidden;
}
</style>
