<template>
  <div v-if="isOpen" class="overlay" @click="closeDropdown" />

  <div class="select">
    <input
      id="input"
      type="text"
      class="input-select"
      placeholder=""
      :value="selected?.description"
      :class="{ selected: selected?.description || isOpen }"
      readonly
      @focus="openDropdown"
      @click="openDropdown"
      @blur="closeDropdown"
      @keydown.down.prevent="navigateItem(1)"
      @keydown.up.prevent="navigateItem(-1)"
      @keydown.enter.prevent="selectActiveIndex"
      @keydown.esc="closeDropdown"
    />

    <label for="input" class="input-placeholder">{{ placeholder }}</label>

    <ul v-if="isOpen" class="dropdown">
      <li class="item">
        {{ shortPlaceholder ?? placeholder }}

        <button
          v-if="selected"
          class="icon-button trash"
          @mousedown="clearSelected"
        >
          <img class="icon" :src="trash" alt="svg" />
        </button>

        <button class="icon-button xmark" @mousedown="closeDropdown">
          <img class="icon" :src="xmark" alt="svg" />
        </button>
      </li>
      <li
        v-for="(item, index) in items"
        :key="item.code"
        class="item"
        :class="{ active: index === activeIndex }"
        @mousedown="selectItem(item)"
        @mouseenter="activeIndex = index"
      >
        {{ item.description }}
      </li>
    </ul>
  </div>
</template>

<script setup lang="ts">
import xmark from "~/shared/assets/icons/xmark.svg";
import trash from "~/shared/assets/icons/trash.svg";

type ItemType = { code: number; description: string };

const props = defineProps<{
  items: ItemType[];
  placeholder: string;
  shortPlaceholder?: string;
}>();

const selected = defineModel<ItemType | null>();

const isOpen = ref(false);
const activeIndex = ref(-1);

const openDropdown = () => {
  isOpen.value = true;
  activeIndex.value = -1;
};

const closeDropdown = () => {
  isOpen.value = false;
  activeIndex.value = -1;
};

const clearSelected = () => {
  selected.value = null;
  closeDropdown();
};

const selectItem = (item: ItemType) => {
  selected.value = item;
  closeDropdown();
};

const navigateItem = (direction: number) => {
  if (!isOpen.value) {
    openDropdown();
  }
  activeIndex.value =
    (activeIndex.value + direction + props.items.length) % props.items.length;
};

const selectActiveIndex = () => {
  if (activeIndex.value >= 0) {
    selectItem(props.items[activeIndex.value]);
  }
};
</script>

<style lang="scss" scoped>
$pading: 15px;

.select {
  position: relative;
  width: 100%;
  min-width: 150px;
  height: fit-content;
}

.input-select {
  width: 100%;
  padding: 20px 30px 7px 15px;
  border: 1px solid #76be85;
  cursor: pointer;

  &:focus-visible {
    outline: 1.5px solid #76be85;
  }
}

.input-placeholder {
  position: absolute;
  left: 17px;
  top: 50%;
  transform: translateY(-50%);
  transition: all 0.2s ease-out;
  padding: 5px 0;
  color: #aaa;
  font-size: 16px;
  pointer-events: none;
}

.input-select:focus + .input-placeholder,
.input-select.selected + .input-placeholder,
.input-select:not(:placeholder-shown) + .input-placeholder {
  top: 0;
  font-size: 12px;

  top: 0;
  transform: translateY(0);
}

.icon-button {
  position: absolute;
  padding: 10px;
  top: 50%;
  transform: translateY(-50%);
  height: 35px;
  border: none;

  .icon {
    width: 15px;
    height: 15px;
  }
}

.xmark {
  right: 10px;
}

.trash {
  right: 45px;
}

.dropdown {
  z-index: 101;
  position: absolute;
  width: 100%;
  background: white;
  border: 1px solid #ccc;
  max-height: 200px;
  overflow-y: auto;
  list-style: none;
  margin: 0;
  padding: 0;

  & .item {
    position: relative;
    padding: 8px;
    cursor: pointer;

    &:nth-child(1) {
      font-weight: 600;
      cursor: default;
    }

    &.active {
      background: #f0f0f0;
    }
  }
}

.overlay {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  z-index: 100;
  background-color: transparent;
}
</style>
