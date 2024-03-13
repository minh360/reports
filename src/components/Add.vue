<!-- eslint-disable vue/multi-word-component-names -->
<template>
    <div class="h-[80vh] w-[60vw] border border-8 mx-auto my-auto px-[20px] py-[20px] bg-gray-300 text-[40px] flex flex-col gap-[7px]">
    <ShowPanel :car="car" @update:car="car = $event" @update:card="card = $event" :card="card"
        @update:num-car="numCar = $event" :num-car="numCar"
         @update:person="person = $event" :person="person" 
         @update:num-person="numPerson = $event" :num-person="numPerson" 
         :percent="percent"  @update:percent="percent = $event"
        :type-person="typePerson" @update:type-person="typePerson = $event" 
        :total-buy="totalBuy" @update:total-buy="totalBuy = $event" :label="'Thêm đoàn'">
        <div class="flex flex-row justify-end w-100"
            v-if="card != '' && car != '' && percent != '' && numCar != '' && person != '' && numPerson != '' && totalBuy != '' && typePerson != ''">
            <button type="button" @click="saveAdd()"
                class="text-[25px] mt-[2px] w-[200px] text-white bg-gradient-to-br from-pink-500 to-orange-400 hover:bg-gradient-to-bl focus:ring-4 focus:outline-none focus:ring-pink-200 dark:focus:ring-pink-800 font-medium rounded-lg px-5 py-2.5 text-center me-2 mb-2">Lưu</button>
        </div>
    </ShowPanel>
    </div>
</template>
<script setup>
import { ref,defineEmits } from "vue";
import ShowPanel from "./ShowPanel.vue";
const emit = defineEmits(['done']);
const card = ref("")
const car = ref("")
const numCar = ref("")
const person = ref("")
const numPerson = ref(10)
const percent = ref("")
const typePerson = ref("")
const totalBuy = ref("")
const saveAdd = () => {
    let a = localStorage.getItem('group') ? JSON.parse(localStorage.getItem('group')) : []
    a.push({
        card: card.value,
        numCar: Number(numCar.value),
        car: Number(car.value),
        person: Number(person.value),
        numPerson: Number(numPerson.value),
        percent: Number(percent.value),
        typePerson: typePerson.value,
        totalBuy: Number(totalBuy.value),
        total: Number(car.value) + (Number(person.value) * Number(numPerson.value)) + Number(percent.value),
        dropdown : false
    })
    const data = JSON.stringify(a)
    localStorage.setItem('group', data)
    emit('done')
}
</script>
<style lang="scss" scoped></style>
