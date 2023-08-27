<template>
  <h1 class="heading">Pocket Reaction Timer</h1>
  <Game />
</template>

<script setup>
import Game from "./components/Game.vue";
</script>

<style>
*,
*::before,
*::after {
  margin: 0;
}
body {
  text-align: center;

  background: hsl(0, 0%, 10%);
  font-family: "Poppins", system-ui, -apple-system, BlinkMacSystemFont,
    "Segoe UI", Roboto, Oxygen, Ubuntu, Cantarell, "Open Sans", "Helvetica Neue",
    sans-serif;

  color: rgb(241, 241, 241);
}
#app {
  min-height: 100dvh;
  display: grid;
  align-content: center;
  gap: 2rem;
}
</style>
