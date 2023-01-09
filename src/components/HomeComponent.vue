<script setup lang="ts">
import Layout from "./LayoutComponent.vue";
import Header from "./HeaderComponent.vue";
import Resume from "./Resume/ResumeComponent.vue";
import Movements from "./Movements/MovementsComponent.vue";
import type { Movement } from "./Movements/types";

const amount = undefined;
const selectedDate = undefined;

const movements: Movement[] = [
  {
    id: 1,
    title: "Movement",
    description: "Deposit",
    amount: 1000,
  },
  {
    id: 2,
    title: "Movement 1",
    description: "Deposit",
    amount: 500,
  },
  {
    id: 3,
    title: "Movement 3",
    description: "Food",
    amount: -100,
  },
  {
    id: 4,
    title: "Movement 4",
    description: "Education",
    amount: -1000,
  },
  {
    id: 5,
    title: "Movement 5",
    description: "Computer",
    amount: -1000,
  },
];
</script>

<template>
  <Layout>
    <template #header>
      <Header />
    </template>
    <template #resume>
      <div>
        <Resume
          :selectedDate="selectedDate"
          :total-amount="10000"
          :amount="amount"
        >
          <template #graphic>
            <div>Graphic</div>
          </template>
          <template #action>
            <div>Action</div>
          </template>
        </Resume>
      </div>
    </template>
    <template #movements>
      <div>
        <Movements :movements="movements" />
      </div>
    </template>
  </Layout>
</template>
