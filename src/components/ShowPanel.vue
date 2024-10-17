<template>
        <h1 class="w-full text-[45px] flex justify-center text-[green] cursor-pointer hover:text-[red]" @click="emit('update:dropdown')">
            {{ props.label }}
        </h1>
        <div class="flex flex-row gap-[90px] h-[400px] " v-if="props.dropdown=='' ? props.dropdown : true">
            <div class="flex flex-col gap-[25px]">
                <div>
                    Thẻ :
                    <input
                        :value="props.card"
                        @input="emit('update:card', $event.target.value)"
                        type="text" 
                        class="pl-[10px] [appearance:textfield] [&::-webkit-outer-spin-button]:appearance-none [&::-webkit-inner-spin-button]:appearance-none ml-[20px] w-[300px] h-[50px] rounded border border-2 focus:outline-none focus:ring focus:border-blue-300 bg-black-300" />
                </div>
                <div>
                    <input :value="props.numCar"
                        @input="emit('update:numCar', $event.target.value)" type="number"
                        class="pl-[10px] [appearance:textfield] [&::-webkit-outer-spin-button]:appearance-none [&::-webkit-inner-spin-button]:appearance-none w-[90px] h-[40px] rounded border border-2 focus:outline-none focus:ring focus:border-blue-300 bg-black-300" />
                    Xe :
                    <input :value="props.car"
                        @input="emit('update:car', $event.target.value)" type="number"
                        class="pl-[10px] [appearance:textfield] [&::-webkit-outer-spin-button]:appearance-none [&::-webkit-inner-spin-button]:appearance-none ml-[20px] w-[90px] h-[50px] rounded border border-2 focus:outline-none focus:ring focus:border-blue-300 bg-black-300" />
                </div>
                <div class="flex items-baseline">
                    <input :value="props.person"
                        @input="emit('update:person', $event.target.value)" type="number"
                        class="pl-[10px] [appearance:textfield] [&::-webkit-outer-spin-button]:appearance-none [&::-webkit-inner-spin-button]:appearance-none mr-[20px] w-[75px] h-[50px] rounded border border-2 focus:outline-none focus:ring focus:border-blue-300 bg-black-300" />
                    Người : x
                    <input :value="props.numPerson"
                        @input="emit('update:numPerson', $event.target.value)" type="number"
                        class="pl-[10px] [appearance:textfield] [&::-webkit-outer-spin-button]:appearance-none [&::-webkit-inner-spin-button]:appearance-none ml-[10px] pl-[10px] w-[60px] h-[50px] rounded border border-2 focus:outline-none focus:ring focus:border-blue-300 bg-black-300" />
                    <div class="ml-[20px]">{{ pricePerson }}</div>
                </div>
                <div class='flex flex-col gap-[25px]'>
                    <p>Phần trăm : {{ props.percent }} </p>
                    <p>Tx : <input v-model="percentTx" type="number"
                        class="pl-[10px] [appearance:textfield] [&::-webkit-outer-spin-button]:appearance-none [&::-webkit-inner-spin-button]:appearance-none ml-[20px] w-[120px] h-[50px] rounded border border-2 focus:outline-none focus:ring focus:border-blue-300 bg-black-300" />
                    </p>
                    <p>Hd : <input v-model="percentHd" type="number"
                        class="pl-[10px] [appearance:textfield] [&::-webkit-outer-spin-button]:appearance-none [&::-webkit-inner-spin-button]:appearance-none ml-[20px] w-[120px] h-[50px] rounded border border-2 focus:outline-none focus:ring focus:border-blue-300 bg-black-300" />
                    </p>
                    <p>Td : <input v-model="percentTd" type="number"
                        class="pl-[10px] [appearance:textfield] [&::-webkit-outer-spin-button]:appearance-none [&::-webkit-inner-spin-button]:appearance-none ml-[20px] w-[120px] h-[50px] rounded border border-2 focus:outline-none focus:ring focus:border-blue-300 bg-black-300" />
                    </p>
                </div>
                <div>
                    Tổng chi : {{ total }}
                </div>
            </div>
            <div class="flex flex-col justify-end flex-grow-1 ">
                <div class="flex flex-row items-baseline">
                    Khách
                    <input :value="props.typePerson"
                        @input="emit('update:typePerson', $event.target.value)" type="text"
                        class="[appearance:textfield] [&::-webkit-outer-spin-button]:appearance-none [&::-webkit-inner-spin-button]:appearance-none ml-[20px] pl-[10px] w-[60px] h-[70px] rounded border border-2 focus:outline-none focus:ring focus:border-blue-300 bg-black-300" />
                </div>
                <div class="flex flex-grow justify-center items-center">
                    Tổng mua : 
                    <input :value="props.totalBuy"
                        @input="emit('update:totalBuy', $event.target.value)" type="number"
                        class="[appearance:textfield] [&::-webkit-outer-spin-button]:appearance-none [&::-webkit-inner-spin-button]:appearance-none ml-[20px] pl-[10px] w-[150px] h-[60px] rounded border border-2 focus:outline-none focus:ring focus:border-blue-300 bg-black-300" />
                </div>
            </div>
        </div>
        <slot></slot>
</template>
<script setup>
import { defineProps,defineEmits,computed,ref } from 'vue';
const props = defineProps(['label','car','card','person','numPerson','numCar','totalBuy','typePerson','percent','index','dropdown'])
const emit = defineEmits(['update:car','update:card','update:person','update:numPerson','update:numCar','update:totalBuy','update:typePerson','update:percent','update:dropdown'])

const percentTx = ref(0)
const percentHd = ref(0)
const percentTd = ref(0)

const percent = computed (()=>{
    return Number(percentTx.value)+Number(percentHd.value)+Number(percentTd.value)
})
watch(percent,(now,last)=>{
    props.percent = now
})
const pricePerson = computed(()=>{
    return Number(props.person)*Number(props.numPerson)
})
const total = computed(() => {
    return Number(props.car) + Number(props.person)*Number(props.numPerson) + percent.value
})
</script>
<style>
</style>