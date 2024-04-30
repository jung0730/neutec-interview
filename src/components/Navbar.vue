<template>
  <div class="container">
    <button class="menu" @click="toggleSidebar"></button>
    <transition>
      <nav class="sidebar" v-if="isSidebarVisible" v-focus tabindex="0" @blur="closeSidebar">
        <div
          v-for="category in menuItems"
          class="items"
          :key="category.key"
          :class="{ selected: isCategorySelected(category) }"
        >
          <div @click="toggleCategory(category)" class="text">
            {{ category.text }}
          </div>
          <div v-show="category.expanded" class="sub-items">
            <div v-for="subcategory in category.children" :key="subcategory.key">
              <div @click="toggleSubCategory(subcategory)" class="text">
                {{ subcategory.text }}
              </div>
              <div v-show="subcategory.children && subcategory.expanded" class="sub-items-children">
                <div v-for="item in subcategory.children" :key="item.key" class="text">{{ item.text }}</div>
              </div>
            </div>
          </div>
        </div>
      </nav>
    </transition>
  </div>
</template>
<script setup>
import { ref, reactive } from 'vue';
import items from '../data.js';

const isSidebarVisible = ref(false);
const selectedCategoryKey = ref(null);
const selectedSubcategoryKey = ref(null);

const menuItems = reactive(items);

const closeSidebar = () => {
  isSidebarVisible.value = false;
};
const toggleSidebar = () => {
  isSidebarVisible.value = !isSidebarVisible.value;
};
const toggleItem = (item, selectedItemKey, selectedSubItemKey) => {
  if (selectedItemKey.value === item.key) {
    selectedItemKey.value = null;
  } else {
    selectedItemKey.value = item.key;
    if (selectedSubItemKey) {
      selectedSubItemKey.value = null;
    }
  }
  item.expanded = !item.expanded;
};
const toggleCategory = (category) => {
  toggleItem(category, selectedCategoryKey, selectedSubcategoryKey);
  closeOtherItems(menuItems, category.key, 'expanded');
};
const toggleSubCategory = (subcategory) => {
  toggleItem(subcategory, selectedSubcategoryKey);
  closeOtherSubItems(subcategory);
};
const closeOtherItems = (items, keyToKeep, propertyToUpdate) => {
  items.forEach((item) => {
    if (item.key !== keyToKeep) {
      item[propertyToUpdate] = false;
    }
  });
};
const closeOtherSubItems = (subcategory) => {
  menuItems.forEach((category) => {
    if (category.children) {
      closeOtherItems(category.children, subcategory.key, 'expanded');
    }
  });
};
const isCategorySelected = (category) => {
  return selectedCategoryKey.value === category.key;
};
</script>

<style scoped>
.container {
  position: relative;
}

.menu {
  position: absolute;
  top: 10px;
  right: 10px;
  width: 40px;
  height: 40px;
  background-color: transparent;
  border: none;
  outline: none;
  cursor: pointer;
}

.menu:before {
  content: '☰';
  font-size: 24px;
  color: #000;
}

.sidebar {
  top: 0;
  width: 45vw;
  background-color: rgba(0, 0, 0, 0.9);
  position: fixed;
  height: 100vh;
  right: 0;
  z-index: 2;
  color: white;
}

.items {
  font-size: 24px;
  padding: 10px;
  cursor: pointer;
}

.selected {
  background: gray;
}

.text:hover {
  color: yellow;
}

.sub-items {
  font-size: 16px;
  padding: 10px;
}

.sub-items-children {
  font-size: 8px;
  padding: 10px;
}

.v-enter-active,
.v-leave-active {
  transition: opacity 1s;
}

.v-enter-from,
.v-leave-to {
  opacity: 0;
}

.v-enter-to,
.v-leave-from {
  opacity: 1;
}
</style>
