<script setup lang="ts">
import { ref } from "vue";
import Modal from "./ModalComponent.vue";

const showModal = ref(false);
const title = ref("");
const amount = ref(0);
const description = ref("");
const type = ref("Income");

const emit = defineEmits(["create"]);

const close = () => {
  showModal.value = false;
};

const submit = () => {
  close();
  emit("create", {
    title: title.value,
    amount: type.value === "Income" ? amount.value : amount.value * -1,
    description: description.value,
    time: new Date(),
    id: new Date().getTime(),
  });
  title.value = "";
  amount.value = 0;
  description.value = "";
  type.value = "Income";
};
</script>

<template>
  <button @click="showModal = true">Add Movement</button>
  <Modal @close="close" v-show="showModal"
    ><form @submit.prevent="submit">
      <div class="field">
        <label>Title</label>
        <input type="text" v-model="title" />
      </div>
      <div class="field">
        <label>Amount</label>
        <input type="number" v-model="amount" />
      </div>
      <div class="field">
        <label>Description</label>
        <textarea rows="4" v-model="description" />
      </div>
      <div class="field">
        <label class="radio-label">
          <input type="radio" value="Income" v-model="type" />
          <span>Income</span>
        </label>
        <label class="radio-label">
          <input type="radio" value="Expense" v-model="type" />
          <span>Expense</span>
        </label>
      </div>
      <div class="action">
        <button>Add</button>
      </div>
    </form>
  </Modal>
</template>

<style scoped>
button {
  color: white;
  font-size: 1.25rem;
  background-color: var(--brand-blue);
  border: none;
  width: 100%;
  padding: 24px 60px;
  border-radius: 60px;
  box-sizing: border-box;
}

form {
  font-size: 1.24rem;
  width: 100%;
}

form .action {
  padding: 0 24px;
}

.field {
  display: flex;
  justify-content: space-between;
  flex-direction: column;
  padding: 16px 24px;
}

label {
  margin-bottom: 8px;
}

input,
textarea {
  font-size: 1.24rem;
  border: 2px solid var(--brand-blue);
  border-radius: 8px;
  padding: 8px;
}

input[type="number"] {
  text-align: right;
}

.radio-label {
  display: flex;
  align-items: center;
  margin-top: 8px;
}

.radio-label span {
  margin-top: 4px;
  margin-left: 8px;
}

input[type="radio"] {
  appearance: none;
  width: 1.24rem;
  height: 1.24rem;
  color: var(--brand-blue);
  border: 2px solid var(--brand-blue);
  border-radius: 50%;
}

input[type="radio"]:checked {
  background-color: var(--brand-blue);
}
</style>
