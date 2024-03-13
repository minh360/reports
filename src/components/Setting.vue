<!-- eslint-disable vue/multi-word-component-names -->
<template>
  <div class="w-[80vw] h-full flex flex-row px-[50px] text-[35px] gap-[70px] overflow-y-auto">
    <div class=" flex flex-col gap-[20px]">
      <div class="flex flex-row gap-[10px] w-[40vw] h-[100px] items-baseline">
        <span class=" text-[50px]">Thêm loại khách</span>
        <button
          class="rounded border border-4 px-[20px] my-[40px] flex items-center hover:outline-none hover:ring hover:border-blue-300"
          v-if="!addMode" @click="() => addMode = true">+</button>
        <button
          class="rounded border border-4 px-[20px] my-[40px] flex items-center hover:outline-none hover:ring hover:border-blue-300"
          @click="changeRuleInfor()" v-if="changeRule">Lưu</button>
      </div>
      <div class="flex flex-row justify-between gap-[30px] items-baseline -mt-[80px]" v-if="addMode">
        <input type="text" v-model="newLabel"
          class="[appearance:textfield] [&::-webkit-outer-spin-button]:appearance-none [&::-webkit-inner-spin-button]:appearance-none 
          ml-[10px] pl-[10px] w-[100px] h-[50px] rounded border border-2 border-[black] focus:outline-none focus:ring focus:border-blue-300 
          bg-black-100" />
        <span>-------------------></span>
        <input type="text" v-model="newText"
          class="[appearance:textfield] [&::-webkit-outer-spin-button]:appearance-none [&::-webkit-inner-spin-button]:appearance-none 
          ml-[10px] pl-[10px] w-[260px] h-[50px] rounded border border-2 border-[black] focus:outline-none focus:ring focus:border-blue-300 
          bg-black-100" />
        <button
          class="rounded border border-4 px-[20px] my-[40px] flex items-center hover:outline-none hover:ring hover:border-blue-300"
          @click="addNewRuleInfor()">
          +
        </button>
        <button
          class="rounded border border-4 px-[20px] my-[40px] flex items-center hover:outline-none hover:ring hover:border-blue-300"
          @click="cancelAdd()">
          huỷ
        </button>
      </div>
      <span v-if="mes!=''" class="text-[red]">{{ mes }}</span>
      <div v-for="(item, index) in ruleInfor" :key="index" class="flex flex-grow w-[60vw]">
        <div class="flex flex-row w-full justify-between text-[35px] items-baseline">
          <div :class="{bg: index % 2==0}" class="flex flex-row w-full justify-between px-[10px] ">
            <span>{{ item.label }} -------------------></span>
            <div class="flex flex-row gap-[30px]">{{ item.text }}</div> 
          </div>
          <button class="ml-[10px] rounded border border-4 px-[20px] flex items-center hover:outline-none hover:ring hover:border-blue-300"
            @click="deleteNewRuleInfor(index)">Xoá</button>
        </div>
        </div>
      </div>
    </div>
</template>
<script setup>
import { ref } from 'vue';
const ruleInfor = ref(JSON.parse(localStorage.getItem('ruleInfor')))
const addMode = ref(false)
const newLabel = ref('')
const newText = ref('')
const mes = ref('')
const changeRule = ref(false)
const cancelAdd = () => {
  addMode.value = false
}
const addNewRuleInfor = () => {
  let flag = 0
  for(let i=0; i<ruleInfor.value.length;i++){
    if (ruleInfor.value[i].label == newLabel.value || ruleInfor.value[i].text == newText.value){
      flag+=1
      break
    }
  }
  if (flag){
    mes.value = 'Đã tồn tại !!!'
  }
  else{
    ruleInfor.value.push({label : newLabel.value, text: newText.value})
    changeRule.value = true
    addMode.value = false
  }
}
const deleteNewRuleInfor = index => {
  ruleInfor.value.splice(index,1)
  changeRule.value = true
}
const changeRuleInfor = () => {
  ruleInfor.value = JSON.stringify(ruleInfor.value)
  localStorage.setItem('ruleInfor', ruleInfor.value)
  ruleInfor.value = JSON.parse(localStorage.getItem('ruleInfor'))
  changeRule.value = false
  mes.value = ''
}
</script>
<style lang="scss" scoped>
.bg{
  background-color: gray;
}
</style>
