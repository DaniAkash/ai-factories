<template>
  <div class="deck-base" />
</template>

<style>
/* The canvas behind every slide, never a theme default. */
.deck-base {
  position: fixed;
  inset: 0;
  background: var(--surface);
  z-index: -1;
}
</style>
