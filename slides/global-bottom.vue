<template>
  <div class="void-base" />
</template>

<style>
/* The canvas behind every slide is the void itself, never a theme default. */
.void-base {
  position: fixed;
  inset: 0;
  background: #000000;
  z-index: -1;
}
</style>
