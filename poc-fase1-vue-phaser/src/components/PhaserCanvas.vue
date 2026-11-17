<script setup>
// 1. Importem ref (referència al DOM) i els hooks del cicle de vida de Vue 3
import { ref, watch, onMounted, onUnmounted } from 'vue';
// 2. Importem el motor Phaser 4 i la nostra escena personalitzada
import Phaser from 'phaser';
import { MainScene } from '../game/MainScene';
const props = defineProps(['nomUsuari', 'color', 'estado']);

const contenidor = ref(null);
let game = null;

onMounted(() => {
  game = new Phaser.Game({
    type: Phaser.AUTO,
    width: 640,
    height: 400,
    backgroundColor: '#1e293b',
    parent: contenidor.value,
    scene: [MainScene]
  });

  game.events.once('ready', () => {
    game?.scene.getScene('MainScene')?.actualitzarNom(props.nomUsuari);
    game?.scene.getScene('MainScene')?.actualitzarColor(props.color);
    game?.scene.getScene('MainScene')?.actualitzarEstat(props.estado);
  });
});

watch(() => props.nomUsuari, (nouNom) => {
  const escena = game?.scene.getScene('MainScene');
  escena?.actualitzarNom(nouNom);
});

watch(() => props.color, (nouColor) => {
  const escena = game?.scene.getScene('MainScene');
  escena?.actualitzarColor(nouColor);
});

watch(() => props.estado, (nouEstat) => {
  const escena = game?.scene.getScene('MainScene');
  escena?.actualitzarEstat(nouEstat);
});

onUnmounted(() => game?.destroy(true));

defineExpose({
  getGame: () => game
});
</script>
<template><div ref="contenidor"></div></template>
