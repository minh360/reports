<!-- eslint-disable vue/multi-word-component-names -->
<template>
    <div class="text-[35px] pl-[30px] pt-[30px] w-[80vw] flex flex-row justify-between gap-[70px]">
        <div>
            <div class="flex flex-row w-full justify-between">
                <div class="flex flex-col  gap-[20px]">
                    <h1>Bán lẻ :</h1>
                    <h1>Đặc sản :</h1>
                </div>
                <div class="flex flex-col  gap-[20px]">
                    <input v-model="banle" type="number" class="pl-[10px] [appearance:textfield] [&::-webkit-outer-spin-button]:appearance-none 
                    [&::-webkit-inner-spin-button]:appearance-none ml-[20px] w-[300px] h-[50px] rounded border border-2 
                    focus:outline-none focus:ring focus:border-blue-300 bg-black-300" />
                    <input v-model="dacsan" type="number" class="pl-[10px] [appearance:textfield] [&::-webkit-outer-spin-button]:appearance-none 
                    [&::-webkit-inner-spin-button]:appearance-none ml-[20px] w-[300px] h-[50px] rounded border border-2 
                    focus:outline-none focus:ring focus:border-blue-300 bg-black-300" />
                </div>
            </div>
            <div class="w-[20vw] h-1 mx-auto my-4 bg-black border-0 rounded"></div>
            <div ref="r1">
                {{ date }}
                <div v-if="group != '' && groupTq != []">
                    <div v-for="rp in report1" :key="rp">
                        <h1 v-if="rp.car != 0">
                            {{ rp.car }} xe {{ rp.label }} / {{ rp.person }} người / = {{ rp.totalBuy }}
                        </h1>
                    </div>
                    <div v-for="item in reportTq" :key="item">
                        <h1 v-if="item.car != 0">
                            {{ item.car }} xe {{ item.label }} / tham quan / = {{ item.totalBuy }}
                        </h1>
                    </div>
                    <h1 v-if="banle != ''">Bán lẻ = {{ formatNum(banle) }}</h1>
                    <h1 v-if="dacsan != ''">Đặc sản = {{ formatNum(dacsan) }}</h1>
                    <h1>Tổng z = {{ lumtien }}</h1>
                </div>
                <div v-else>
                    <h1>Không có đoàn</h1>
                </div>
                
            </div>
            <button @click="copy(1)" class="rounded border border-4 px-[20px] my-[40px] flex items-center hover:outline-none 
                hover:ring hover:border-blue-300">
                {{ mes1 }}
            </button>
        </div>
        <div class="flex-grow gap-[20px]">
            <div>Chi c.k : <input v-model="chick" type="number" class="pl-[10px] [appearance:textfield] [&::-webkit-outer-spin-button]:appearance-none 
                    [&::-webkit-inner-spin-button]:appearance-none ml-[20px] w-[300px] h-[50px] rounded border border-2 
                    focus:outline-none focus:ring focus:border-blue-300 bg-black-300" />
            </div>
            <div class="w-[20vw] h-1 mx-auto my-4 bg-black border-0 rounded"></div>
            <div ref="r2">
                {{ date }}
                <h1>Chi đầu xe + người + phần trăm hướng dẫn</h1>
                <h1>{{ report2.numCar }} xe = {{ report2.car }}</h1>
                <h1>{{ report2.numPerson }} người = {{ report2.person }}</h1>
                <h1>Phần trăm = {{ report2.percent }}</h1>
                <h1>Tổng chi = {{ report2.total }}</h1>
                <h1>( chi mặt {{  formatNum(total - chick) }} <h1 v-if="chick > 0"> ; chi c.k {{ formatNum(chick) }}</h1>)</h1>
                <h1>Tiền thu về {{ lumtien }} dư </h1>
                <h1>Mặt = </h1>
                <h1>Quẹt thẻ = </h1>
                <h1>Chuyển khoản = </h1>
            </div>
            <div class="flex flex-row gap-[50px]">
                <button @click="copy(2)" class="rounded border border-4 px-[20px] my-[40px] flex items-center hover:outline-none hover:ring 
                    hover:border-blue-300">{{
                        mes2 }}</button>
            </div>
        </div>
    </div>
</template>
<script setup>
import { onMounted, ref, computed } from 'vue';
const ruleInfor = ref(JSON.parse(localStorage.getItem('ruleInfor')))
const group = ref(JSON.parse(localStorage.getItem('group')))
const rule = ref(JSON.parse(localStorage.getItem('rule')))
const groupTq = ref([])
const r1 = ref(null)
const mes1 = ref('Copy')
const r2 = ref(null)
const mes2 = ref('Copy')
const today = new Date()
const date = String(today.getDate()) + '/' + String(today.getMonth() + 1) + '/' + String(today.getFullYear()) + " :"
const banle = ref("")
const dacsan = ref('')
const sum = ref(0)
const chick = ref(0)
const total = ref(0)
const lumtien = computed(() => {
    return formatNum(Number(dacsan.value) + Number(banle.value) + sum.value)
})
const copy = num => {
    navigator.clipboard.writeText(num == 1 ? r1.value.innerText : r2.value.innerText)
    mes1.value = num == 1 ? "Đã copy" : "Copy"
    mes2.value = num == 2 ? 'Đã copy' : 'Copy'
}
const formatNum = num => {
    return num.toLocaleString()
}
let report1 = ref([])
let reportTq = ref([])
let report2 = ref([])
onMounted(() => {
    function customSort(a, b) {
        return rule.value.indexOf(a.label[0]) - rule.value.indexOf(b.label[0]);
    }
    for (let z = 0; z < group.value.length; z++) {
        if (group.value[z].person == 0) {
            groupTq.value.push(group.value[z])
        }
    }
    for (let i = 0; i < ruleInfor.value.length; i++) {
        let car = 0
        let carTq = 0
        let person = 0
        let totalBuyTq = 0
        let totalBuy = 0
        for (let j = 0; j < group.value.length; j++) {
            if (ruleInfor.value[i].label == group.value[j].typePerson && group.value[j].person != 0) {
                car += group.value[j].numCar
                person += group.value[j].person
                totalBuy += group.value[j].totalBuy
                sum.value += group.value[j].totalBuy
            }
        }
        for (let s = 0; s < groupTq.value.length; s++) {
            if (ruleInfor.value[i].label == groupTq.value[s].typePerson) {
                carTq += groupTq.value[s].numCar
                totalBuyTq += groupTq.value[s].totalBuy
                sum.value += groupTq.value[s].totalBuy
            }
        }
        report1.value.push({ label: ruleInfor.value[i].text, car: car, person: person, totalBuy: formatNum(totalBuy) })
        reportTq.value.push({ label: ruleInfor.value[i].text, car: carTq, totalBuy: formatNum(totalBuyTq) })
    }
    let numCar = 0
    let car = 0
    let numPerson = 0
    let person = 0
    let percent = 0
    let totalz = 0
    for (let k = 0; k < group.value.length; k++) {
        numCar += group.value[k].numCar
        car += group.value[k].car
        numPerson += (group.value[k].person == 0 || group.value[k].numPerson == 0 ? 0 : group.value[k].person)
        person += (group.value[k].numPerson * group.value[k].person)
        percent += group.value[k].percent
        totalz += group.value[k].total
    }
    total.value = totalz
    report2.value = { numCar: numCar, car: formatNum(car), numPerson: formatNum(numPerson), person: formatNum(person), percent: formatNum(percent), total:  formatNum(totalz) }
    report1.value = report1.value.sort(customSort)
    return report1, report2, sum
})

</script>
<style lang="scss" scoped></style>