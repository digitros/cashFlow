<script setup lang="ts">
import { computed, ref, toRefs, defineEmits, watch } from "vue";

const props = defineProps<{
  amounts: number[];
}>();
const { amounts } = toRefs(props);

const amountToPx = (amount: number) => {
  const min = Math.min(...amounts.value);
  const max = Math.max(...amounts.value);

  const absAmount = amount + Math.abs(min);
  const minmax = Math.abs(max) + Math.abs(min);

  const pixels = 150 - ((absAmount * 100) / minmax) * 1.5;
  return pixels || 0;
};

const zero = computed(() => {
  return amountToPx(0);
});

const points = computed(() => {
  const total = amounts.value.length;
  return amounts.value.reduce((points, amount, index) => {
    const x = (300 / total) * (index + 1);
    const y = amountToPx(amount);
    return `${points} ${x},${y}`;
  }, `0, ${amountToPx(amounts.value.length ? amounts.value[0] : 0)}`);
});

const showPointer = ref(false);
const pointer = ref(0);

const emit = defineEmits<{
  (event: "select", index: number): void;
  (event: "deselect"): void;
}>();

watch(pointer, (value) => {
  const index = Math.ceil(value / (300 / amounts.value.length)) || 0;
  if (index < 0 || index > amounts.value.length) {
    return;
  }
  emit("select", index);
});

const tap = (event: TouchEvent) => {
  const node = event.target as HTMLElement;
  showPointer.value = true;
  const elementWidth = node.getBoundingClientRect().width;
  const elementX = node.getBoundingClientRect().x;
  const touchX = event.touches[0].clientX;
  pointer.value = ((touchX - elementX) / elementWidth) * 300;
};

const untap = () => {
  showPointer.value = false;
  emit("deselect");
};
</script>

<template>
  <div>
    <svg
      view-box="0 0 300 150"
      @touchstart="tap"
      @touchmove="tap"
      @touchend="untap"
    >
      <line
        stroke="#c4c4c4"
        stroke-width="2"
        x1="0"
        :y1="zero"
        x2="300"
        :y2="zero"
      />
      <polyline
        fill="none"
        stroke="#0689B0"
        stroke-width="2"
        :points="points"
      />
      <line
        v-show="showPointer"
        stroke="#04b500"
        stroke-width="2"
        :x1="pointer"
        y1="0"
        :x2="pointer"
        y2="150"
      />
    </svg>
    <p>Latest 30 days</p>
  </div>
</template>

<style scoped>
svg {
  width: 100%;
}
p {
  text-align: center;
}
</style>
