<script setup lang="ts">
import { currencyFormatter } from "@/utils/currencyFormatter.js";
import { computed, toRefs } from "vue";

const props = defineProps<{
  totalAmount: number;
  amount?: number;
  selectedDate?: Date;
}>();
const { totalAmount, amount, selectedDate } = toRefs(props);

const visualAmount = computed(() => {
  return amount?.value ? amount.value : totalAmount.value;
});
const labelToDisplay = computed(() => {
  return selectedDate?.value ? selectedDate.value : "Total amount";
});
const amountFormatted = computed(() => {
  return currencyFormatter.format(visualAmount.value);
});
</script>

<template>
  <main>
    <p>{{ labelToDisplay }}</p>
    <h1>{{ amountFormatted }}</h1>
    <div class="graphic">
      <slot name="graphic"></slot>
    </div>
    <div class="action">
      <slot name="action"></slot>
    </div>
  </main>
</template>

<style scoped>
main {
  display: flex;
  justify-content: center;
  align-items: center;
  flex-direction: column;
  width: 100%;
}
h1,
p {
  margin: 0;
  text-align: center;
}
h1 {
  margin-top: 14px;
  color: var(--brand-green);
}
.graphic {
  display: flex;
  justify-content: center;
  align-items: center;
  width: 100%;
  padding: 48px 24px;
  box-sizing: border-box;
}
</style>
