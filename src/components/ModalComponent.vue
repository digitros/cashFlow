<script setup lang="ts">
import { defineEmits } from "vue";

const emit = defineEmits(["close"]);

const close = () => {
  emit("close");
};
</script>

<template>
  <div class="modal">
    <div class="head">
      <p>New movement</p>
      <img @click="close" src="@/assets/close-icon.svg" alt="" />
    </div>
    <div class="body">
      <slot></slot>
    </div>
  </div>
</template>

<style scoped>
.modal {
  z-index: 2;
  position: absolute;
  background-color: #fff;
  width: 90%;
  height: 90%;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  border-radius: 8px;
  box-shadow: 0 8px 16px #e5e5e5;
}

.head {
  display: flex;
  justify-content: space-between;
  align-items: center;
  width: 100%;
  padding: 24px 24px;
  box-sizing: border-box;
}

p {
  margin: 0;
  font-size: 1.5rem;
  color: var(--brand-blue);
}
</style>
