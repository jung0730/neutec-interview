<template>
  <div class="container">
    <button class="menu" @click="toggleSidebar"></button>
    <transition>
      <nav class="sidebar" v-if="isSidebarVisible">
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

<script>
export default {
  data() {
    return {
      isSidebarVisible: false,
      selectedCategoryKey: null,
      selectedSubcategoryKey: null,
      menuItems: [
        {
          key: '64f',
          text: '好喝黑糖',
          children: [
            {
              key: '445',
              text: '黑糖鮮奶',
              children: [
                { key: '37a', text: '黑糖珍珠鮮奶' },
                { key: 'feb', text: '黑糖芋圓鮮奶' },
                { key: '59c', text: '黑糖蒟蒻鮮奶' },
              ],
            },
            {
              key: '29e',
              text: '黑糖冬瓜',
              children: [
                { key: 'ac3', text: '黑糖冬瓜牛奶' },
                { key: 'ca0', text: '黑糖冬瓜珍珠' },
              ],
            },
          ],
        },
        {
          key: '6c3',
          text: '茶',
          children: [
            { key: '5dc', text: '烏龍綠' },
            { key: 'b5f', text: '綠茶' },
            { key: 'b09', text: '紅茶' },
            { key: '887', text: '青茶' },
          ],
        },
        {
          key: 'c81',
          text: '咖啡',
          children: [
            {
              key: 'e02',
              text: '黑咖啡',
              children: [
                { key: 'd20', text: '濃縮咖啡' },
                { key: '1f8', text: '美式咖啡' },
              ],
            },
            {
              key: 'd7a',
              text: '牛奶咖啡',
              children: [
                {
                  key: 'c09',
                  text: '拿鐵',
                  children: [
                    { key: 'db2', text: '黑糖拿鐵' },
                    { key: '9f6', text: '榛果拿鐵' },
                    { key: 'b61', text: '香草拿鐵' },
                  ],
                },
                { key: '9ac', text: '卡布奇諾' },
                { key: 'ce8', text: '摩卡' },
              ],
            },
          ],
        },
      ],
    };
  },
  methods: {
    toggleSidebar() {
      this.isSidebarVisible = !this.isSidebarVisible;
    },
    toggleItem(item, selectedItemKey, selectedSubItemKey) {
      if (this[selectedItemKey] === item.key) {
        this[selectedItemKey] = null;
      } else {
        this[selectedItemKey] = item.key;
        if (selectedSubItemKey) {
          this[selectedSubItemKey] = null;
        }
      }
      item.expanded = !item.expanded;
    },
    toggleCategory(category) {
      this.toggleItem(category, 'selectedCategoryKey', 'selectedSubcategoryKey');
      this.closeOtherItems(this.menuItems, category.key, 'expanded');
    },
    toggleSubCategory(subcategory) {
      this.toggleItem(subcategory, 'selectedSubcategoryKey');
      this.closeOtherSubItems(subcategory);
    },
    closeOtherItems(items, keyToKeep, propertyToUpdate) {
      items.forEach((item) => {
        if (item.key !== keyToKeep) {
          item[propertyToUpdate] = false;
        }
      });
    },
    closeOtherSubItems(subcategory) {
      this.menuItems.forEach((category) => {
        if (category.children) {
          this.closeOtherItems(category.children, subcategory.key, 'expanded');
        }
      });
    },
    isCategorySelected(category) {
      return this.selectedCategoryKey === category.key;
    },
  },
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
