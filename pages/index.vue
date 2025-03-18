<template>
  <div class="main-page">
    <div class="selected-container">
      <Select
        v-model="selected1"
        :items="items1"
        placeholder="Статус поставки"
        short-placeholder="Статус"
      />

      <p>Value: {{ selected1 }}</p>
    </div>

    <div class="selected-container">
      <Select
        v-model="selected2"
        :items="items2"
        placeholder="Режим перемещения"
        short-placeholder="Режим"
      />

      <p>Value: {{ selected2 }}</p>
    </div>
  </div>
</template>

<script setup lang="ts">
import { Select } from "~/shared/ui/select";

const selected1 = ref<{
  code: number;
  description: string;
} | null>(null);

const selected2 = ref<{
  code: number;
  description: string;
} | null>(null);

const items1 = [
  {
    code: 0,
    description: "Формируется",
  },
  {
    code: 10,
    description: "Товар прибыл",
  },
  {
    code: 20,
    description: "Товар прибыл (срочная поставка)",
  },
  {
    code: 30,
    description: "Оформление ДТ",
  },
  {
    code: 40,
    description: "ДТ выпущена",
  },
  {
    code: 50,
    description: "Поставка закрыта",
  },
];

const items2 = [
  {
    code: 1,
    description: "Импорт",
  },
  {
    code: 2,
    description: "Экспорт",
  },
];
</script>

<style lang="css" scoped>
.main-page {
  display: flex;
  flex-direction: column;
  gap: 30px;
}

.selected-container {
  display: flex;
  align-items: center;
  flex-wrap: wrap;
  gap: 5px 20px;
}

:deep(.select) {
  width: 300px;
}
</style>

<style>
* {
  box-sizing: border-box;
}

body {
  font-family: "Segoe UI", Tahoma, Geneva, Verdana, sans-serif;
}

button {
  cursor: pointer;
  background: none;
  border: 1px solid;
  padding: 0;
}

p {
  margin: 0;
}
</style>
