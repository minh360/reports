<!-- eslint-disable vue/multi-word-component-names -->
<template>
  <div class="w-100 flex flex-row">
    <div
      class="h-[100vh] bg-[#2d4563] w-[300px] flex flex-col items-center pt-[40px] sticky top-0 "
    >
      <h1 class="text-white text-[40px] logo">Coconut Mart</h1>
      <div class="w-48 h-1 mx-auto my-4 bg-white border-0 rounded"></div>
      <div
        class="flex flex-col py-[50px] gap-[50px] cursor-pointer w-[200px] text-white w-100 text-[32px]"
      >
        <div
          class="hover:text-[green]"
          v-for="(item, index) in menu"
          :key="index"
          :class="{ active: item.active }"
          @click="selectItem(index)"
        >
          {{ item.label }}
        </div>
      </div>
    </div>
      <Wait v-if="window==WINDOW.WAIT"/>
      <Add v-if="window==WINDOW.ADD" />
      <Setting v-if="window==WINDOW.SETTING"/>
      <Sum v-if="window==WINDOW.SUM" />
  </div>
</template>
<script setup>
import { ref } from "vue";
import Add from "./Add.vue"
import Wait from "./Wait.vue";
import Setting from "./Setting.vue";
import Sum from "./Sum.vue";
const WINDOW= {
  ADD : 0,
  SUM : 1,
  TOTAL : 2,
  REPORTS : 3,
  SETTING : 4,
  WAIT : 5
}
const window = ref(WINDOW.WAIT)
const menu = ref([
  { label: "Thêm Đoàn", active: false },
  { label: "Tổng Đoàn", active: false },
  { label: "Tổng Kết", active: false },
  { label: "Báo Cáo", active: false },
  { label: "Cài Đặt", active: false },
]);
const selectItem = (index) => {
  for (let i = 0; i < menu.value.length; i++) {
    menu.value[i].active = false;
  }
  menu.value[index].active = true;
  window.value = Object.values(WINDOW)[index]
};
</script>
<style scoped lang="scss">

.active {
  color: green;
  text-decoration: underline;
}
</style>