<script setup>
import SearchBox from './components/SearchBox.vue';
import iconLoading from './components/icons/iconLoading.vue';

</script>

<template>
  <div class="center">
    <Suspense>
      <template #default>
        <SearchBox />
      </template>
      <template #fallback>
        <iconLoading />
      </template>
    </Suspense>
  </div>
</template>

<style>
* {
  font-family: Arial, Helvetica, sans-serif;
  box-sizing: border-box;
}

body,
html {
  margin: 0;
  padding: 0;
}

.center {
  display: flex;
  align-items: center;
  justify-content: center;
  width: 100vw;
  height: 100vh;
}
</style>
