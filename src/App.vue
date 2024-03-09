<template>
  <HomePage/>
</template>

<script setup>
import HomePage from './components/HomePage.vue'
const today = new Date()
if(localStorage.getItem('time')==String(today.getDate())+String(today.getMonth() + 1)+String(today.getFullYear())){
  console.log("Same Day!!!")
}
else {
    let k = String(today.getDate())+String(today.getMonth() + 1)+String(today.getFullYear())
    localStorage.setItem('time', k)
    localStorage.removeItem('group')
}
</script>

<style lang="scss">
#app {
  font-family: "Lobster", sans-serif;
  font-weight: 400;
  font-style: normal;
  .logo{
  font-family: "Madimi One", sans-serif;
  }
  -moz-user-select: none !important;
  -webkit-touch-callout: none !important;
  -webkit-user-select: none !important;
  -khtml-user-select: none !important;
  -moz-user-select: none !important;
  -ms-user-select: none !important;
  user-select: none !important;
}
</style>
