<script setup lang="ts">
defineProps<{ msg: string }>()
</script>

<template>
  <h1 color="$ep-color-primary">
    {{ msg }}
  </h1>

  <em>Click the logo to start operation</em>
  <!-- <p>
    Oracle Operation
    <a href="/nav/1">Click here</a>
  </p>
  <p>
    PostgreSQL Operation
    <a href="/nav/2">Click here</a>
  </p>
  <p>
    MongoDB Operation
    <a href="/nav/3">Click here</a>
  </p> -->
</template>

<style>
.ep-button {
  margin: 4px;
}

.ep-button+.ep-button {
  margin-left: 0;
  margin: 4px;
}
</style>
