<template>
  <div ref="container" class="solar-system-container"></div>
</template>

<script setup>
import { onMounted, onUnmounted, ref } from 'vue';
import * as THREE from 'three';
import { OrbitControls } from 'three/examples/jsm/controls/OrbitControls';
import { CSS2DRenderer, CSS2DObject } from 'three/examples/jsm/renderers/CSS2DRenderer';

const container = ref(null);
let animationId = null;

onMounted(() => {
  if (!container.value) return;

  // Scene setup
  const scene = new THREE.Scene();
  scene.background = null; // Transparent background
  
  const width = container.value.clientWidth;
  const height = container.value.clientHeight;
  
  const camera = new THREE.PerspectiveCamera(45, width / height, 0.1, 5000);
  camera.position.set(350, 350, 550); // Moved closer for a more zoomed-in feel

  const renderer = new THREE.WebGLRenderer({ antialias: true, alpha: true });
  renderer.setSize(width, height);
  renderer.setPixelRatio(window.devicePixelRatio);
  container.value.appendChild(renderer.domElement);

  const labelRenderer = new CSS2DRenderer();
  labelRenderer.setSize(width, height);
  labelRenderer.domElement.style.position = 'absolute';
  labelRenderer.domElement.style.top = '0px';
  labelRenderer.domElement.style.pointerEvents = 'none';
  container.value.appendChild(labelRenderer.domElement);

  const controls = new OrbitControls(camera, renderer.domElement);
  controls.enableDamping = true;
  controls.dampingFactor = 0.05;
  controls.enableZoom = false; // Zoom disabled

  // Lighting
  const ambientLight = new THREE.AmbientLight(0x404040, 2);
  scene.add(ambientLight);

  const sunLight = new THREE.PointLight(0xffffff, 4, 1500);
  scene.add(sunLight);

  // Sun
  const sunGeometry = new THREE.SphereGeometry(40, 64, 64);
  const sunMaterial = new THREE.MeshBasicMaterial({
    color: 0x129347, // Brand Green
    transparent: true,
    opacity: 1
  });
  const sun = new THREE.Mesh(sunGeometry, sunMaterial);
  scene.add(sun);

  // Sun Label
  const sunDiv = document.createElement('div');
  sunDiv.className = 'planet-label';
  sunDiv.textContent = 'SPAYMENT';
  sunDiv.style.color = '#129347';
  sunDiv.style.fontWeight = 'bold';
  sunDiv.style.fontSize = '16px'; 
  sunDiv.style.letterSpacing = '2px';
  const sunLabel = new CSS2DObject(sunDiv);
  sunLabel.position.set(0, 54, 0); // Increased 4px from 50 to 54
  sun.add(sunLabel);

  // Sun Glow
  const sunGlowGeometry = new THREE.SphereGeometry(50, 64, 64);
  const sunGlowMaterial = new THREE.MeshBasicMaterial({
    color: 0x129347,
    transparent: true,
    opacity: 0.15,
    side: THREE.BackSide
  });
  const sunGlow = new THREE.Mesh(sunGlowGeometry, sunGlowMaterial);
  scene.add(sunGlow);

  // Bank Data with Icons - Further Enlarged Sizes
  const planetsData = [
    { name: 'MB BANK', icon: '/icon/mbbank-icon.webp', size: 30, distance: 90, speed: 0.047 },
    { name: 'TECHCOMBANK', icon: '/icon/techcombank-icon.webp', size: 35, distance: 150, speed: 0.035 },
    { name: 'VIETCOMBANK', icon: '/icon/vietcombank-icon.webp', size: 32, distance: 210, speed: 0.029 },
    { name: 'BIDV', icon: '/icon/bidv-icon.webp', size: 30, distance: 280, speed: 0.024 },
    { name: 'ACB', icon: '/icon/acb-icon.webp', size: 30, distance: 380, speed: 0.013 },
    { name: 'TPBANK', icon: '/icon/tpbank-icon.webp', size: 28, distance: 490, speed: 0.009 },
    { name: 'VPBANK', icon: '/icon/vpbank-icon.webp', size: 30, distance: 600, speed: 0.007 },
    { name: 'AGRIBANK', icon: '/icon/agribank-icon.webp', size: 30, distance: 720, speed: 0.005 }
  ];

  const textureLoader = new THREE.TextureLoader();
  const planetMeshes = [];

  planetsData.forEach(p => {
    // Orbit Line
    const orbitCurve = new THREE.EllipseCurve(0, 0, p.distance, p.distance * 0.8);
    const orbitPoints = orbitCurve.getPoints(200);
    const orbitGeometry = new THREE.BufferGeometry().setFromPoints(orbitPoints);
    const orbitMaterial = new THREE.LineBasicMaterial({ color: 0x000000, transparent: true, opacity: 0.1 });
    const orbitLine = new THREE.Line(orbitGeometry, orbitMaterial);
    orbitLine.rotation.x = Math.PI / 2;
    scene.add(orbitLine);

    // Bank Logo Mesh
    const texture = textureLoader.load(p.icon);
    const geometry = new THREE.PlaneGeometry(p.size, p.size);
    const material = new THREE.MeshBasicMaterial({ 
      map: texture, 
      transparent: true,
      side: THREE.DoubleSide
    });
    const mesh = new THREE.Mesh(geometry, material);
    
    const angle = Math.random() * Math.PI * 2;
    mesh.position.set(
      Math.cos(angle) * p.distance,
      0,
      Math.sin(angle) * p.distance * 0.8
    );
    scene.add(mesh);

    planetMeshes.push({ mesh, data: p, angle });
  });

  const simulationSpeed = 5;
  const clock = new THREE.Clock();

  function animate() {
    animationId = requestAnimationFrame(animate);

    const delta = clock.getDelta();

    planetMeshes.forEach(pm => {
      pm.angle += pm.data.speed * delta * simulationSpeed;
      // Precise mathematical positioning on the ellipse
      pm.mesh.position.set(
        Math.cos(pm.angle) * pm.data.distance,
        0,
        Math.sin(pm.angle) * pm.data.distance * 0.8 // Standard parameterization matches rotated plane
      );
      pm.mesh.quaternion.copy(camera.quaternion);
    });

    const time = clock.getElapsedTime();
    sunGlow.scale.setScalar(1 + Math.sin(time * 2) * 0.05);

    controls.update();
    renderer.render(scene, camera);
    labelRenderer.render(scene, camera);
  }

  animate();

  const handleResize = () => {
    if (!container.value) return;
    const w = container.value.clientWidth;
    const h = container.value.clientHeight;
    camera.aspect = w / h;
    camera.updateProjectionMatrix();
    renderer.setSize(w, h);
    labelRenderer.setSize(w, h);
  };

  window.addEventListener('resize', handleResize);

  onUnmounted(() => {
    cancelAnimationFrame(animationId);
    window.removeEventListener('resize', handleResize);
    renderer.dispose();
  });
});
</script>

<style scoped>
.solar-system-container {
  width: 100%;
  height: 100%;
  position: relative;
  overflow: hidden;
  cursor: grab;
}

.solar-system-container:active {
  cursor: grabbing;
}

.solar-system-container {
  width: 100%;
  height: 100%;
  position: relative;
  overflow: hidden;
  cursor: grab;
}

.solar-system-container:active {
  cursor: grabbing;
}

/* Only target the actual renderers, not the labels */
.solar-system-container > :deep(canvas),
.solar-system-container > :deep(div) {
  width: 100% !important;
  height: 100% !important;
}

:deep(.planet-label) {
  width: auto !important;
  height: auto !important;
  pointer-events: none;
  font-size: 11px;
  text-transform: uppercase;
  letter-spacing: 2px;
  white-space: nowrap;
  font-family: 'Inter', sans-serif;
  color: #333;
  font-weight: 600;
  text-shadow: 0 0 10px rgba(255, 255, 255, 0.8);
  padding: 4px 8px;
  background: rgba(255, 255, 255, 0.1);
  border-radius: 4px;
}
</style>
