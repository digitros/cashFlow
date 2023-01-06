<script setup lang="ts">
import SplashScreen from "@/components/SplashScreen.vue";
import { defineAsyncComponent } from "vue";

const Home = defineAsyncComponent(
  () => import("@/components/HomeComponent.vue")
);
</script>

<template>
  <Suspense>
    <template #default>
      <Home />
    </template>
    <template #fallback>
      <SplashScreen />
    </template>
  </Suspense>
</template>

<style>
html,
body,
.app {
  min-height: 100vh;
  margin: 0;
  font-family: Arial, Helvetica, sans-serif;
}

* {
  --brand-green: #04b500;
  --brand-blue: #0689b0;
}
</style>
