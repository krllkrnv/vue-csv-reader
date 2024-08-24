<template>
  <div class="min-h-screen flex flex-col items-center">
    <header-component class="mb-10"></header-component>
    <div class="w-full max-w-7xl px-14">
      <main-page></main-page>
    </div>
  </div>
</template>

<script setup>
import MainPage from "./pages/MainPage.vue";
import HeaderComponent from "./components/HeaderComponent.vue";
</script>

<style lang="scss" scoped></style>
