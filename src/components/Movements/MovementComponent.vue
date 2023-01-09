<script setup lang="ts">
import { computed, toRefs, defineEmits } from "vue";
import type { Movement } from "./types";
import { currencyFormatter } from "@/utils/currencyFormatter.js";

const props = defineProps<{
  movement: Movement;
}>();
const { movement } = toRefs(props);

const amountFormatted = computed(() => {
  return currencyFormatter.format(movement.value.amount);
});

const isNegative = computed(() => {
  return movement.value.amount < 0;
});

const emit = defineEmits(["remove"]);

const remove = () => {
  emit("remove", movement.value.id);
};
</script>

<template>
  <div class="movement">
    <div class="content">
      <h4>{{ movement.title }}</h4>
      <p>{{ movement.description }}</p>
    </div>
    <div class="action">
      <img src="@/assets/trash-icon.svg" alt="Delete" @click="remove" />
      <p :class="{ red: isNegative, green: !isNegative }">
        {{ amountFormatted }}
      </p>
    </div>
  </div>
</template>

<style scoped>
.movement {
  display: flex;
  justify-content: space-between;
  align-items: center;
  width: 100%;
  padding: 16px;
  background-color: #e6f9ff;
  border-radius: 8px;
  box-sizing: border-box;
}
.movement .content {
  width: 100%;
}
.movement .action {
  display: flex;
  justify-content: space-between;
  align-items: flex-end;
  flex-direction: column;
}
h4,
p {
  margin: 0;
  padding: 0;
}
h4 {
  margin-bottom: 8px;
}
.movement .action img {
  margin-bottom: 16px;
}
.red {
  color: red;
}
.green {
  color: green;
}
</style>
