<script setup>
import { ref } from 'vue';
// Importem el component pont que conté el <canvas> de Phaser
import PhaserCanvas from './components/PhaserCanvas.vue';

// Variable reactiva que guarda el nom escrit a la caixa de text
const nom = ref('Anna');
// Booleà per muntar/desmuntar el joc i verificar onMounted / onUnmounted
const mostrarJoc = ref(true);

const color = ref('red');
const estado = ref('Disponible');
const isPaused = ref(false);
const phaserCanvasRef = ref(null);

function togglePause() {
  const game = phaserCanvasRef.value?.getGame?.();
  const escena = game?.scene.getScene('MainScene');

  if (!escena) return;

  if (isPaused.value) {
    escena.scene.resume();
  } else {
    escena.scene.pause();
  }

  isPaused.value = !isPaused.value;
}
</script>

<template>
  <main class="contenidor-app">
    <!-- Tota la interfície d'usuari (inputs, botons) es fa en HTML/CSS dins de Vue -->
    <div class="controls">
      <input v-model="nom" placeholder="Escriu el teu nom d'usuari..." />
      <button @click="mostrarJoc = !mostrarJoc">
        {{ mostrarJoc ? 'Amaga el llenç' : 'Mostra el llenç' }}
      </button>
      <input type="color" v-model="color" placeholder="Escriu el color..." />
      <button @click="color = 'red'">Change Color</button>
      <select v-model="estado">
        <option value="Disponible">Disponible</option>
        <option value="Ocupat">Ocupat</option>
        <option value="Absent">Absent</option>
      </select>
      <button @click="togglePause">
        {{ isPaused ? 'Reprèn' : 'Pausa' }}
      </button>
    </div>

    <!-- Passem 'nom' cap avall amb :nomUsuari i controlem el cicle amb v-if -->
    <PhaserCanvas ref="phaserCanvasRef" v-if="mostrarJoc" :nomUsuari="nom" :color="color" :estado="estado" />
  </main>
</template>
