<script setup lang="ts">
import Layout from "./LayoutComponent.vue";
import Header from "./HeaderComponent.vue";
import Resume from "./Resume/ResumeComponent.vue";
import Movements from "./Movements/MovementsComponent.vue";
import Action from "./ActionComponent.vue";
import Graphic from "./Resume/GraphicComponent.vue";
import type { MovementType } from "./Movements/types";
import { computed, onMounted, ref } from "vue";

const amount = ref<number | undefined>(undefined);
const selectedDate = ref<string | undefined>(undefined);

const movements = ref<MovementType[]>([]);

const amounts = computed(() => {
  const lastDays = movements.value
    .filter((movement) => {
      const today = new Date();
      const oldDate = today.setDate(today.getDate() - 30);
      return movement.time >= new Date(oldDate);
    })
    .map((movement) => movement.amount);

  return lastDays.map((m, i) => {
    const lastMovements = lastDays.slice(0, i + 1);
    return lastMovements.reduce((sum, movement) => sum + movement, 0);
  });
});

const create = (movement: MovementType) => {
  movements.value.push(movement);
  saveOnLocalStorage();
};

const remove = (id: number) => {
  movements.value = movements.value.filter((movement) => movement.id !== id);
  saveOnLocalStorage();
};

const saveOnLocalStorage = () => {
  localStorage.setItem("movements", JSON.stringify(movements.value));
};

const totalAmount = computed(() => {
  return movements.value.reduce((sum, movement) => sum + movement.amount, 0);
});

const select = (index: number) => {
  amount.value = amounts.value[index - 1];
  selectedDate.value = movements.value[index - 1].time.toLocaleDateString(
    "en-US",
    {
      year: "numeric",
      month: "long",
      day: "numeric",
      hour: "2-digit",
      minute: "2-digit",
      second: "2-digit",
    }
  );
};

const deselect = () => {
  amount.value = undefined;
  selectedDate.value = undefined;
};

onMounted(() => {
  const movementsFromLocalStorage = localStorage.getItem("movements");
  if (movementsFromLocalStorage) {
    movements.value = JSON.parse(movementsFromLocalStorage)?.map(
      (movement: any) => ({
        ...movement,
        time: new Date(movement.time),
      })
    );
  }
});
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
          :total-amount="totalAmount"
          :amount="amount"
        >
          <template #graphic>
            <Graphic @select="select" @deselect="deselect" :amounts="amounts" />
          </template>
          <template #action>
            <Action @create="create" />
          </template>
        </Resume>
      </div>
    </template>
    <template #movements>
      <div>
        <Movements @remove="remove" :movements="movements" />
      </div>
    </template>
  </Layout>
</template>
