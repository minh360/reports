<!-- eslint-disable vue/multi-word-component-names -->
<template>
    <div
        class="overflow-y-auto h-[100vh] mt-[20px] w-[60vw] border border-8 mx-auto my-auto px-[20px] py-[20px] bg-gray-300 text-[40px]  
        gap-[7px]">
        <h1 class="w-[55vw] text-[45px] flex justify-center text-[orange] bg-gray-500 sticky top-0">
            Tổng {{ groupLength }} đoàn
        </h1>
        <div class="flex flex-row justify-end w-100 mr-[30px]" v-if="changeValue">
            <button type="button" @click="saveAdd()"
                class="text-[25px] mt-[2px] w-[200px] text-white bg-gradient-to-br from-pink-500 to-orange-400 hover:bg-gradient-to-bl 
                focus:ring-4 focus:outline-none focus:ring-pink-200 dark:focus:ring-pink-800 font-medium rounded-lg px-5 py-2.5 text-center 
                me-2 mb-2">Lưu lại</button>
        </div>
        <div v-for="(item, index) in group" :key="index">
            <div> 
            <ShowPanel :label="'Đoàn ' + (index + 1) + ':\t\t\t'+ formatNum(item.totalBuy)" :dropdown="item.dropdown"
                @update:dropdown="change(SELECT_ITEM.DROPDOWN, value, index)" 
                :carS="item.carS" @update:carS="change(SELECT_ITEM.CAR_S, $event, index)" 
                :carM="item.carM" @update:carM="change(SELECT_ITEM.CAR_M, $event, index)" 
                :carL="item.carL" @update:carL="change(SELECT_ITEM.CAR_L, $event, index)" 
                @update:card="change(SELECT_ITEM.CARD, $event, index)"
                :card="item.card"
                @update:person="change(SELECT_ITEM.PERSON, $event, index)" :person="item.person"
                @update:num-person="change(SELECT_ITEM.NUM_PERSON, $event, index)" :num-person="item.numPerson"
                :percent="item.percent" @update:percent="change(SELECT_ITEM.PERCENT, $event, index)"
                :type-person="item.typePerson" @update:type-person="change(SELECT_ITEM.TYPE_PERSON, $event, index)"
                :total-buy="item.totalBuy" @update:total-buy="change(SELECT_ITEM.TOTAL_BUY, $event, index)"
                :car7="item.car7" @update:car6="change(SELECT_ITEM.TYPE_CAR, $event, index)" 
                :car16="item.car16" @update:car16="change(SELECT_ITEM.TYPE_CAR, $event, index)" 
                :index="index">
            </ShowPanel>
            </div>
            <div class="flex flex-row justify-end w-100 mr-[30px]" v-show="item.dropdown">
                <button type="button" @click="deleteGroup(index)"
                    class="text-[25px] mt-[10px] w-[200px] text-white bg-gradient-to-br from-pink-500 to-orange-400 hover:bg-gradient-to-bl focus:ring-4 focus:outline-none focus:ring-pink-200 dark:focus:ring-pink-800 font-medium rounded-lg px-5 py-2.5 text-center me-2 mb-2">
                    Xoá
                </button>
            </div>
            <div class="w-[40vw] h-1 mx-auto my-4 bg-white border-0 rounded"></div>
        </div>

    </div>
</template>
<script setup>
import { computed, ref } from 'vue'
import ShowPanel from './ShowPanel.vue';
const SELECT_ITEM = {
    CAR_S: 1,
    CAR_M: 2,
    CAR_L: 3,
    CARD: 0,
    PERSON: 4,
    NUM_PERSON: 5,
    PERCENT: 6,
    TYPE_PERSON: 7,
    TOTAL_BUY: 8,
    DROPDOWN: 9,
    TYPE_CAR: 10,
}
const group = ref(JSON.parse(localStorage.getItem('group')) || [])
const groupLength = computed(() => {
    return group.value.length
})
const changeValue = ref(false)
const change = (select, value, index) => {
    if (value != "") {
        switch (select) {
            case SELECT_ITEM.CAR_S:
            changeValue.value = true
            group.value[index].carS = (value ? value : 0)
            break
        case SELECT_ITEM.CAR_M:
            changeValue.value = true
            group.value[index].carM = (value ? value : 0)
            break
        case SELECT_ITEM.CAR_L:
            changeValue.value = true
            group.value[index].carL = (value ? value : 0)
            break
        case SELECT_ITEM.CARD:
            changeValue.value = true
            group.value[index].card = value
            break
        case SELECT_ITEM.PERSON:
            changeValue.value = true
            group.value[index].person = value;
            break
        case SELECT_ITEM.NUM_PERSON:
            changeValue.value = true
            group.value[index].numPerson = value;
            break
        case SELECT_ITEM.PERCENT:
            changeValue.value = true
            group.value[index].percent = value;
            break
        case SELECT_ITEM.TYPE_PERSON:
            changeValue.value = true
            group.value[index].typePerson = value;
            break
        case SELECT_ITEM.TOTAL_BUY:
            changeValue.value = true
            group.value[index].totalBuy = Number(value);
            break
        case SELECT_ITEM.DROPDOWN:
            group.value[index].dropdown = !group.value[index].dropdown;
            break
        case SELECT_ITEM.TYPE_CAR:
            changeValue.value = true
            group.value[index].car16 = !group.value[index].car16;
            group.value[index].car7= !group.value[index].car7;
            break
        }
    }

}
const formatNum = num => {
    return num.toLocaleString()
}
const deleteGroup = index => {
    changeValue.value = true
    group.value.splice(index, 1)
}
const saveAdd = () => {
    for(let i=0;i<group.value.length;i++){
        group.value[i].dropdown = false
    }
    group.value = JSON.stringify(group.value)
    localStorage.setItem('group', group.value)
    group.value = JSON.parse(localStorage.getItem('group'))
    changeValue.value = false
}
</script>
<style lang="scss" scoped></style>