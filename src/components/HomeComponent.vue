<script setup lang="ts">
import Layout from "./LayoutComponent.vue";
import Header from "./HeaderComponent.vue";
import Resume from "./Resume/ResumeComponent.vue";
import Movements from "./MovementsComponent.vue";
</script>

<template>
  <Layout>
    <template #header>
      <Header />
    </template>
    <template #resume>
      <div>
        <Resume />
      </div>
    </template>
    <template #movements>
      <div>
        <Movements />
      </div>
    </template>
  </Layout>
</template>
