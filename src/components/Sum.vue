<!-- eslint-disable vue/multi-word-component-names -->
<template>
    <div
        class="overflow-y-auto h-[100vh] mt-[20px] w-[60vw] border border-8 mx-auto my-auto px-[20px] py-[20px] bg-gray-300 text-[40px]  gap-[7px]">
        <h1 class="w-[55vw] text-[45px] flex justify-center text-[green] sticky top-0">
            Tổng {{ groupLength }} đoàn
        </h1>
        <div class="flex flex-row justify-end w-100" v-if="changeValue">
            <button type="button" @click="saveAdd()"
                class="text-[25px] mt-[2px] w-[200px] text-white bg-gradient-to-br from-pink-500 to-orange-400 hover:bg-gradient-to-bl focus:ring-4 focus:outline-none focus:ring-pink-200 dark:focus:ring-pink-800 font-medium rounded-lg px-5 py-2.5 text-center me-2 mb-2">Hoàn
                tất</button>
        </div>
        <div v-for="(item, index) in group" :key="index">
            <ShowPanel :label="'Đoàn ' + (index + 1)" :car="item.car" @update:car="change(SELECT_ITEM.CAR, $event, index)"
                @update:card="change(SELECT_ITEM.CARD, $event, index)" :card="item.card"
                @update:num-car="change(SELECT_ITEM.NUM_CAR, $event, index)" :num-car="item.numCar"
                @update:person="change(SELECT_ITEM.PERSON, $event, index)" :person="item.person"
                @update:num-person="change(SELECT_ITEM.NUM_PERSON, $event, index)" :num-person="item.numPerson"
                :percent="item.percent" @update:percent="change(SELECT_ITEM.PERCENT, $event, index)"
                :type-person="item.typePerson" @update:type-person="change(SELECT_ITEM.TYPE_PERSON, $event, index)"
                :total-buy="item.totalBuy" @update:total-buy="change(SELECT_ITEM.TOTAL_BUY, $event, index)">
            </ShowPanel>
        </div>

    </div>
</template>
<script setup>
import { computed, ref } from 'vue'
import ShowPanel from './ShowPanel.vue';
// localStorage.removeItem('group')
const SELECT_ITEM = {
    CAR: 1,
    NUM_CAR: 2,
    CARD: 0,
    PERSON: 4,
    NUM_PERSON: 5,
    PERCENT: 6,
    TYPE_PERSON: 7,
    TOTAL_BUY: 8
}
const group = ref(JSON.parse(localStorage.getItem('group')) || [])
const groupLength = computed(() => {
    return group.value.length
})
const changeValue = ref(false)
const change = (select, value, index) => {
    if (value != "") {
        changeValue.value = true
        switch (select) {
            case SELECT_ITEM.CAR:
                group.value[index].car = value
                break
            case SELECT_ITEM.CARD:
                group.value[index].card = value
                break
            case SELECT_ITEM.NUM_CAR:
                group.value[index].numCar = value
                break
            case SELECT_ITEM.PERSON:
                group.value[index].person = value;
                break
            case SELECT_ITEM.NUM_PERSON:
                group.value[index].numPerson = value;
                break
            case SELECT_ITEM.PERCENT:
                group.value[index].percent = value;
                break
            case SELECT_ITEM.TYPE_PERSON:
                group.value[index].typePerson = value;
                break
            case SELECT_ITEM.TOTAL_BUY:
                group.value[index].totalBuy = value;
                break
        }
    }

}
const saveAdd = () => {
    group.value = JSON.stringify(group.value)
    console.log(group.value)
    localStorage.setItem('group', group.value)
    group.value = JSON.parse(localStorage.getItem('group'))
    changeValue.value = false
}
</script>
<style lang="scss" scoped></style>