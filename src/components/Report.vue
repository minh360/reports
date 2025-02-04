<!-- eslint-disable vue/multi-word-component-names -->
<template>
    <div class="text-[35px] pl-[30px] pt-[30px] w-[80vw] flex flex-row justify-between gap-[70px]">
        <div>
            <div class="flex flex-row w-full justify-between">
                <div class="flex flex-col  gap-[20px]">
                    <h1>Bán lẻ :</h1>
                    <h1>Nước + Kem :</h1>
                    <h1>Cà Phê:</h1>
                </div>
                <div class="flex flex-col  gap-[20px]">
                    <input v-model="banle" type="number" class="pl-[10px] [appearance:textfield] [&::-webkit-outer-spin-button]:appearance-none 
                    [&::-webkit-inner-spin-button]:appearance-none ml-[20px] w-[300px] h-[50px] rounded border border-2 
                    focus:outline-none focus:ring focus:border-blue-300 bg-black-300" />
                    <input v-model="dacsan" type="number" class="pl-[10px] [appearance:textfield] [&::-webkit-outer-spin-button]:appearance-none 
                    [&::-webkit-inner-spin-button]:appearance-none ml-[20px] w-[300px] h-[50px] rounded border border-2 
                    focus:outline-none focus:ring focus:border-blue-300 bg-black-300" />
                    <input v-model="caphe" type="number" class="pl-[10px] [appearance:textfield] [&::-webkit-outer-spin-button]:appearance-none 
                    [&::-webkit-inner-spin-button]:appearance-none ml-[20px] w-[300px] h-[50px] rounded border border-2 
                    focus:outline-none focus:ring focus:border-blue-300 bg-black-300" />
                </div>
            </div>
            <div class="w-[20vw] h-1 mx-auto my-4 bg-black border-0 rounded"></div>
            <div ref="r1">
                {{ date }}
                <div v-if="group != [] || group">
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
                    <h1 v-if="dacsan != ''">Nước + Kem = {{ formatNum(dacsan) }}</h1>
                    <h1 v-if="caphe != ''">Cà Phê = {{ formatNum(caphe) }}</h1>
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
        <div class="flex-grow gap-[20px]" v-if="group != '' && groupTq != []">
            <h1 v-if="report3.carS || report3.carS != 0">{{ report3.carS }} nhỏ = {{ report3.carS*CAR.CAR_S }}</h1>
            <h1 v-if="report3.carM || report3.carM != 0">{{ report3.carM }} trung = {{ report3.carM*CAR.CAR_M }}</h1>
            <h1 v-if="report3.carL || report3.carL != 0">{{ report3.carL }} lớn = {{ report3.carL*CAR.CAR_L }}</h1>
            <div class="w-[20vw] h-1 mx-auto my-4 bg-black border-0 rounded"></div>
            <div ref="r2">
                {{ date }}
                <h1>Chi đầu xe + người + phần trăm hướng dẫn</h1>
                <h1>{{ report2.numCar }} xe = {{ report2.car }}</h1>
                <h1>{{ report2.numPerson }} người = {{ report2.person }}</h1>
                <h1>Phần trăm = {{ report2.percent }}</h1>
                <h1>Tổng chi = {{ report2.total }}</h1>
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
const CAR = {
    CAR_S : 50,
    CAR_M : 70,
    CAR_L : 100
}
const groupTq = ref([])
const r1 = ref(null)
const mes1 = ref('Copy')
const r2 = ref(null)
const mes2 = ref('Copy')
const today = new Date()
const date = String(today.getDate()) + '/' + String(today.getMonth() + 1) + '/' + String(today.getFullYear()) + " :"
const banle = ref("")
const dacsan = ref('')
const caphe = ref('')
const sum = ref(0)
const lumtien = computed(() => {
    return formatNum(Number(dacsan.value) + Number(banle.value) + sum.value + Number(caphe.value))
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
let report3 = ref([])
onMounted(() => {
    function customSort(a, b) {
        return rule.value.indexOf(a.totalBuy) - rule.value.indexOf(b.totalBuy);
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
                car += (Number(group.value[j].carS)+Number(group.value[j].carM)+Number(group.value[j].carL))
                person += Number(group.value[j].person)
                totalBuy += group.value[j].totalBuy
                sum.value += group.value[j].totalBuy
            }
        }
        for (let s = 0; s < groupTq.value.length; s++) {
            if (ruleInfor.value[i].label == groupTq.value[s].typePerson) {
                carTq += (Number(group.value[s].carS)+Number(group.value[s].carM)+Number(group.value[s].carL))
                totalBuyTq += groupTq.value[s].totalBuy
                sum.value += groupTq.value[s].totalBuy
            }
        }
        report1.value.push({ label: ruleInfor.value[i].text, car: car, person: person, totalBuy: formatNum(totalBuy) })
        reportTq.value.push({ label: ruleInfor.value[i].text, car: carTq, totalBuy: formatNum(totalBuyTq) })
    }
    let numCar = 0
    let car = 0
    let carS = 0
    let carM = 0
    let carL = 0
    let numPerson = 0
    let person = 0
    let percent = 0
    let totalz = 0
    for (let k = 0; k < group.value.length; k++) {
        car += (Number(group.value[k].carS*CAR.CAR_S)+Number(group.value[k].carM*CAR.CAR_M)+Number(group.value[k].carL*CAR.CAR_L))
        numCar += (Number(group.value[k].carS)+Number(group.value[k].carM)+Number(group.value[k].carL))
        numPerson += (Number(group.value[k].person) == 0 || group.value[k].numPerson == 0 ? 0 : Number(group.value[k].person))
        carS += Number(group.value[k].carS)
        carM += Number(group.value[k].carM)
        carL += Number(group.value[k].carL)
        person += (group.value[k].numPerson * group.value[k].person)
        percent += group.value[k].percent
    }
    totalz = percent + car + person
    report2.value = { numCar: numCar, car: formatNum(car), numPerson: formatNum(numPerson), person: formatNum(person), percent: formatNum(percent), total:  formatNum(totalz) }
    report1.value = report1.value.sort(customSort)
    report3.value = { carS : carS, carM : carM, carL : carL}
    return report1, report2, sum, report3
})

</script>
<style lang="scss" scoped></style>